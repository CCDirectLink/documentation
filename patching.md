# What is patching?

In the scope modding, patching is changing the original game content in any way.


# How do I patch game files? 

There is more than one way to patch the game. This page will walk you through the various ways to patch.

<hr>

#  Asset Replacement

This is the most basic type of patching (anyone can do it).

This is a standard modloader feature that allows you to replace any file the game loads with your own.

## Process

Inside your mod folder, create a folder called `assets`.

![Image](/assets/patching/images/assets-folder.png?raw=true)

Decide what file you want to replace. For this tutorial, I will be showing you how to replace Lea's portraits.

Find the original relative path to asset of interest.

![Image](/assets/patching/images/lea-face-path.png?raw=true)

All you have to do is now mimic the folder structure.

In this example, two folders need to be created.

Inside the `assets` folder in your mod, create a `media` folder. Inside the `media` folder, create a `face` folder. 

Finally, add the file you want to replace (your own `lea.png`).

It should end up looking something like this.

![Image](/assets/images/patching/faceless-mod-path-example.png?raw=true)

![Image](/assets/patching/images/faceless-mod-folder-contents-example.png?raw=true)

That is it! 


You can find more examples in the [CCAssetsSwaps](https://github.com/CCDirectLink/CCAssetSwaps/) repo.

* Used faceless Lea mod for some of the demonstration

# JSON Patching (Object Patching)

Note: This requires some know of the JSON format and some programming concepts

As the majority of the game assets are json files, this is really useful to know about.

What this allows you to do is modify any json file without doing a file replacement.

The same ideas apply as assets override, except the file will have `.patch` added to the full file name.

You need to have file extensions visible in order to modify it.

## Process

Let's say you want to patch a file `assets/data/test.json` with this as the contents:

```js
{
    "storage" : {
        "level": 100,
        "xp": 500
    }
}
```

Your mod is located at `assets/mods/my-mod/`

You would create a file with the filepath `assets/mods/my-mod/assets/data/test.json.patch`

To replace "storage.level" with 200, `test.json.patch` would look like this:

```js
{
    "storage" : {
        "level": 200
    }
}
```

To add "storage.name":

```js
{
    "storage" : {
        "name": "Shizuka"
    }
}
```

Object patching is useful for adding new properties and modifying existing ones.

Weakness:
- Inability to removal properties
- Arrays are hard to patch