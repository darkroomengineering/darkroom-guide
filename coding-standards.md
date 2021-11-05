# Coding Standards 

- Use lowercase and kebab-case for EVERY file, even hooks:

```javascript
// BAD
RealViewport
```

```javascript
// GOOD
real-viewport
```
- Do not export as default:

```javascript
// BAD
const test = () => ()
export default test

//Then 
import test from 'components/test'
```

```javascript
// GOOD
export const test = () => ()

//then
import { test } from 'components/test'
```
- Use CMS folder for all CMS related files:

```javascript
// BAD
lib/contenful/renderer.js
```

```javascript
// GOOD
contentful/home-renderer.js
```
- Use /assets folder with assets/icons for icons and assets/illustrations for illustrations duh:

```javascript
// BAD
illustrations/wizard.svg
lib/arrow.svgslider/arrow.svg
```

```javascript
// GOOD
assets/illustrations/wizard.svg
assets/icons/arrow.svg
```
- Don't attach ui elements to components

```javascript
// BAD
//in components/slider/index.js:
import arrow from './arrow.svg'
```

```javascript
// GOOD
//in the page where slider is being imported:
import { Slider } from 'components/slider'
import Arrow from 'assets/icons/arrow.svg'
```
- Add a render validation for rich text or must be set as required on contentful

```javascript
// BAD
<div>
	{renderBody(firstBodyContent.content)}
</div>
```

```javascript
// GOOD
<div>
  {firstBodyContent.content && renderBody(firstBodyContent.content)}
</div>
```
- When using graphql not add "|| null" in getStaticProps, graphql already does that

```javascript
// BAD
const data = {briefDescription: item?.briefDescription || null}
```

```javascript
// GOOD
const data = {briefDescription: item.briefDescription}
```
- In getStaticProps manage arrays with validation and return empty array if is null

```javascript
// BAD
export const getStaticProps = async ({ preview = false }) => {
  const imagesArray = data?.imagesArray ? 
  data.imagesArray.map((item) => ({image: item.src}) 
  : null   return imagesArray
}
 
const MyRender = ({imagesArray}) => {
  return (
    <div>
      {imagesArray && imagesArray.map( ...}
    </div>
  )
}

```

```javascript
// GOOD
export const getStaticProps = async ({ preview = false }) => {
   const imagesArray = data?.imagesArray ? 
   data.imagesArray.map((item) => ({image: item.src}) 
   : []
  return imagesArray
}

const MyRender = ({imagesArray}) => {
  return (
    <div>
      {imagesArray.map( ...}
    </div>
  )
}
```
- Use mobile css as default

```css
// BAD
.my-class{
  position: relative;
  top: 50%;
  @include mobile{
    top:unset 
  }
}

```

```css
// GOOD
.my-class{
  position: relative;
  @include desktop{
    top: 50%;
  }
}

```
