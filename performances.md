# Browser Performances

## How browser works
First, read [this](https://gist.github.com/paulirish/5d52fb081b3570c81e3a) and every its references below.   

## Advices

- Force layout/reflow only when it's needed and not every frame. The browser has to recalculate all page layout.

```javascript
// BAD
function update() {
	// trigger layout/reflow every frame
	const rect = node.getBoundingClientRect()
	//process
	requestAnimationFrame(update)
}

requestAnimationFrame(update)
```

```javascript
// GOOD

let rect

function resize() {
	// trigger layout/reflow on window resize and cache the value
	rect = node.getBoundingClientRect()
}

resize()
window.addEventListener('resize',resize,false)

function update() {
	//process
	requestAnimationFrame(update)
}

requestAnimationFrame(update)
```
---
- Reduce the amount of DOM elements, avoid using unnecessary elements. The more elements you have the more time the browser will take to recalculate.
```html
// BAD
<div class="accordion">
	<div class="accordion__head">
		<div>
			<p>title<p>
		<div>
	<div>
	<div class="accordion__content">
		<div>
			<p>content<p>
		<div>
	<div>
</div>
```

```html
// GOOD
<div class="accordion">
	<div class="accordion__head">
		title
	<div>
	<div class="accordion__content">
		content
	<div>
</div>
```
---
- Do not update React state too often, only render it when it's needed. Every time a state change it will render the component and all its children. You also can use `useMemo` to avoid rendering. Put `console.log('update')` above the return to see when the component is rendered. 

```javascript
// BAD
const Cursor = () => {
	const [mouse,setMouse] = useState({x=0,y=0})
	
	function onMouseMove({pageX,pageY}) {
		// render component every mousemove events
		setMouse({x:pageX,y:pageY})
	}

	useLayoutEffect(()=>{
		window.addEventListener('mousemove',onMouseMove,false)
	},[])

	console.log('update')

	return <div style={"transform":`translate3d(${x}px,${y}px,0)`}></div>
}
```

```javascript
// GOOD
const Cursor = () => {
	const ref = useRef()
	
	function onMouseMove({pageX,pageY}) {
		// render component once
		ref.current.style.setProperty('transform',`translate3d(${pageX}px,${pageY}px,0)`)
	}

	useLayoutEffect(()=>{
		window.addEventListener('mousemove',onMouseMove,false)
	},[]) 

	console.log('update')
	
	return <div ref={ref}></div>
}
```
---
- Prefer using CSS transition than Spring when it's possible. Animation libs such as GSAP or react-spring applies style every frame on the targeted elements. It triggers layout/reflow every frame while toggle a class/style only triggers layout/reflow once.

```javascript
// BAD
const Fade = ({children, active}) => {
	const [style] = useSpring(()=>({
		opacity: active ? 1 : 0
	}),[active])

	return (
		<a.div style={style}>
			{children}
		</a.div>
	)
}
```

```javascript
// GOOD
const Fade = ({children, active}) => {
	return (
		<div style={"opacity": active ? 1 : 0, "transition": "opacity 1s ease-out"}>
			{children}
		</div>
	)
}
```
---
- Only use one and only requestAnimationFrame. See performances benchmark [here](https://perf.link/#eyJpZCI6Ino5OWlwNHFvb2t6IiwidGl0bGUiOiJGaW5kaW5nIG51bWJlcnMgaW4gYW4gYXJyYXkgb2YgMTAwMCIsImJlZm9yZSI6ImNvbnN0IGNhbGxiYWNrMSA9ICgpPT57fVxuY29uc3QgY2FsbGJhY2syID0gKCk9Pnt9XG5jb25zdCBjYWxsYmFjazMgPSAoKT0%2Be30iLCJ0ZXN0cyI6W3sibmFtZSI6IlRlc3QgQ2FzZSIsImNvZGUiOiJjb25zdCBjYWxsYmFjayA9ICgpPT57Y2FsbGJhY2sxKCk7Y2FsbGJhY2syKCk7Y2FsbGJhY2szKCl9XG5yZXF1ZXN0QW5pbWF0aW9uRnJhbWUoY2FsbGJhY2spXG4iLCJydW5zIjpbMzAwMDAwLDQ0NzAwMCw2MzAwMCwyNjkwMDAsNTcwMDAsMTAwMCwxNDIwMDAsMTAwMCwxMDAwLDE1NzAwMCwyNTcwMDAsODcwMDAsNDkwMDAsMTQwMDAwLDE1NzAwMCw2MTAwMCwzOTAwMCwxODUwMDAsMTAwMCw0MTkwMDAsMzAyMDAwLDMwMDAwLDMxMDAwLDE4MjAwMCwyOTMwMDAsMTA2MDAwLDEwMDAsNTQwMDAsNzQwMDAsMTI5MDAwLDEzNjAwMCwyNTgwMDAsMTEyMDAwLDM2MDAwMCwyOTIwMDAsMjIwMDAsMzY1MDAwLDIzMDAwMCw1ODAwMCwxMDAwLDEwMDAwMCw3NTAwMCwxODMwMDAsMTI2MDAwLDE0MDAwMCwyMzIwMDAsMTEyMDAwLDI5ODAwMCw5MDAwLDEwMDAsMTUwMDAsMjA3MDAwLDI1MDAwLDEwMDAsMTcwMDAwLDI5NTAwMCwxMDYwMDAsNDU2MDAwLDY3MDAwLDIyMDAwLDkwMDAsNDY1MDAwLDMwOTAwMCwyNzUwMDAsMjUwMDAsODAwMDAsMTg0MDAwLDk3MDAwLDI3MjAwMCw1ODAwMDAsMzAwMDAsMjIwMDAwLDQzODAwMCwyMzMwMDAsMTY1MDAwLDUwMzAwMCwyMDYwMDAsMjgzMDAwLDE2MTAwMCwyMDIwMDAsMTcwMDAwLDE1NDAwMCwxNDAwMCwxMDAwMCwxNjgwMDAsMjE5MDAwLDIwMDAsNjgwMDAsMTAwMCwxMjAwMCw5MjAwMCwxMDAwLDEwMTAwMCw5NjAwMCwzNTcwMDAsMTIzMDAwLDEwMDAsMjAwMCwzMzkwMDAsMjc5MDAwXSwib3BzIjoxNTQ1NTB9LHsibmFtZSI6IlRlc3QgQ2FzZSIsImNvZGUiOiJyZXF1ZXN0QW5pbWF0aW9uRnJhbWUoY2FsbGJhY2sxKVxucmVxdWVzdEFuaW1hdGlvbkZyYW1lKGNhbGxiYWNrMilcbnJlcXVlc3RBbmltYXRpb25GcmFtZShjYWxsYmFjazMpIiwicnVucyI6WzEyMjAwMCwxMDAwLDE5NzAwMCwyOTAwMCwyNTAwMCw1MTAwMCwxNjEwMDAsMTAwMCwxMDAwLDY0MDAwLDEwODAwMCw2MjAwMCwxMDAwLDc0MDAwLDEwMDAsNjIwMDAsOTAwMCwxMDAwLDE1MDAwLDEzMjAwMCwzMjAwMCwxMTAwMCw1MTAwMCw5MzAwMCw0OTAwMCw3MjAwMCwxMDAwLDM5MDAwLDY5MDAwLDIyMDAwLDYzMDAwLDE0MzAwMCwxMDAwLDQ4MDAwLDMzMDAwLDI0MDAwLDE0OTAwMCw0MDAwLDIwMDAwLDEwMDAsNTQwMDAsNDMwMDAsMTAwMCw3MDAwLDkzMDAwLDcwMDAsMTAwMCwxMTgwMDAsMzIwMDAsMTAwMCw0NTAwMCwxMTAwMCwyMTAwMCw4NzAwMCw3NzAwMCwxMjIwMDAsMTAwMCwzMDAwMCwxMzAwMCwxMTAwMCw4NzAwMCwyMTgwMDAsMjQwMDAsMTM3MDAwLDYwMDAwLDQzMDAwLDEwMDAsMTAwMCwxMDAwLDE0NTAwMCw2NTAwMCwxMDAwLDEyNTAwMCw1MDAwLDE4MzAwMCw2MjAwMCwxMzUwMDAsODkwMDAsMzAwMDAsMTUxMDAwLDYyMDAwLDgyMDAwLDExMTAwMCwxMDAwLDcyMDAwLDIwMDAsMjAwMCw4NjAwMCwxMDAwLDg2MDAwLDEwMDAsODQwMDAsMjAwMCw0ODAwMCw4NzAwMCwxMzIwMDAsNzcwMDAsMjQwMDAsMTY5MDAwLDE1NTAwMF0sIm9wcyI6NTY2NDB9XSwidXBkYXRlZCI6IjIwMjEtMTAtMDhUMTQ6MTc6NDkuMTYxWiJ9). 
```javascript
// BAD
function update() {
	// process
	requestAnimationFrame(update)
}

requestAnimationFrame(update)
```

```javascript
import { raf } from  '@react-spring/rafz'

// GOOD
function update() {
	// process
	return true
}

raf.onFrame(update)
```
---
-   Leverage hardware acceleration on costly CSS operations. Add `will-change: transform` or `transform: translateZ(0)`.

```javascript
// BAD
function onMouseMove({pageX,pageY}) {
	// no hardware acceleration
	node.style.setProperty('transform',`translate(${pageX}px,${pageY}px)`)
}
	
window.addEventListener('mousemove',onMouseMove,false)
```

```javascript
// GOOD
function onMouseMove({pageX,pageY}) {
	// translate3d leverage hardware acceleration
	node.style.setProperty('transform',`translate3d(${pageX}px,${pageY}px,0)`)
}
	
window.addEventListener('mousemove',onMouseMove,false)
```
---
- Prefer animation optimized CSS properties as `opacity` or `transform`. See [here](https://csstriggers.com/).
```javascript
// BAD
function onMouseMove({pageX,pageY}) {
	// animating top is costly
	node.style.setProperty('left',`${pageX}px`)
	node.style.setProperty('top',`${pageY}px`)
}
	
window.addEventListener('mousemove',onMouseMove,false)
```
```javascript
// GOOD
function onMouseMove({pageX,pageY}) {
	// animating transform is cheap
	node.style.setProperty('transform',`translate3d(${pageX}px,${pageY}px,0)`)
}
	
window.addEventListener('mousemove',onMouseMove,false)
```
---
- Use your browser devtools performances tab to figure out costly operations. Keep in mind to run 60fps, a browser frame execution duration shouldn't exceed 16ms.
