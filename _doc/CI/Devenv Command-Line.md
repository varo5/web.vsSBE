---
layout: doc
title: vsSolutionBuildEvent Devenv Command-Line
permalink: /doc/CI/Devenv Command-Line/
---
# vsSolutionBuildEvent Devenv Command-Line

Utility to support the command-line mode of the Visual Studio for work with [vsSolutionBuildEvent](https://visualstudiogallery.msdn.microsoft.com/0d1dbfd7-ed8a-40af-ae39-281bfeca2334/) through **[Devenv](https://msdn.microsoft.com/en-us/library/vstudio/xee0c8y7.aspx)**

**Please note**: you can use the **[CI.MSBuild](../../CI/CI.MSBuild/)** utility for work through msbuild.exe for special build server etc.

-------
{% assign icon = "glyphicon-check" %}{% include elem/gicon %} 

The [Add-ins are deprecated in Visual Studio 2013](http://msdn.microsoft.com/en-us/library/80493a3w.aspx) and removed in [14CTP's](http://www.visualstudioextensibility.com/2014/06/05/visual-studio-14-ctp-add-ins-are-gone/) (also in VS2015 RC)

Therefore, this utility supports the next versions: Visual Studio 2010, 2012, 2013. ([Want to see in VS2015+ ?](https://connect.microsoft.com/VisualStudio/Feedback/Details/1075033))

* The command-Line features are not available for [VSPackages](https://msdn.microsoft.com/en-us/library/bb166424.aspx) and this possible only with our [tricks](https://bitbucket.org/3F/vssolutionbuildevent/issue/25/)

This reported on MS Connect Issue #[1075033](https://connect.microsoft.com/VisualStudio/Feedback/Details/1075033) and you can [vote or comment here](https://connect.microsoft.com/VisualStudio/Feedback/Details/1075033) *if you want to see this feature on newer versions*!

* As variant, you can use other our utility - **[CI.MSBuild](../../CI/CI.MSBuild/)**

-------

## How to get & Install

* Download *[{{site.lnkCur_Devenv[1]}}]({{site.lnkCur_Devenv[2]}})* (SourceForge.net) 
    * All binaries of the CI.MSBuild: [{{site.lnkAll_Devenv[0]}}]({{site.lnkAll_Devenv[1]}})

Currently the Devenv Command-Line it's only additional wrapper (~41 kb) for work with vsSolutionBuildEvent plugin through [API](../../API/). It means, you should also have this library for work.

* Unpack the Devenv archive into `%HOMEPATH%\Documents\` -> `Visual Studio <num>\Addins` folder (see full instruction in [MSDN](https://msdn.microsoft.com/en-us/library/19dax6cz.aspx)). *(you can also delete all *.pdb files from archive)*
    * For example: `C:\Users\<user>\Documents\Visual Studio 2013\Addins\`
* The {% assign lnkT = "vsSolutionBuildEvent plugin" %}{% include elem/vsixlatest %} is already should be installed for your version of the Visual Studio.
    * **Or** {% assign lnkT = "Download" %}{% include elem/vsixlatest %} and extract all files from *.vsix with any archiver ([it's a simple 'zip' archive](https://msdn.microsoft.com/en-us/library/ff407026.aspx)) into `%HOMEPATH%\Documents\Visual Studio 2013\Addins\Devenv\`

That's all. Now you can use the vsSolutionBuildEvent with msbuild.

## How to use

After install you can use the vsSolutionBuildEvent with [Devenv](https://msdn.microsoft.com/en-us/library/vstudio/xee0c8y7.aspx), for example:

```{{site.msblang}}
"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv" "D:\tmp\App1\App1.sln" /Rebuild Debug
```

You can also use the `verbosity` key for details information from vsSolutionBuildEvent - `verbosity:diagnostic` (**debug mode**), for example:

```{{site.msblang}}
"C:\Program Files (x86)\Microsoft Visual Studio 12.0\Common7\IDE\devenv" "D:\tmp\App1\App1.sln" verbosity:diagnostic /Build Release
```

For more details about Command Line mode see [MSDN documentation](https://msdn.microsoft.com/en-us/library/vstudio/xee0c8y7.aspx)

![Example of work](../../Resources/Devenv_example.png)


### Newtonsoft.Json.JsonSerializationException: Error setting value to ...

If you see similar error:

* Try to update manually your old Json library, or remove it. Path, for example:

```
C:\Program Files\Common Files\Microsoft Shared\Visual Studio\12.0\Newtonsoft.Json.dll
```
More details about bug **[here](https://github.com/JamesNK/Newtonsoft.Json/issues/647)**


# References

* [vsSolutionBuildEvent CI.MSBuild](../CI.MSBuild/)
* [Scripts & Commands](../../Scripts/)
* [Examples](../../Examples/)