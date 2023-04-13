---
title: Undefined type 'Spatie\Permission\Models\' . 'Permission' | 'Role' | 'HasRoles'
description: VSCode with intelephense throws a strange and hard to debug error when using the spatie/laravel-permissions package even when you follow the docs to the letter. Here is how to fix it.
draft: false
date: "2023-04-13"
categories:
- web development
tags:
- web dev
- laravel
- PHP
- VSCode
ogimage:
- /images/spatie-laravel-vscode-error.png
- 784
- 270
---

![Error message on VSCode](/images/spatie-laravel-vscode-error.png)

I was very frustrated for more than a few minutes with this after installing the great [Spatie package for permissions](https://spatie.be/docs/laravel-permission/v5/introduction) in a project I am working on that requires teams, roles and permissions. Click the link above to check out the documentation and you'll see immediately the appeal it has for any Laravel developer.

> Sidenote: These guys really do churn out an ever growing collection of the most helpful packages that will save you hours of thinking and development, so if you don't know about them... **get to know!**

### The Error

The error itself is the VSCode extension [Intelephense](https://intelephense.com/), that is usually very helpful when writing PHP in VSCode, telling you that there is an error with an `Undefined type ...` after you have followed the steps [outlined here in detail](https://spatie.be/docs/laravel-permission/v5/installation-laravel) to install the permissions package on your Laravel app.

We have:

- Used composer to pull in the package
- Manually registered the service provider
- Published migration and configuration files
- Adjusted to allow for teams
- Cleared the configuration cache
- Run the migrations
- Added the trait to out `User` model

**And BOOM!**... error.

![Y THO](/images/y-tho.png)

I tried `composer dump-autoload`, `./artisan config:clear` (again), `./artisan config:optimize`, all to no avail. Double checked I had followed all the steps... I had. So??

### The Solution

The short answer is, there is no reason. But here is the solution. Just reload the window.

```php
Ctrl + Shift + P // To open the editor commands

// then type in or scroll down and select...
>Developer:Reload Window
```

It's a simple as that. How annoying.

### Credit

I need to shoutout [Akshay K Nair](https://github.com/akshayknz) for replying somewhere deep in a github issue and pointing me to this. I just hope that someone else finds this a lot easier than I did!