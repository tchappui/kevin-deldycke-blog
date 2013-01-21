comments: true
date: 2011-09-06 12:32:09
layout: post
slug: how-to-fix-flash-kubuntu
title: How-to fix Flash on Kubuntu
wordpress_id: 3890
category: English
tags: adobe, apt-get, flash, KDE, kpackagekit, kubuntu, Linux, package, plugin, Web

If like me you want the greatest and latest Flash version on your Kubuntu, you probably added the [SevenMachine's repository](http://launchpad.net/~sevenmachines/+archive/flash) to your sources. Else, you should, as it's where you'll find all the fresh Flash packages, for both 32 bits and 64 bits architectures.

Everything will be great after that. Until the day this repository is updated, which will break the Flash plugin if you attempt an upgrade with KPackageKit.

[![](http://kevin.deldycke.com/wp-content/uploads/2011/09/kpackagekit-flash-300x174.png)](http://kevin.deldycke.com/wp-content/uploads/2011/09/kpackagekit-flash.png)

The Flash package does not contain the binary plugin itself, but is just an empty shell which download the plugin from the web to your machine. And for this operation to work as expected, the package need to be in a terminal environment.

To fix this, you'll have to remove all previous Flash package (as a prevention measure), then install the latest with the command line. Here are the commands to do exactly this:


    :::console
    sudo apt-get remove --purge flashplugin-installer flashplugin64-installer
    sudo apt-get clean
    sudo apt-get update
    sudo apt-get install flashplugin64-installer




Commands above are for a 64 bits distribution. If you're still running a 32 bits Linux, just replace the last reference of `flashplugin64-installer` by `flashplugin-installer`.

Now restart your browser and YouTube videos and other Flash stuff should work again.