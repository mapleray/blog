---
title: Install TrueCrypt On Mac OS X Yosemite 10.10
tags:
  - mac
permalink: install-truecrypt-on-mac-osx-yosemite-10-10
categories:
  - Mac
date: 2014-10-19 00:00:00
---


Yesterday, I upgraded my MacBook OS to Yosemite 10.10. But when I reinstall [Truecrypt](http://en.wikipedia.org/wiki/TrueCrypt)(For some reasons, recommend Truecrypt 7.1a version), something get error.

![TrueCrypt-Error](/images/truecrypt-install-error.png)

Guess the OS X version numbers. Truecrypt thinks 10.10 is < 10.4 (the minimum Truecrypt requires), and the installer will block it's installation on Yosemite. So you'll be able to install it again on 10.40...or maybe, possibly 10.11 or later.

In the end, I found two solutions:


First method
------
* Open the .dmg
* You’ll find the .mpkg. Right*click and “Show Package Contents”
* Open Contents Dir
* Open Packages Dir
* Install each of the 4 packages in this order: OSXFUSECore.pkg, OSXFUSEMacFUSE.pkg, MacFUSE.pkg, TrueCrypt.pkg (It is possible MacFUSE.pkg will install the two before it, but we ran each to play it safe.).

That’s it; it’s Truecrypt has been working fine for us using this method.

Second method
------


* Open the .dmg
* You’ll find the .mpkg. Right*click and “Show Package Contents”
* Open Contents Dir
* Edit Contents/distribution.dist using Text Editor
* You'll find the code as below

		function pm_install_check() {
		  if(!(system.version.ProductVersion >= '10.4.0')) {
		    my.result.title = 'Error';
		    my.result.message = 'TrueCrypt requires Mac OS X 10.4 or later.';
		    my.result.type = 'Fatal';
		    return false;
		  }
		  return true;
		}

* change it as follow:

		function pm_install_check() {
		  return true;
		}

Now, you can install  .mpkg  without error.
