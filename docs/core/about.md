---
title: About .NET Core
description: Learn about .NET Core.
ms.date: 09/17/2019
---
# About .NET Core

.NET Core has the following characteristics:

- **Cross-platform:** Runs on Windows, macOS, and Linux [operating systems](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md).
- **Consistent across architectures:** Runs your code with the same behavior on multiple architectures, including x64, x86, and ARM.
- **Command-line tools:**  Includes easy-to-use command-line tools that can be used for local development and in continuous-integration scenarios.
- **Flexible deployment:** Can be included in your app or installed side-by-side (user-wide or system-wide installations). Can be used with [Docker containers](docker/introduction.md).
- **Compatible:** .NET Core is compatible with .NET Framework, Xamarin, and Mono, via [.NET Standard](../standard/net-standard.md).
- **Open source:** The .NET Core platform is open source, using MIT and Apache 2 licenses. .NET Core is a [.NET Foundation](https://dotnetfoundation.org/) project.
- **Supported by Microsoft:** .NET Core is supported by Microsoft, per [.NET Core Support](https://dotnet.microsoft.com/platform/support/policy).

## Languages

C#, Visual Basic, and F# languages can be used to write applications and libraries for .NET Core. These languages can be used in your favorite text editor or Integrated Development Environment (IDE), including:

- [Visual Studio](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)
- [Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
- Sublime Text
- Vim

This integration is provided, in part, by the contributors of the [OmniSharp](https://www.omnisharp.net/) and [Ionide](http://ionide.io) projects.

## APIs

.NET Core exposes APIs for many scenarios, a few of which follow:

- Primitive types, such as <xref:System.Boolean?displayProperty=nameWithType> and <xref:System.Int32?displayProperty=nameWithType>.
- Collections, such as <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> and <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType>.
- Utility types, such as <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>, and <xref:System.IO.FileStream?displayProperty=nameWithType>.
- Data types, such as <xref:System.Data.DataSet?displayProperty=nameWithType>, and [DbSet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/).
- High-performance types, such as <xref:System.Numerics.Vector?displayProperty=nameWithType> and [Pipelines](https://devblogs.microsoft.com/dotnet/system-io-pipelines-high-performance-io-in-net/).

.NET Core provides compatibility with .NET Framework and Mono APIs by implementing the [.NET Standard](../standard/net-standard.md) specification.

## Frameworks

Multiple frameworks have been built on top of .NET Core:

- [ASP.NET Core](/aspnet/core/)
- [Windows 10 Universal Windows Platform (UWP)](https://developer.microsoft.com/windows)
- [Tizen](https://developer.tizen.org/development/training/.net-application)

## Composition

.NET Core is composed of the following parts:

- The [.NET Core runtime](https://github.com/dotnet/coreclr), which provides a type system, assembly loading, a garbage collector, native interop, and other basic services. [.NET Core framework libraries](https://github.com/dotnet/corefx) provide primitive data types, app composition types, and fundamental utilities.
- The [ASP.NET runtime](https://github.com/aspnet/home), which provides a framework for building modern cloud-based internet connected applications, such as web apps, IoT apps, and mobile backends.
- The [.NET Core CLI tools](https://github.com/dotnet/cli) and language compilers ([Roslyn](https://github.com/dotnet/roslyn) and [F#](https://github.com/microsoft/visualfsharp)) that enable the .NET Core developer experience.
- The [dotnet tool](https://github.com/dotnet/core-setup), which is used to launch .NET Core apps and CLI tools. It selects the runtime and hosts the runtime, provides an assembly loading policy, and launches apps and tools.

These components are distributed in the following ways:

- [.NET Core Runtime](https://dotnet.microsoft.com/download) -- includes the .NET Core runtime and framework libraries.
- [ASP.NET Core Runtime](https://dotnet.microsoft.com/download) -- includes ASP.NET Core and .NET Core runtime and framework libraries.
- [.NET Core SDK](https://dotnet.microsoft.com/download) -- includes the .NET CLI Tools, ASP.NET Core runtime, and .NET Core runtime and framework.

### Open source

[.NET Core](https://github.com/dotnet/core) is open source ([MIT license](https://github.com/dotnet/core/blob/master/LICENSE.TXT)) and was contributed to the [.NET Foundation](https://dotnetfoundation.org) by Microsoft in 2014. It's now one of the most active .NET Foundation projects. It can be used by individuals and companies, including for personal, academic, or commercial purposes. Multiple companies use .NET Core as part of apps, tools, new platforms, and hosting services. Some of these companies make significant contributions to .NET Core on GitHub and provide guidance on the product direction as part of the [.NET Foundation Technical Steering Group](https://dotnetfoundation.org/blog/tsg-welcome).

### Designed for adaptability

.NET Core has been built as a very similar but unique product compared to other .NET products. It was designed to enable broad adaptability to new platforms and workloads and it has several OS and CPU ports available (and it may be ported to many more).

The product is broken into several pieces, enabling the various parts to be adapted to new platforms at different times. The runtime and platform-specific foundational libraries must be ported as a unit. Platform-agnostic libraries should work as-is on all platforms, by construction. There's a project bias towards reducing platform-specific implementations to increase developer efficiency, preferring platform-neutral C# code whenever an algorithm or API can be implemented in-full or in-part that way.

People commonly ask how .NET Core is implemented in order to support multiple operating systems. They typically ask if there are separate implementations or if [conditional compilation](https://en.wikipedia.org/wiki/Conditional_compilation) is used. It's both, with a strong bias towards conditional compilation.

You can see in the following chart that the vast majority of [CoreFX](https://github.com/dotnet/corefx) is platform-neutral code that is shared across all platforms. Platform-neutral code can be implemented as a single portable assembly that is used on all platforms.

![CoreFX: Lines of Code per Platform](../images/corefx-platforms-loc.png)

Windows and Unix implementations are similar in size. Windows has a larger implementation since CoreFX implements some Windows-only features, such as [Microsoft.Win32.Registry](https://github.com/dotnet/corefx/tree/master/src/Microsoft.Win32.Registry) but doesn't yet implement many Unix-only concepts. You'll also see that the majority of the Linux and macOS implementations are shared across a Unix implementation, while the Linux and macOS-specific implementations are roughly similar in size.

There's a mix of platform-specific and platform-neutral libraries in .NET Core. You can see the pattern in a few examples:

- [CoreCLR](https://github.com/dotnet/coreclr) is platform-specific. It builds on top of OS subsystems, like the memory manager and thread scheduler.
- [System.IO](https://github.com/dotnet/corefx/tree/master/src/System.IO) and [System.Security.Cryptography.Algorithms](https://github.com/dotnet/corefx/tree/master/src/System.Security.Cryptography.Algorithms) are platform-specific, given that storage and cryptography APIs are different on each OS.
- [System.Collections](https://github.com/dotnet/corefx/tree/master/src/System.Collections) and [System.Linq](https://github.com/dotnet/corefx/tree/master/src/System.Linq) are platform-neutral, given that they create and operate over data structures.

## Comparisons to other .NET implementations

It's probably easier to understand the size and shape of .NET Core by comparing it to existing .NET implementations.

### Comparison with .NET Framework

.NET was first announced by Microsoft in 2000 and then evolved from there. The .NET Framework has been the primary .NET implementation produced by Microsoft during that nearly two decade period.

The major differences between .NET Core and the .NET Framework:

- **App-models** -- .NET Core doesn't support all the .NET Framework app-models. In particular, it doesn't support ASP.NET Web Forms and ASP.NET MVC, but it supports ASP.NET Core MVC. And starting with .NET Core 3.0, .NET Core also supports WPF and Windows Forms on Windows only.
- **APIs** -- .NET Core contains a large subset of .NET Framework Base Class Library, with a different factoring (assembly names are different; members exposed on types differ in key cases). In some cases, these differences require changes to port source to .NET Core. For more information, see [The .NET Portability Analyzer](../standard/analyzers/portability-analyzer.md). .NET Core implements the [.NET Standard](../standard/net-standard.md) API specification.
- **Subsystems** -- .NET Core implements a subset of the subsystems in the .NET Framework, with the goal of a simpler implementation and programming model. For example, Code Access Security (CAS) isn't supported, while reflection is supported.
- **Platforms** -- The .NET Framework supports Windows and Windows Server while .NET Core also supports macOS and Linux.
- **Open Source** -- .NET Core is open source, while a [read-only subset of the .NET Framework](https://github.com/microsoft/referencesource) is open source.

While .NET Core is unique and has significant differences to the .NET Framework and other .NET implementations, it's straightforward to share code between these implementations, using either source or binary sharing techniques.

Because .NET Core supports side-by-side installation and its runtime is completely independent of the .NET Framework, it can be installed on machines with .NET Framework installed without any issues.

### Comparison with Mono

[Mono](https://www.mono-project.com/) is the original cross-platform implementation of .NET. It started out as an [open-source](https://github.com/mono/mono) alternative to .NET Framework and transitioned to targeting mobile devices as iOS and Android devices became popular. It can be thought of as a community clone of the .NET Framework. The Mono project team relied on the open [.NET standards](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/dotnet-standards.md) (notably ECMA 335) published by Microsoft to provide a compatible implementation.

The major differences between .NET Core and Mono:

- **App-models** -- Mono supports a subset of the .NET Framework app-models (for example, Windows Forms) and some additional ones for mobile development (for example, [Xamarin.iOS](https://www.xamarin.com/platform)) through the Xamarin product. .NET Core doesn't support Xamarin.
- **APIs** -- Mono supports a [large subset](http://docs.go-mono.com/?link=root%3a%2fclasslib) of the .NET Framework APIs, using the same assembly names and factoring.
- **Platforms** -- Mono supports many platforms and CPUs.
- **Open Source** -- Mono and .NET Core both use the MIT license and are .NET Foundation projects.
- **Focus** -- The primary focus of Mono in recent years is mobile platforms, while .NET Core is focused on cloud and desktop workloads.

## The future

It was announced that .NET 5 will be the next release of .NET Core and represents a unification of the platform. The project aims to improve .NET in a few key ways:

- Produce a single .NET runtime and framework that can be used everywhere and that has uniform runtime behaviors and developer experiences.
- Expand the capabilities of .NET by taking the best of .NET Core, .NET Framework, Xamarin and Mono.
- Build that product out of a single code-base that developers (Microsoft and the community) can work on and expand together and that improves all scenarios.

For more details about what's being planned for .NET 5, see [Introducing .NET 5](https://devblogs.microsoft.com/dotnet/introducing-net-5/).
