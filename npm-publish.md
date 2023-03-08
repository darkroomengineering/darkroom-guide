## Make sure the following scripts are present in your `package.json`:

```json
    "prepublishOnly": "npm version patch",
    "postpublish": "git push --follow-tags",
    "preversion": "npm run build",
```

This will take care of NPM version and github versions too.


### If they are you should be able to just

- make the change
- git push
- npm publish
