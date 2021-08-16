# ccmod.json
`ccmod.json` is the new manifest file that replaces the soon-to-be deprecated `package.json` normally associated with a crosscode mods.

## Motivation

`package.json` is normally associated with nodejs. For this reason, creating a new format was necessary to avoid conflicts from properties that both CCLoader and nodejs used.

## Format
Bare minimum
```js
{
  "id": "my-mod"
}
```
