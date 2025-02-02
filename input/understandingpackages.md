﻿---
Order: 50
Title: Understanding Packages
---

# Understanding Packages

Boxstarter installs are a "mash-up" of Chocolatey packages mixed with specific customizations and scripts to wire-up these packages to create a complete environment.

> :choco-info: **NOTE**
>
> If you are familiar with the basic objectives of NuGet and Chocolatey technologies. You can skip this page, but it is important to understand in order to comprehend how Boxstarter installs work.

## What's a package and why does that matter to Boxstarter?

Boxstarter leverage's [Chocolatey](https://chocolatey.org) packages to compose the applications, components and features that make up the Windows environment you aim to create. Chocolatey is based on [NuGet](https://nuget.org) packages. NuGet provides a standardized way of describing a reusable piece of software. Every NuGet package includes a \*.nuspec file which is a manifest that declares the package's version, its dependencies and other descriptive information that assists package consumers to easily discover packages they are looking for. It includes a set of conventions for structuring the files inside of a package and naming them to assist third party software to consume these packages and know what to install based on the environment that wants to install the package.

NuGet in its original and currently most basic implementation is largely centered around the integration of code libraries that another application wants to use. As an application author, I would otherwise need to find a library that provides the functionality I need, and possibly any libraries that library depends on, and so forth. As the versions of these libraries advance, I need to decide if I want to upgrade and if so, how that will affect its compatibility with the other libraries I consume. NuGet addresses a lot of this pain and helps to keep application authors informed when the libraries they consumed are updated.

Chocolatey extends NuGet's scope to the system or machine level. Instead of managing the dependencies of library packages that will be compiled into a single application, Chocolatey focuses on managing the applications that are consumed by a machine, and the other applications those applications depend on. While most Chocolatey packages focus on applications one would install on a computer, these can be any piece of state that can be required by a package. They could be a set of registry keys, a PowerShell module, a Windows Feature such as IIS or MSMQ or an ad-hoc script that needs to be executed in order to bring a machine into the state that is required by a environment in order for it to accomplish its purpose.

To learn more about NuGet and Chocolatey flavored NuGet packages, please check out the [NuGet.org docs](https://docs.nuget.org) and the [Chocolatey documentation site](https://docs.chocolatey.org).

While a typical Chocolatey package contains code involved to silently install an application with no user intervention, Boxstarter packages are usually made up of several calls to Chocolatey's `choco install` command to install all the applications that an environment requires. However, it may also contain quite a bit of custom code needed to wire these applications together and customize the environment. For example, Boxstarter packages might:

- Create IIS sites, port bindings and Certificate setup for an individual IIS web application.
- Configure Firewall rules for applications that need to listen on specific ports.
- Tweak various windows settings to make a machine personalized to a users tastes.
- Install critical Windows updates.
- Manipulate configuration files, toggle WMI settings and start (or stop) services needed for an environment to function.
