---
layout: post
title: Bypass Online Account Requirement When Installing Windows 11
description: Go through Normal Install in Windows 11
date: 2024-05-06 10:15:00 -0500
tags: Windows
---

As of writing (May 2024) and with Windows 11 23H2 you can bypass the online account requirement when you install Windows 11. This can be useful if you are either needing a Windows install ready or needing a mahcine and there is no reason to sign in with an online account.

## After Post Install

After you install Windows you'll go through the OOBE experience. what you need to do first is open a command prompt window which can be done by pressing SHIFT + F10.

After the command prompt is open Type in the following:

```cmd
oobe\bypassnro
```

After you press enter it'll reboot which is normal and it'll go back to the OOBE workflow like earlier but this time you can skip the online account requirement and you can use a local account instead.
