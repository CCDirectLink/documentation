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
        "level": 200,
        "saves": [{
            "data": "blob"
        }]
    }
}
```

to 

```js
{
    "level": 200,
    "saves": [{
        "data": "blob"
    }]
}
```



```js
[{
    "type": "ENTER",
    "index": ["storage", "saves", 0]
}]
```

would change the internal view of: 
```js
{
    "storage" : {
        "level": 200,
        "saves": [{
            "data": "blob"
        }]
    }
}
```

to 

```js
{
    "data": "blob"
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


### INIT_KEY

This is the contents of `assets/data/test.json`:

```js
{
    "storage": {
        "user": {
            "name": 20
        }
    }
}
```

This is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "INIT_KEY",
    "index": "storage",
    "content": {
        "a": 2
    }
}]
```

Nothing will change to the data because "storage" already exists.

Changing `assets/mods/my-mod/assets/data/test.json.patch` to:

```js
[{
    "type": "INIT_KEY",
    "index": "storage2",
    "content": {
        "a": 2
    }
}]
```

would result in:


```js
{
    "storage": {
        "user": {
            "name": 20
        }
    },
    "storage2": {
        "a": 2
    }
}
```

### REMOVE_ARRAY_ELEMENT


This is the contents of `assets/data/test.json`:

```js
{
    "a": [1,2,3]
}
```


This is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "REMOVE_ARRAY_ELEMENT",
    "index": 0
}]
```

Applying the patch will result in this:

```js
{
    "a": [2,3]
}
```

### ADD_ARRAY_ELEMENT

This is the contents of `assets/data/test.json`:

```js
{
    "a": [1,2,3]
}
```

This is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "ENTER",
    "index": "a"
},{
    "type": "ADD_ARRAY_ELEMENT",
    "content": 4
}, {
    "type": "ADD_ARRAY_ELEMENT",
    "index": 0,
    "content": 0
}, {
    "type": "EXIT"
}]
```


After the first step, the internal state will be:

```js
[1,2,3]
```

After the second step, the number 4 will be pushed to the end of the array:

```js
[1,2,3,4]
```

After the third step, the number 0 will be added to the start of the array:

```js
[0,1,2,3,4]
```

The last step will revert the value of focus to the initial object:

```js
{
    "a": [0,1,2,3,4]
}
```

### Import

This is the contents of `assets/data/test.json`:

```js
{
    "a": {
        "b": 2
    }
}
```

This is the contents of `assets/data/test2.json`:

```js
{
    "b": {
        "a": 2
    },
    "c": {
        "a": 3
    },
    "d": 4
}
```

This is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "IMPORT",
    "src": "data/test2.json"
}]
```

This is equivalent to:

```js
[{
    "type": "IMPORT",
    "src": "game:data/test2.json"
}]
```

The first step would load `assets/data/test2.json` then merge it with the current value resulting in this:

```js
{
    "a": {
        "b": 2
    },
    "b": {
        "a": 2
    },
    ,
    "c": {
        "a": 3
    },
    "d": 4
}
```


If `assets/data/test2.json` had patches associated with it, it would be applied before being merged.

Another feature is loading json files inside the mod directory and merging. 
No patches to the loaded file will be done if `mod:` is specified.

This is the contents of `assets/data/test.json`:

```js
{
    "a": {
        "b": 2
    }
}
```


This is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "IMPORT",
    "src": "mod:patches/data/test.json",
    "path": ["1", "2", "3"],
    "index": "y"
}]
```

This is the contents of `assets/mods/my-mod/patches/data/test.json`:

```js
{
    "1": {
        "2": {
            "3": 4
        }
    }
}
```


First it will load `assets/mods/my-mod/patches/data/test.json`.

Then it will walk the path specified.

Initial loaded file internal representation:

```js
{
    "1": {
        "2": {
            "3": 4
        }
    }
}
```

After entering "1":

```js
{
    "2": {
        "3": 4
    }
}
```

After entering "2":

```js
{
    "3": 4
}
```

After entering "3":

```js
4
```

Finally, it will add a key "y" with the value that resulted from the path walk (4).

So the overall result will be:

```js
{
    "a": {
        "b": 2
    },
    "y": 4
}
```

Note: "index" and "path" options are optional. 

### Include

The purpose of `INCLUDE` is to allow external Object Patching or PatchSteps to be performed. 


This is the contents of `assets/data/test.json`:

```js
{
    "a": {
        "b": 2
    }
}
```


This is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "INCLUDE",
    "src": "mod:patches/data/test.json",
}]
```

This is the contents of `assets/mods/my-mod/patches/data/test.json`:

```js
{
    "1": {
        "2": {
            "3": 4
        }
    }
}
```

This is like setting the contents of `assets/mods/my-mod/assets/data/test.json.patch` to:

```js
{
    "1": {
        "2": {
            "3": 4
        }
    }
}
```

However, `INCLUDE` is used as an organization method.


This is the contents of `assets/data/test.json`:

```js
{
    "a": {
        "b": 2
    }
}
```


This is the contents of `assets/mods/my-mod/assets/data/test.json.patch`:

```js
[{
    "type": "INCLUDE",
    "src": "mod:patches/data/test.json",
}, {
    "type": "SET_KEY",
    "index": "b",
    "content": [1,2,3]
}]
```

This is the contents of `assets/mods/my-mod/patches/data/test.json`:

```js
[{
    "type": "ENTER",
    "index": ["a"]
}]
```

This results in:
```js
{
    "a": {
        "b": 2
    },
    "b": [1,2,3]
}
```

Think of PatchStep files as its own room. What include does is generate a new room to perform some special operation.
Rooms do not intefere with each other, they just modify the data. 