---
layout: post
title: Changing Default OU For New Computers
description: All the quirks with using raiserror
date: 2024-05-06 09:30:00 -0600
tags: Active-Directory
---

Changing the default OU for new computers can be important especially if you have GPO/Policies that are applied to all workstations that are applied to an OU. It'll also help from you or other techs from "forgetting" to move the machine or a set of machines from its default Computer OU.

## Prerequisites

You need the following:
- The distinguished name of the OU you want the default to be for new computers
- Be a part of either the Domain Admins or Enterprise Admins group to use the following tool

## Steps to accomplish this
1. open a powershell or cmd window as admin
2. type in the following syntax:

```cmd
redircmp "OU=EXAMPLE,OU=TEST,DC=DOMAIN,DC=TLD"
```
3. Press enter and your done

Now whenever new computers are added to the domain they will be automatically be added to the OU that you specify.
