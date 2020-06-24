# DJSON

DJSON is short for dynamic json. This allows for json files to be generated during runtime. 

```js
ccmod3.resources.dynamicJSONFiles.add('data/test.json', function() {
    return {};
});
```

When ever the game tries to fetch `assets/data/test.json`, `{}` will be return even if the file does not exist on the filesystem.


There is support for asynchronously callbacks.

```js
ccmod3.resources.dynamicJSONFiles.addAsync('data/test.json', async function() {
    const jsonData = await fetch('/assets/data/test.json').then(e => e.json());
    jsonData.f = 2;
    return jsonData;
});
```

This will get the original file. Add f property  with the value 2 to it and return the result. 
Because DJSON only works for $.ajax calls, fetch can be used to retrieve the original file and modify it.

Note: There can be only one handler for a given resource.