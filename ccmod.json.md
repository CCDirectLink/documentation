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

### id (required)
The mod ID is a unique name used to refer to a mod. It is a string containing one or more alphanumeric (in other words, lowercase/uppercase Latin letters and numbers) characters, dashes (-), and underscores (_). 

Suggestions:
* Don't include the prefix crosscode or cc, unless a generic term follows (e.g. if you remove the cc prefix from CCLang you are left with Lang, or in the case of crosscode-ru you are left with ru).
* Use kebab-case instead of camelCase, PascalCase or snake_case when using multiple words. kebab-case is a de facto standard for package names in virtually every Linux distribution I've seen so far, and is considered easier to type because it doesn't require holding the Shift key.


### version

```js
{
  "version": "0.1.0"
}
```
A Semver compliant string identifiying the version of your mod. This field can be ommitted. Its default default value is "0.0.0". If you intend to work on your mod over a period of time, it is recommended to frequently update the version. If an update is not backwards compatible then change the major version (e.g 1.0.0 -> 2.0.0), if an update is backwards compatiable then change the minor version (e.g. 1.1.0 -> 1.2.0), if an update only fixes an issue in a backwards compatabile matter then change the patch version (e.g 1.1.0 -> 1.1.1). 

Suggestions:
* Use 1.0.0 to signify a stable release.
* Use anything below 1.0.0 to signify early development.

### title

```js
{
  "title": "My mod"
}
```


or


```js
{
  "title": {
    "en_US": "My mod"
  }
}
```

This is what is displayed to the user. Keep it short and descriptive. When this field is not set, it defaults to the mod id.

### description


```js
{
  "description": "My cool mod!"
}
```

or


```js
{
  "description": {
    "en_US": "My cool mod!"
  }
}
```


A short description of your mod describing its purpose. Long descriptions may cause UI bugs.

### license


```js
{
  "license": "MIT"
}
```

The license of your project. 

### homepage

```js
{
  "homepage": "https://github.com/CCDirectLink/documentation/"
}
```

or


```js
{
  "homepage": {
    "en_US": "https://github.com/CCDirectLink/documentation/"
  }
}
```

A url to a mod's homepage.

### keywords 

```js
{
  "keywords": ["example", "mod"]
}
```

or


```js
{
  "keywords": [{
    "en_US": "example"
  }, {
    "en_US": "mod"
  }]
}
```

A list of keywords to tag your mod for searching purposes.

### authors

```js
{
  "authors": [{
    "name": "Solo Developer",
    "email": "solo@best!-developer.com",
    "url": "https://solo.best!-developer.com/",
    "comment": "Super cool!"
  }]
}
```

or 


```js
{
  "authors": [{
    "name": {
      "en_US": "Solo Developer"
     },
    "email": {
      "en_US": "solo@best!-developer.com"
    },
    "url": {
      "en_US": "https://solo.best!-developer.com/"
     },
    "comment": {
      "en_US": "Super cool!"
     }
  }]
}
```

List of mod contributors information.


### dependencies

```js
{
  "dependencies": {
    "cool-mod": "1.0.0"
  }
}
```

or


```js
{
  "dependencies": {
    "cool-mod": {
      "version": "1.0.0",
      "optional": true
    }
  }
}
```

A list of external dependencies that your mod requires to work. 


### assets

```js
{
  "assets": [
    "path/to/file.json"
  ]
}
```

A list of all assets in your mod. This is typically used for mod browser support.


### assetsDir

```js
{
  "assetsDir": "assets"
}
```

A relative path to your assets directory as the mod folder as the root.



### plugin

 ```js
{
  "plugin": "plugin.js"
}
```

First of all, a short note before I explain what this specific field does. Firstly, all scripts are loaded as ES modules, which allows the usage of import and export statements, enables strict mode and does a few other things (more info can be seen here). More importantly, all scripts are imported and executed in the mod load order, see dependencies for more info.

The `plugin` field is basically what makes mods powerful. It allows you to specify path to a JavaScript script that must export a so-called `plugin` class of your mod (or a mod delegate if you prefer that) with export default. I'll explain what this class is supposed to do in a second. This script is imported very early during the game loading process (to be precise, right after the libraries required by the game, such as JQuery, are loaded) and immediately after it is read, before moving on to the next mod, the constructor of that `plugin` class is called.

### preload

 ```js
{
  "preload": "preload.js"
}
```



### postload

 ```js
{
  "postload": "postload.js"
}
```


### prestart

 ```js
{
  "prestart": "prestart.js"
}
```


### poststart

 ```js
{
  "poststart": "poststart.js"
}
```


### Localized strings

Localized strings are regular string values (hence the "string" part in their name), but they optionally allow localization into different languages (hence the "localized" part). Localization allows the user to view the information about the mod in their preferred language, e.g. by selecting the language in the options menu of CrossCode or the mod portal. 

Normally localized strings are written as just plain strings, but when you want to translate that into different languages you have to use a dictionary where keys are locale names and values are the respective translations for a given locale, for example:

without localization:

```js
{
  "id": "localize-me",
  "title": "Localize Me",
  "description": "Add support for more locales, languages and translations",
  ...
}
```
with localization:
```js
{
  "id": "localize-me",
  "title": {
    "en_US": "Localize Me",
    "fr_FR": "Régionalisez-Moi",
    "de_DE": "Lokalisiert mich"
  },
  "description": {
    "en_US": "Add support for more locales, languages and translations",
    "fr_FR": "Ajoute la gestion de plus de locales, de languages et de traductions",
    "de_DE": "Fügt Unterstützung für mehr Lokalen, Sprachen und Übersetzungen hinzu",
    "ru_RU": "Мод для создания дополнительных региональных настроек, языков и переводов"
  },
  ...
}
```

The locale names are usually strings in the format `<language>_<territory>`. Here are the locales provided by CrossCode:

* `en_US` - American English (the default locale)
* `de_DE` - German
* `ja_JP` - Japanese
* `ko_KR` - Korean
* `zh_CN` - Simplified Chinese
* `zh_TW` - Traditional Chinese

It should be noted what I mean by "the default locale": when a translation for locale selected by the user isn't specified, the translation for `en_US` will be chosen instead, so it's recommended to provide at least the English variant of all localized strings. It is also assumed that when a plain string is used instead of a dictionary, this string contains text for the `en_US` locale. In other words using plain strings is equivalent to writing { "en_US": "<text>" }.

You are not limited to just the locales listed above, however. You can specify translation for any locale, and depending on the platform which processes the manifest, more locales can be viewed, e.g. the mod portal can easily support more languages. Plus, there are translation mods for CrossCode, so, e.g. `fr_FR` (French) and `ru_RU` (Russian) can be viewed as well. Be careful when using locale-specific characters though: text rendering in CC is very simple and the bitmap fonts don't support every Unicode character out there.

### File paths

This should go without saying, but file paths specified in the mod manifest are relative to the mod's directory. This means that the mod is independent of the name and path of the directory it was installed in, unless, of course, it contains any hardcoded references to it.

All paths must use forward slashes (/) for separating directory and file names, and not backslashes (\) like MS Windows does. The reason for that is that forward slashes are canonically used as separators in URLs. Actually, backslashes shouldn't be used at all because using them results in different ("undefined" in programming terms) behavior across different platforms: on Windows they'll be treated as directory name separators, and everywhere else they will be treated as a part of a single name. ( NOTE: this may change in the future, as I was planning to automatically replace all backslashes with forward slashes in the modloader code, but even that doesn't mean that you should use backslashes)

In addition to that all paths are "jailed" inside the mod's directory: this means that you won't be able to "escape" the mod's directory by prepending a bunch of ../ or a leading / - those components stripped off, which makes the whole path relative, and then it is finally resolved by being appended to the mod's directory.
