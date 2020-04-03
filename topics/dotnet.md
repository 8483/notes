# .NET Framework

It's a computing framework for building applications on Windows, and it serves as an environment in which computer programs can run.

It is composed of: 
- **Assemblies**, which are typically `.exe` or `.dll` files that contain compiled code which requires .NET to run.
- **The Common Language Runtime (CLR)**. This is a set of Windows libraries whose job is to run assemblies. It's essentially a Virtual Machine.
- **Class libraries**. A set of object-oriented APIs which can be used by assemblies.

It's possible for anyone to create a compiler that compiles a language for .NET, but the most common languages that use .NET are C# and VB.NET.

If you write your code to the framework, you can be sure it will work on anything that the framework says it can run on. In the case of Windows .NET, that means 'any' version of Windows. In the case of .NET Core, that's starting to mean 'any' modern device.

### CLR

It's an application sitting in memory, whose job is to translate IL Code (Intermediate Language) into Machine Code i.e. JIT (Just-in-time-compilation).

Java's "CLR" is called "JVM".

C/C++ > Machine Code  
Java > ByteCode (JVM) > Machine Code  
C# > IL Code (CLR) > Machine Code

### .dll

Basically a module i.e. a collection of reusable code for other programs to call.

### .bat

A script file, equivalent to a Linux shell/bash script.

### Class Library

Applications consist of the building block classes.  

Related classes are organized in containers called Namespaces.  

Namespaces are organized in an Assembly, either an EXE (Executable) or DLL (Dynamically Linked Library)

When the application is compiled, one or more Assemblies are made.

# For Development

1. .NET contains a large set of programs which you can call through your program. These are the programs which contain simple functions like join two arrays to complex functions like translate voice to text /or recognize red object in a image(image analyses) etc, Provide functionality to make a internet application, mobile application etc, alot of them are provided(you can call those functions from your program). The .NET libraries are soo vast that you can program Robots/Arduino etc to develop signal processing, image analysis, large set of web application frameworks, etc.

2. It maintains a common language underneath and allows you to program in different higher level languages like C#, VB, IronPython etc. When you compile it convert to a common language. It provides different set of build tools to develop applications, integrate, add other frameworks, allow others to easily write frameworks, etc.

# For End User: 

When you develop a program in .NET (or you can say using .NET) to run this program on many other computers you need to have a corresponding .NET framework available on that computer. So you have to install the .NET framework before you run your program. The .NET framework which you install on different computers have all the functionality your program needs but it wont have the tools for compile/build/develop .NET applications (because those are not needed on the end user machines. They also ported this framework to Linux so you can run .NET applications on Linux platform.