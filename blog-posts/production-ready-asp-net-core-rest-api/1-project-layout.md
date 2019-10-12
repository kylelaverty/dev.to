---
published: false
title: "Build a Production Ready ASP.NET Core REST API: Part 1 - Project Layout"
cover_image: "https://raw.githubusercontent.com/kylelaverty/dev.to/master/blog-posts/production-ready-asp-net-core-rest-api/assets/header.jpg"
description: ""
tags: csharp, aspnetcore, dotnet, rest
series: Build a Production Ready ASP.NET Core REST API
canonical_url:
---

## Table Of Contents

- [Introduction](#introduction)
- [Goals](#goals)

### Introduction <a name="introduction"></a>

So, you've been tasked with creating a new REST API for your work and you need it to be production ready, fully tested and full of logging. In addition, your company is making heavy use of AWS.

Throughout this series I will be taking you through the process of creating a production ready ASP.NET Core REST API that will end up being hosted inside of AWS.

For Part 1, I will be going over how to get started creating a ASP.NET Core REST API project and how you should lay it out so that you can easily update it and test it in the future.

If you would like to see the progress of the code as I make posts, or if you are coming here after I have completed the series, the code will be made available at the repo below:
{% github kylelaverty/tutorial-restapi no-readme %}

---

### Goals <a name="goals"></a>

When you are dealing with large projects it is essential to know what your goals are each step of the way. In keep with that, I will be laying out in each post what the goals are of the post so that people can skip ones they are not interested in.

- Understand how to create a new ASP.NET project using dotnet core
- Select a project layout
- Setup your GitHub repo
- Make your first commit
- Make your first PR
- Merge your first PR
