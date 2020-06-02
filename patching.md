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


# Patch Steps

Patch Steps is another way to perform json file Patching. It is more powerful than Object Patching. 

The only difference between Object Patching and Patch Steps is the format of the `.patch` file. 

We will be using the same sample json file used to demonstrate how Object Patching works.

Location: `assets/data/test.json`
```js
{
    "storage" : {
        "level": 200
    }
}
```

## Appliers

An Applier is a Patch Step command.


## Patch Step Appliers

```js
```

### Enter

This changes the context of a patch. 

```js
[{
    "type": "ENTER",
    "index": ["storage"]
}]
```

or 

```js
[{
    "type": "ENTER",
    "index": "storage"
}]
```

would change the internal view of:
```js
{
    "storage" : {
        "level": 200
    }
}
```

to 

```js
{
    "level": 200
}
```


### Exit

For every `ENTER`, there should be at least one `EXIT`.

`EXIT` undos an `ENTER`.

```js
[{
    "type": "EXIT"
}]
```

is equivalent to

```js
[{
    "type": "EXIT",
    "count": 1
}]
```


For demonstration purposes, this is the contents of `assets/data/test.json`:

```js
{
    "storage": {
        "user": {
            "name": 20
        }
    }
}
```

and this is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "ENTER",
    "index": ["storage", "user"]
}, {
    "type": "EXIT",
    "count": 2
}]
```

The first step will produce the internal result of:

```js
{
    "name": 20
}
```

The second step will undo the first leading to the initial state.

```js
{
    "storage": {
        "user": {
            "name": 20
        }
    }
}
```