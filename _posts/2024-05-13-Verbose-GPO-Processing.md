---
layout: post
title: Figuring Out Which GPO Is Being Processed Or Stuck
description: GPO Quick Tip
date: 2024-05-13 21:10:00 -0500
tags: Active-Directory
---

# Figuring Out Which GPO Is Being Processed Or Stuck

## How To Verbose This

Often times if you have a handful of Group Policies applied to a machine sometimes its hard to determine which GPO is being stuck. We are going to determine how we can see this by being a bit verbose on the post boot screen rather than windows saying "Please Wait..."

To do this create a computer GPO  and create the following GPP for the registry:

- Registry Key Path: HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
- Value: VerboseStatus
- Type: DWORD
- Data: 1

When that is done and apply the registry either a `gpupdate /force` or reboot the machine you can now see a more verbose output when it tries to load the logon screen.

Here's an example, I've made a Group Policy where it assigns Google Chrome to Computers and this is what it'll show that its being processed:

![GPO Process](/images/posts/2024-05-13-GPO-Verbose/Screenshot-2024-05-13.png)

Not only is this useful on troubleshooting which GPO is taking a long time to process but it can take the guessing work out from "Why is this taking so long?"
