---
layout: post
title: Azure DevOps Build Errors
tags: [Development, C#, NuGet, Tips]
categories: [Nuget]
author: Haris Ahmad
description: The project is not compatible with monoandroid10.0. The nuget command failed with exit code(1) and error(System.AggregateException Error parsing solution file at Exception has been thrown by the target of an invocation.
---
# Azure DevOps Build Errors

This post addresses some of the build errors I came across on Azure Pipelines. 

## Incorrect NuGet Version
A CI build on Azure DevOps started failing with the following error:

```shell
[error]The nuget command failed with exit code(1) and error(System.AggregateException: One or more errors occurred. ---> NuGet.CommandLine.CommandLineException: Error parsing solution file at <Solution>.sln: Exception has been thrown by the target of an invocation.
```
Reviewing the build logs, I noticed that the Azure Pipeline was using a much older version of the NuGet executable.

The solution was to use the **NuGet Tool Installer** task in the Azure Pipeline to force a specific NuGet version. The [Nuget Tool Installer](https://docs.microsoft.com/en-us/azure/devops/pipelines/tasks/tool/nuget?view=azure-devops) task can be set in the YAML file like so: 

```shell
- task: NuGetToolInstaller@1
  inputs:
    versionSpec: '>= 5.5.1'
```
This will set the NuGet version to 5.5.1 or above.

## Xamarin.Android Build Error
I recently ran into a build error on Azure Pipelines for a Xamarin.Android project. The project would build fine in Visual Studio 2019, but would fail on Azure Pipelines with the following error:

```shell
Errors in <Project>.csproj
    Project  is not compatible with monoandroid10.0 (MonoAndroid,Version=v10.0). Project  supports: netstandard2.0 (.NETStandard,Version=v2.0)
    One or more projects are incompatible with MonoAndroid,Version=v10.0.
```
Upon further research, I found that the class libraries used in the project were targeting .NET Standard 2.0. However Xamarin.Android 10.x only supports .NET Standard 2.1 onwards. Once the class libraries were targeted to .NET Standard 2.1 the problem was resolved. This is confirmed by the implementation chart available at [.NET Standard](https://docs.microsoft.com/en-us/dotnet/standard/net-standard).