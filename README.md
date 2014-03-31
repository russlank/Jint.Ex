﻿# Jint.Ex

## Overview

Jint.Ex is a C# library adding new features to the JavaScript runtime ***[Jint](https://github.com/sebastienros/jint)***.
The main goal is to be able to integrate Jint with UI SDK like 

1. .NET WinForm in Windows
2. UIKit SDK on iOS with the Xamarin stack 
3. Android too with Xamarin stack
4. WinPhone

Integrating with these UI SDKs (1 and 2 are my primary goals) requires the following:

* Building non blocking UI
    * Script are executed in a background thread with ability to access UI (forms and controls)
* Abililty to build true asynchronous API
* Ability to call setTimeout() and setInterval()

Though Jint.Ex will use only 1 .NET background Thread and multiple Timers, 
only one piece of JavaScript will be executed at the same time.

## Phases

1.  Implement: setTimeout(), clearTimeOut(), setInterval() and clearInterval()
2.  Implement: Integration with WinForm
3.  Implement: Integration with iOS

## Features

### setTimeout and clearTimeOut
The methods setTimeout() and clearTimeOut() are not part of the JavaScript language,
but part of the DOM. Nevertheless they can be usefull and sometime necessary
to run some JavaScript libraries written for the browser.
These methods are also present in NodeJs.

#### Javascript

```javascript
function f3() {
    print('Hi 3');
}
var timeout3 = setTimeout(f3, 3000);
    
function f1() {
    print('Hi 1');
}
var timeout1 = setTimeout(f1, 1000);

clearTimeout(timeout3);
print('timeout3 cleared');
```

#### CSharp

```csharp
static void SetIntervalDemo()
{
    Console.WriteLine("Jint setInterval() demo");
    AsyncronousEngine.EmbedScriptAssembly = Assembly.GetExecutingAssembly();
    AsyncronousEngine.Start("setIntervalSetTimeout.js");   
    AsyncronousEngine.Run();

    Console.WriteLine("*** Done ***");
    Console.ReadKey();
}
```

### setInterval and clearInterval

#### Javascript

```javascript
function f1() {
    print('Hi 1 '+(new Date()));
}
var timeout1 = setInterval(f1, 1000);

function f2() {
    print('Hi 2 '+(new Date()));
}
var timeout2 = setInterval(f2, 3000);
```


### Jint.Ex Api

#### The AsyncronousEngine class.
The class AsyncronousEngine allows to run JavaScript script in a background script
and supports interaction with UI and the execution of the asynchronous events.

```csharp

/// <summary>
/// The instance of Jint
/// </summary>
public static Jint.Engine Engine = null;

/// <summary>
/// Reference the assembly that embed the JavaScript scripts.
/// </summary>
public static Assembly EmbedScriptAssembly = null;

/// <summary>
/// Load multiple scripts embed or from the file system. This method must be used the load
/// libraries and must not be used to execute the main script. This method is synchronous.
/// </summary>
/// <param name="fileNames"></param>
public static void LoadScripts(params string[] fileNames);

/// <summary>
/// Wait until all asynchronous event are processed
/// </summary>
public static void Run();

}

/// <summary>
/// Request the event loop to stop;
/// </summary>
public static void Stop();

/// <summary>
/// Kill the event loop. Kill the background thread.
/// </summary>
public static void Kill();

/// <summary>
/// Start the execution of multiple scripts as the Main Script, in the Jint.Ex
/// background thread. The method returns right away.
/// </summary>
/// <param name="fileNames"></param>
public static void Start(params string[] fileNames);

```