# EC601-MiniProject1

By Matthew Boyd

#### Contents

* [Introduction](#introduction)
* [Technology Overview](#technology-overview)
* [Resources](#resources)
* [Demo](#demo)
* [Lessons Learned](#lessons-learned)

<a name="introduction"/>

## Introduction

I chose to review WebAssembly for Mini Project 3 because I am intrigued by its possibility to bring complex, compute-intensive applications to the browser, and accordingly to every device regardless of operating system, without rewriting existing code or writing new code in JavaScript/Typescript.

<a name="technology-overview"/>

## Technology Overview

WebAssembly is the browser-based equivalent of platform-specific Assembly code. It allows developers to compile code written in a standard compiled programming language (C, C++, Rust, Go, C#, etc.) into a version of assembly and binary code that can be run in a browser on any operating system. This provides many advantages over running code locally or using JavaScript/Typescript:
- Because the code is compiled to binary ahead of time, the file transferred to the user's browser is very small
- Browser caching means the binary file only must be downloaded to your browser once and can be executed repeatedly, across page refreshes, very quickly
- Because the code is pre-compiled, there is no "interpretation" step to perform at the client, speeding execution significantly
  - The execution environment created by the browser uses common capabilities available on most CPUs/OSes (i.e., if you can install the browser, WebAssembly will execute)
  - The aim is to execute at native speed (i.e., just like a program running locally outside the browser)
- Rather than compile an executable file for every platform you want to support (Windows, Mac, Linux, Raspberry Pi, Android, IOS, etc.) you only need to compile to one file for one platform: WebAssembly
- Utilizes a memory-safe sandbox environment for code execution
- Direct memory stack manipulation (e.g., malloc) inside the sandbox environment improves performance and minimizes execution footprint

WebAssembly can be used to replace JavaScript entirely (e.g., Blazor from Microsoft), but more often it is used to either augment an existing site or to migrate an existing tool to the web (e.g., the WebAssembly implementation of "jq" at jqkungfu.com). Reasons for this limited use are the inherent complexity of compiling existing code to WebAssembly, the ease of writing JavaScript/Typescript, and the limited list of WebAssembly output/return types. (Note: While there are only a few valid return types available to WebAssembly functions, JavaScript can access the memory stack in the WebAssembly sandbox, allowing for efficient passing of large volumes of data between the sandbox and the browser.) 

Given the above, WebAssembly does have some ideal use cases:
- Migrating existing legacy codebases to the web (e.g., AutoCAD)
- Migrating command line tools to the web (e.g., jqkungfu.com)
- Performing compute intensive tasks efficiently without local installs (i.e., image or video manipulation, large dataset visualization, etc.) (e.g., squoosh.app, github.com/iodide-project/pyodide)

<a name="resources"/>

## Resources

[WebAssembly - https://webassembly.org/](https://webassembly.org/)  
[Awesome WebAssembly Languages - https://github.com/appcypher/awesome-wasm-langs](https://github.com/appcypher/awesome-wasm-langs)  
[Hit the Ground Running with WebAssembly - https://medium.com/@robaboukhalil/hit-the-ground-running-with-webassembly-56cf9b2fa35d](https://medium.com/@robaboukhalil/hit-the-ground-running-with-webassembly-56cf9b2fa35d)  
[Blazor: Build client web apps with C# - https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor](https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor)  
[Roundup: The AutoCAD Web App at Google I/O 2018 - https://blogs.autodesk.com/autocad/autocad-web-app-google-io-2018/](https://blogs.autodesk.com/autocad/autocad-web-app-google-io-2018/)  
[Level up command-line playgrounds with WebAssembly - https://opensource.com/article/19/4/command-line-playgrounds-webassembly](https://opensource.com/article/19/4/command-line-playgrounds-webassembly)  
[Google Labs Announces Squoosh: Image Compression PWA - https://www.infoq.com/news/2018/11/google-squoosh-pwa-webassembly/](https://www.infoq.com/news/2018/11/google-squoosh-pwa-webassembly/)  
[iodide-project/pyodide: The Python scientific stack, compiled to WebAssembly - https://github.com/iodide-project/pyodide](https://github.com/iodide-project/pyodide)  
[Level Up with WebAssembly Book - https://levelupwasm.com/](https://levelupwasm.com/)  

<a name="demo"/>

## Demo

Following the tutorial provided in the excellent WebAssembly ebook "Level Up With WebAssembly" by Robert Aboukhalil, I ported an open source version of the classic arcade game "Pacman" written in C++ to WebAssembly. The port required some effort, but not nearly as much as trying to implement this from scratch in JavaScript and HTML.  
Steps:
- Installing the WebAssembly compilation toolchain "Emscripten"
- Making minor modifications to the original C++ source code to reconfigure infinite loops (which are common in games) 
- Running 3 levels of custom commands to finally generate the running code, which includes 4 files: 
  - 1 HTML file
  - 1 WebAssembly binary (pacman.wasm)
  - 1 WebAssembly "file" of included assets (audio, etc.)
  - 1 JavaScript file to glue it all together

Run the demo in your browser at [https://mcboyd-bu.github.io/EC601-MiniProject3-WebAssembly/pacman.html](https://mcboyd-bu.github.io/EC601-MiniProject3-WebAssembly/pacman.html).  
NOTE: There is sound, but it doesn't sound great - the sampling is less than stellar. 

![Online Demo Screenshot](https://github.com/mcboyd-bu/EC601-MiniProject3-WebAssembly/blob/master/docs/DemoScreenshot.png "Online Demo Screenshot")

<a name="lessons-learned"/>

## Lessons Learned

Going into this project I knew just enough about WebAssembly to think it was a magical panacea that would usher in a new era of complex desktop apps ported to run on the web by any and every developer who wanted to do so. 
Now that I know more and have done some app conversion myself, I see that magical era won't be coming soon. Some of the challenges I see (that will likely change in the future, given the active development in this technology):
- Graphics applications are clearly possible (see [demo above](https://mcboyd-bu.github.io/EC601-MiniProject3-WebAssembly/pacman.html)), but are limited to specific libraries already ported to WebAssembly or will require a significant rewrite
- Applications that require additional DLLs outside the binary executable are challenging to convert (there are no best practices or even good tutorials)
- Applications written to a specific OS and its services/display conventions (e.g., "WinForms") will require significant code changes before conversion
- The toolchain is great, but still complex for a novice: issues can be challenging to track down and resolve

I would like to continue learning more about WebAssembly and to convert more (*useful*) applications. For example, I would like to port a point cloud viewer/manipulation app to Web Assembly. I see great potential in the technology to broaden access to complex applications, especially in the engineering fields where half the challenge can be getting complex tools and their dependencies installed, much less running. 
