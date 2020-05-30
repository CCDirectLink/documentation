# What is a patching?

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

