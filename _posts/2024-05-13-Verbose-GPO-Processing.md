---
layout: post
title: Verbose Logon, Logoff And Startup Messages
description: GPO Quick Tip
date: 2024-05-13 21:10:00 -0500
tags: Active-Directory
---

# Verbose Logon, Logoff And Startup Messages

## How To Verbose This

Often times if you have a handful of Group Policies, software or other miscellaneous settings applied to a machine, sometimes it's hard to determine what is being stuck. We are going to show how we can see this by being a bit verbose on the Logon, Logoff and startup screen rather than Windows saying "Please Wait..."

To do this, create a computer GPO and create the following GPP for the registry:

- Registry Key Path: HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
- Value: VerboseStatus
- Type: DWORD
- Data: 1

When you are done creating the GPO and you have linked it, run a `gpupdate /force` in CMD and then reboot the machine. Now you can see a more verbose output when you logoff, logon and startup.

Here's an example, I've made a Group Policy where it assigns Google Chrome to Computers and this is the output after a reboot:

![GPO Process](/images/posts/2024-05-13-GPO-Verbose/Screenshot-2024-05-13.png)

Not only is this useful on troubleshooting which GPO or setting is taking a long time to process but it can take the guessing work out from "Why is this taking so long?"
