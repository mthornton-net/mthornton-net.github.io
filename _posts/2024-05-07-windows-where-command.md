---
layout: post
title: Where Windows Command? Yes, Right Here!
description: Windows where command
date: 2024-05-07 21:45:00 -0500
tags: Windows
---

# Where Windows Command? Yes, Right Here!

## What Is Where?

Where is a command in Windows as the name implies tells you where files are located based on the drive or folder you specify to look at.

This can be useful for finding a specifc file or putting in "*" or "?" as part of a wildcard search. This program woks best in command prompt as running it in powershell just spits out errors when you attempt to run `where`.

Here are the switches you can use with this command:

- /R - Recursive search
- /Q = Quiet mode
   - This will not return anything on the console, instead it'll either dump a 0 or a 1 to %errorlevel%.
   - you can just type `echo %errorlevel%` in a command prompt to get the code.
- /F = Displays the files in double quotes
- /T = Displays the file size and last modified time for the matching file(s)

Heres an example of how to run this, say we want to look at the D:\ drive and search if any files ending in HTML exist:

```cmd
where /r d:\ *.html
```

## Where vs Dir /S /B|Find /I "Whatever String"

Now you can probably saying whats the difference between the two? from a functionality perspective its the same, but using fewer commands can make your code a bit cleaner if you can use just one command vs two if not more just to perform the same task.
