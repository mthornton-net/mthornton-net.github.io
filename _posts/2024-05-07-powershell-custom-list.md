---
layout: post
title: Creating A Custom List In Powershell
description: Powershell custom list
date: 2024-05-07 21:30:00 -0500
tags: Powershell
---

# Creating A Custom List In Powershell


Creating a custom list in powershell can be useful if you need to make a report with custom headers, properties where it has multiple data variables but no labels and among other reasons. Here I’ll show you how to do it in two different ways.

For this example we are going to use the following command:

```powershell
Get-WinEvent -FilterHashtable @{LogName = 'Security';id = '4625'}
```

Here we are getting event logs of failed login attempts on a machine. Lets get all the properties of the event from at least one:

```powershell
(Get-WinEvent -FilterHashtable @{LogName = 'Security';id = '4625'})[0]|select -expandproperty properties
```

Below are the properties that we get when we run said command:

```
Value                          
-----                          
S-1-5-18                       
DESKTOP-P9BU1MI$               
WORKGROUP                      
999                            
S-1-0-0                        
Mike                           
DESKTOP-P9BU1MI                
-1073741715                    
%%2313                         
-1073741718                    
2                              
User32                         
Negotiate                      
DESKTOP-P9BU1MI                
-                              
-                              
0                              
2004                           
C:\Windows\System32\svchost.exe
127.0.0.1                      
0    
```

As you can see its all just a bunch of data with no labels other than the header of “Value”. To make this simple we are going to only get the data that we want to read which is going to be:
- The time the log was generated
- The impacted user
- The IP address it reported

Hint: To figure out what all the values mean from above you can open the event viewer and filter by event ID and check the “Details” tab to figure out what the data is.


## Method One

First lets address the pros and cons of using this Method:

|Pros|Cons|
|---|---|
|This can work on most windows operating systems (This works on Windows 7 and 2008 R2)|It can be tedious to add more data if needed| 

First declair your data variable and the events you want to read.

```powershell
$data = @()
$Events = Get-WinEvent -FilterHashtable @{LogName = 'Security';id = '4625'}
```

Next we are going to start our foreach loop and assign variables in the loop with the data we need like so.

```powershell
foreach ($Event in $Events){
    $Time = ($Event.TimeCreated).ToString("yyyy/MM/dd HH:MM:ss")
    $User = (($Event|select -ExpandProperty properties)[5]).value
    $IPAddress = (($Event|select -ExpandProperty properties)[19]).value
    $Row = ''|Select Time,User,IPAddress
    $Row.time = $Time
    $Row.User = $User
    $Row.IPAddress = $IPAddress
    $Data += $Row   
}
```

As you notice from above we are getting all the data we need for each event entry stored in the $Event variable. what the $Row is doing is that not only its declairing itself as a variable so it knows it exist but it also clears any data that was stored in it so it can add new data to the $Data variable and it creates its own variables "Time", "User" and "IPAddress".

At the very bottom it adds all the data from the $Row variable to the $Data variable. 

After that is done we can check all the data just by typing this:

```powershell
$Data
```

This is the output that was produced on my lab PC:

```
Time                User IPAddress
----                ---- ---------
2024/05/06 22:05:47 Mike 127.0.0.1
2024/05/06 22:05:45 Mike 127.0.0.1
2024/05/06 22:05:43 Mike 127.0.0.1
2024/05/06 22:05:41 Mike 127.0.0.1
```

## Method 2

This method is a more "modern" way of creating lists, like before here are the pros and cons of using this method:

|Pros|Cons|
|---|---|
|This will make/add columns easier|This will not work on older versions of windows (before Windows 10)|

We will use the same data as the previous example to make a custom list. First what you need to do is to declair a list object:

```powershell
$Data = [System.Collections.Generic.List[Object]]::new()
```
Now in the foreach loop the script is a bit different but it'll be the same as before in terms of functionality:

```powershell
foreach ($Event in $Events){

    $ReportLine = [PSCustomObject] @{
        Time = ($Event.TimeCreated).ToString("yyyy/MM/dd HH:MM:ss")
        User = (($Event|select -ExpandProperty properties)[5]).value
        IPAddress = (($Event|select -ExpandProperty properties)[19]).value
        }
    $Data.Add($ReportLine)
}
```

As you can see there is a lot of less lines to put in and if you need to add columns it's simple as making a new name and tell what data it needs.

The output will be the exact same as before:

```
Time                User IPAddress
----                ---- ---------
2024/05/06 22:05:47 Mike 127.0.0.1
2024/05/06 22:05:45 Mike 127.0.0.1
2024/05/06 22:05:43 Mike 127.0.0.1
2024/05/06 22:05:41 Mike 127.0.0.1
```

Hope this helps on creating a custom list for your reports!
