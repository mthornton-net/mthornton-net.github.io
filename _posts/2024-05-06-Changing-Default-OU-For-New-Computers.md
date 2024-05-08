---
layout: post
title: Changing Default OU For New Computers
description: Computer OU default for new machines
date: 2024-05-06 09:30:00 -0500
tags: Active-Directory
---
# Changing Default OU For New Computers

## What Are The Benefits?

Changing the default OU for new computers can be important especially if you have GPOs and/or Policies that are applied to all workstations that are outside of the default Computers container in Active Directory. It'll also help from you or other techs from "forgetting" to move the machine or a set of machines from its default Computers container.

## Prerequisites

You need the following:
- The distinguished name of the OU you want new computers to be when they are created
- Be a part of either the Domain Admins or Enterprise Admins group to use the following tool

## Steps to accomplish this
1. open a powershell or cmd window as admin
2. type in the following syntax:

```cmd
redircmp "OU=EXAMPLE,OU=TEST,DC=DOMAIN,DC=TLD"
```
3. Press enter and your done

Now whenever new computers are added to the domain they will be automatically be added to the OU that you specify.
