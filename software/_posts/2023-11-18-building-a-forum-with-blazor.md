---
layout: post
title: Building a Forum with Blazor
date: 2023-11-18 00:00:00 -0000
tags: dotnet, blazor, supabase forum, webdev
---

I'm beginning a project to build a simple forum using Blazor. I'm going to be using the WebAssembly version of Blazor, which is currently in preview. I'm going to be using the following technologies:
- Blazor Server
- ASP.NET Core (v8.0)
- Supabase Dotnet Client

## Getting started

Check out [this Microsoft tutorial](https://dotnet.microsoft.com/en-us/learn/aspnet/blazor-tutorial/install) to get started with Blazor WebAssembly. You can download the .NET 8 SDK there. I'm developing on a Mac with Visual Studio Code.

## Creating a new project

Create a new project with the following command:

```bash
dotnet new blazorserver -o Forum
```


## References
- [Microsoft Blazor Tutorial](https://dotnet.microsoft.com/en-us/learn/aspnet/blazor-tutorial/next)
- [Supabase C# Client](https://supabase.com/docs/reference/csharp/v1/initializing)
- https://antblazor.com/