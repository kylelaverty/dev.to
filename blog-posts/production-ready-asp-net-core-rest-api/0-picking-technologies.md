---
published: false
title: "Build a Production Ready ASP.NET Core REST API: Part 0 - Picking Technologies"
cover_image: "https://raw.githubusercontent.com/kylelaverty/dev.to/master/blog-posts/production-ready-asp-net-core-rest-api/assets/header.jpg"
description: ""
tags: csharp, aspnetcore, dotnet, rest
series: Build a Production Ready ASP.NET Core REST API
canonical_url:
---

## Table Of Contents

- [Introduction](#introduction)
- [Requirements](#requirements)
- [Selected Technologies](#selected-technologies)
- [Technology Explanation](#technology-explanation)
- [Wrapup](#wrapup)

### Introduction <a name="introduction"></a>

So, you've been tasked with creating a new REST API for your work and you need it to be production ready, fully tested and full of logging. In addition, your company is making heavy use of AWS.

Throughout this series I will be taking you through the process of creating a production ready ASP.NET Core REST API that will end up being hosted inside of AWS.

For part 0, I will be going through all of the technologies we will be using and a brief description of why it was selected.

If you would like to skip right into creating the application, here is a link to Part 1: [link to follow once posted]

If you would like to see the progress of the code as I make posts, or if you are coming here after I have completed the series, the code will be made available at the repo below:
{% github kylelaverty/tutorial-restapi no-readme %}

---

### Requirements <a name="requirements"></a>

So the first step in any great project is to figure out what you actually need. In keeping with this idea I have listed out what a good production ready REST API should have:

- Support for HTTP response codes
- Support for HTTP methods (GET,POST,PUT, etc.)
- Support for endpoint versioning
- Support request validation
- Support request routing
- Support Authentication
- Support Authorization
- Provide request/response logging
- Provide error handling

From a technical standpoint it should also have the following:

- An easy to understand project layout
- Unit tests
- Cross platform compatible
- Standardized API documentation

---

### Selected Technologies <a name="selected-technologies"></a>

With all of these requirements layed out it is time to dig into what technologies I will be using for this series:

- AWS EC2 for the hosting environment
- ASP.NET Core for the REST API framework
- FluentValidation for request validation
- Dapper for basic ORM
- Autofac for a DI library
- AutoMapper for cross object mappings
- NLog for log generation
- NUnit for unit tests
- OpenAPI 3.0 for API documentation
- Existing user table in sql database for Authorization and Authentication

---

### Technology Explanation <a name="technology-explanation"></a>

Below will be an item by item listed of each technology, why it was selected and a link out to the project's github repo.

#### AWS EC2

AWS is one of the most cost effective cloud providers for continuous operation linux instances. They also allow you to construct your own AMI if you need a customized image. For these reasons we are going to be assuming the application will be hosted inside of an AWS EC2 instance.

As of the time of writing this, using the `Amazon Linux 2 AMI` image and hosting it on a `t3.micro` in `us-east-1` with a `30GB` drive would cost **\$10.59** per month.

#### ASP.NET Core

We will be making use of ASP.NET Core with c# for our REST API.

I selected this framework because, donet core has multi-platform deployment and development support, is a typed languaged and has a healthly, well maintained ecosystem. Once that was selected ASP.NET is the defacto way to create REST APIs in dotnet core so it was a very easy pick.

Here is the github for the framework:
{% github dotnet/core no-readme %}

#### FluentValidation

Except from FluentValidation github on what FluentValidation is:
`A popular .NET validation library for building strongly-typed validation rules.`

We will be making use of this library for handling incoming request validation in the REST API. Because it is able to link into the existing ASP.NET Core request pipeline, it is one of the best options for providing request validation that does not require any changes or code in your controller.

Here is the github for this library:
{% github JeremySkinner/FluentValidation no-readme %}

#### Dapper

Dapper's github describes it as `a simple object mapper for .Net`. This is correct but misses that it is only used for mapping objects from sql commands to plain old c# objects (POCOs). This is often refered to as ORM or Object-relational mapping.

When I was looking into how to handle returning data from sql commands most of the examples I saw were making use of Entity Framework Core (EF Core). In the past I have run into issues trying to fix up broken queries and maintain the table mappings inside of EF. I have also used Dapper on other projects and been very happy with the syntax of mapping results to objects. For these reasons, it was selected to perform the role of ORM in this project.

It is important to note that Dapper is a very focused product and because of that it might not be the correct choice for all production projects.

Here is the github for this library:
{% github StackExchange/Dapper no-readme %}

#### Autofac

Autofac is best described with an extert from their github readme:
`Autofac is an IoC container for Microsoft .NET. It manages the dependencies between classes so that applications stay easy to change as they grow in size and complexity.`

When investigating the best way to make my project easily testable so unit tests could be created without driving me crazy, I discovered that ASP.NET Core shipps with a pluggable framework for DI (Dependency Injection). So my first step in evaluating what to use was to find something that could hook into the existing system in the framework without a bunch of janky code. Autofac fit the bill. After doing some reasearch into the capabilities of Autofac I decided that it had the right feature set to meet my needs.

Here is the github for this library:
{% github autofac/Autofac no-readme %}

#### AutoMapper

AutoMpper is a small library that is designed to handle the surprisingly complicated process of mapping one object to another.

Once I got far enough into the planning stages of the project to start thinking about how the project was going to be layed out I realized that there were going to be DTOs and business objects and I would need to be able to go back and forth between the two frequently. This is some of the most boring and error prone code that I would be writing so I looked for a library to help.

AutoMapper is designed to address both of these problems. It does optomistic mapping between two objects ignoring differences in casing of property names and only mapping the ones that exist in both objects. It also supports more complex mappings if there is some kind of transformation that has to happen between the two objects.

Here is the github for this library:
{% github AutoMapper/AutoMapper no-readme %}

#### NLog

NLog's github describes it as: `...a free logging platform for .NET with rich log routing and management capabilities. It makes it easy to produce and manage high-quality logs for your application regardless of its size or complexity.`

NLog was selected because it supports the following:

- third party log output targets
- structured logging (outputting JSON objects)
- code based configuration
- asp.net core pipeline integration

The primary items of interest are the core pipeline integration and the third part log output targets. With these features we will be hooking into the asp.net core ILogger and outputting logs to AWS CloudWatch.

Here is the github for this library:
{% github NLog/NLog no-readme %}

#### NUnit

NUnit is a unit-testing framework that supports all .NET languages.

It will be used to write the unit tests for this project and support running those tests and creating the test results. NUnit was selected since I had experience with it from several previous projects and it met my requirement of being runnable in a CI system.

Here is the github for this library:
{% github nunit/nunit no-readme %}

#### OpenAPI 3.0

OpenAPI is a community-driven open sepcification for describing a programming language agnostic description of REST APIs. It has the goal of ensuring that both computer and humans can discover and understand the abilities of an API without needing to look at the source code.

OpenAPI 3.0 is the first version where it has been seperate from, and replaced, the older Swagger spec. OpenAPI 2.0 was identiticle to Swagger 2.0.

We will be using this to document our API so that computers and humans can easily consume it.

When evaluating what to use for API documentation OpenAPI quickly became the best choice based on the direction the industry was moving in and because of the support that exists within third part programs for reading in the spec and generating elements from it. One example is Postman which can read in an OpenAPI spec and generate a collection with all of the endpoints already configured with the correct headers and bodies.

Here is the github for this library:
{% github OAI/OpenAPI-Specification no-readme %}

#### Existing user table

This is the one part of this project that will be very different from what most examples show. In most cases examples online will make use of either ASP.NET's Identity system or an online auth system like Auth0 or Google. The problem with that approach is that many production systems already have user logins stored in a database somewhere.

So for this series I will be using a user table stored in an existing database that is currently driving website logins and desktop application logins.

---

### Wrapup <a name="wrapup"></a>

This is where I leave you for today. In the next installment I will be going over how to initialize a project and how to layout the project.

---

Good luck with your own projects.

<a href="https://www.buymeacoffee.com/QSHlybZIM" target="_blank"><img src="https://bmc-cdn.nyc3.digitaloceanspaces.com/BMC-button-images/custom_images/orange_img.png" alt="Buy Me A Coffee" style="height: auto !important;width: auto !important;" ></a>
