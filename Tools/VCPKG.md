# VCPKG
---
```toc
title: ""
```
---

> vcpkg is a free C/C++ package manager for acquiring and managing libraries.

vcpkg is a open-source dependency manager developed by Microsoft for C/C++. It is similar to NuGet for .NET.

Below, either do the [[#Normal Setup|normal]] or [[#Quick Setup|quick]] setup (don't do the quick setup if you don't know what you're doing).

## Quick Setup
**Only execute this script if you know what it does**. <small>(Are you crazy ?? don't execute random stuff).</small>

The script contains all the commands explained in the [[#Normal Setup]].
```powershell
$path = "C:\dev\vcpkg"

# Creates the directory (if it doesn't exist already)
New-Item -ItemType Directory -Path $path

# Clone repo
git clone "https://github.com/microsoft/vcpkg" $path

cd $path

# execute bootstrap
.\bootstrap-vcpkg.bat

# Add vcpkg directory to PATH env-var
setx PATH "$env:PATH;$path"

# Install 64bit by default
setx VCPKG_DEFAULT_TRIPLET "x64-windows"

# Disable defalut metrics
setx VCPKG_DISABLE_METRICS "true"

# Set proxy (trumpf in this case)
setx HTTP_PROXY "172.17.7.191:80"  
setx HTTPS_PROXY "172.17.7.191:80"

# Integrate vcpkg
vcpkg integrate powershell
vcpkg integrate install
```

## Normal Setup
### Download & Default Installation
The installation requires Git to be installed. In case you haven't installed Git yet (maybe rethink your career-choices before proceeding), you can install it [here](https://git-scm.com/downloads).

After that, clone the `vcpkg` GitHub repository into any folder:
```powershell
git clone "https://github.com/microsoft/vcpkg"
```
It's recommended to clone the repository into a easily accessible directory like `C:\dev`.

Then, you can execute the bootstrap script:
```powershell
.\vcpkg\bootstrap-vcpkg.bat
```

### Install 64-Bit by default
`vcpkg` installs 32-bit packages by default. Normally you'd want to download 64-bit libraries though. You can set an environment variable for vcpkg to use that by default:
```powershell
setx VCPKG_DEFAULT_TRIPLET "x64-windows"
```
### Setup Proxy
vcpkg won't work directly after installing if your computers network flows over a proxy. vcpkg uses `curl` in the background, which executes the web-requests for downloads. 

Therefore, you need to set the curl-proxy for vcpkg to work on your machine:
```powershell
setx HTTP_PROXY "<proxy>"  
setx HTTPS_PROXY "<proxy>"
```

Just fill your proxy in as `<proxy>`: *172.17.7.191:80*

### Disable metrics
vcpkg sends anonymous data to Microsoft by default. No one wants that, you can disable metrics with another environment-variable (which is not set, obviously Microsoft):

```powershell
setx VCPKG_DISABLE_METRICS "true"
```

### Setup for Visual Studio
Most likely you'll be coding `C++` in Visual Studio. To use vcpkg with visual studio, you first need to integrate all packages first. What this means is, that vcpkg makes all installed packages available user-wide, so you can use them in VS projects:

```powershell
vcpkg integrate install
```
<sub>(Admin rights might be necessary on first use)</sub>

You can easily remove this integration again (if you need to for whatever reason):
```powershell
vcpkg integrate remove
```

### Powershell tab-completion
Similar to the [[#Setup for Visual Studio|integration for VS use]], you can also integrate vcpkg in Powershell, so that it auto-completes your commands on tab:
```powershell
vcpkg integrate powershell
```

### Add `vcpkg.exe` to PATH
You will be using vcpkg from the command line to install packages (there's no GUI unlike NuGet). Normally you wouldn't want to call the full vcpkg path `C:\dev\vcpkg\vcpkg.exe` to use the CLI. Adding the `C:\dev\vcpkg` path to the `PATH`-env variable, adds global support for `vcpkg` use.

```powershell
setx PATH "$env:PATH;C:\dev\vcpkg"
```

## Using vcpkg
Now that you've installed vcpkg with its settings, it's time to use it:
```
vcpkg install opencv
```

Yeah, it's this easy. You've now installed OpenCV. The more time-consuming part is figuring out what package you need to use. You can browse packages on [vcpkg.io](https://vcpkg.io/en/packages.html).

Also, if you're `#include`-ing a package in a Visual Studio C++ project which you haven't downloaded yet, VS IntelliSense will copy you the command to download the vcpkg-package when you hover over the include tag.