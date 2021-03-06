---
layout: doc
title: FileComponent
description: I/O File operations.
permalink: /doc/Scripts/SBE-Scripts/Components/FileComponent/
---
# FileComponent

I/O File operations.

### Remarks

#### Redirection for standard streams

*for call/scall/sout* etc.:

* All errors can be ~disabled with arguments, for example:
    * stderr to stdout: `command` **2>&1**
    * stderr to NUL i.e. as disabled: `command` **2>nul**
    * stderr into file: `command` **2>** `path_to_file`
    * etc.
* stdout can be a similar:
    * `command` **>** `destination` or `command` **1>** `destination`
    * Where `destination` it's similar as above.
* For more details see: [MS Q110930](http://support.microsoft.com/kb/110930/en-us)

#### Support of standard streams

* STDOUT - Standard output stream.
* STDERR - Standard error stream.

#### IO alias

*available with v0.12+*

```{{site.sbelang1}}
#[IO ...]
```

#### MSBuild Property Functions.

You should also remember, the some features also available with [MSBuild](../../../MSBuild/) core as [Property Functions](https://msdn.microsoft.com/en-us/library/vstudio/dd633440%28v=vs.120%29.aspx)

* Use the [System.IO Namespace](https://msdn.microsoft.com/en-us/library/System.IO%28v=vs.100%29.aspx) for example:

```{{site.msblang}}
$([System.IO.File]::Exists("D:/tmp/result.log"))
```
*[File.Exists](https://msdn.microsoft.com/en-us/library/system.io.file.exists%28v=vs.100%29.aspx) Method*

#### Regex & Wildcards rules

* Regular Expression Language - [Quick Reference](https://msdn.microsoft.com/en-us/library/az24scfc.aspx)
* Wildcards - Matches any single character:
    * `*` - 0 or more times.
    * `+` - 1 or more times.
    * `?` - 1 any single character.

## Available features

### call 

Caller of executable files.

Syntax:

```{{site.sbelang1}}
void #[File call(string filename [, string args])]
```

v0.11.3+:

```{{site.sbelang1}}
void #[File call(string filename [, string args [, integer timeout]])]
```

Where,

* filename - Path to executable file.
* args - Arguments to executable file.
* timeout - How long to wait the execution, in seconds. 0 value - infinitely

### scall

Caller of executable files in silent mode.

Syntax:

```{{site.sbelang1}}
void #[File scall(string filename [, string args])]
```

v0.11.3+:

```{{site.sbelang1}}
void #[File scall(string filename [, string args [, integer timeout]])]
```
* timeout - How long to wait the execution, in seconds. 0 value - infinitely

### sout

Receives data from standard streams for executed file. To disable errors use the `2>nul` and similar ([Redirection for standard streams](#redirection-for-standard-streams)).

Syntax:

```{{site.sbelang1}}
string #[File sout(string filename [, string args])]
```

v0.11.3+:

```{{site.sbelang1}}
string #[File sout(string filename [, string args [, integer timeout]])]
```
* timeout - How long to wait the execution, in seconds. 0 value - infinitely

### cmd

v0.11.3+:

Alias to sout() for cmd.
`- #[File cmd("args")] -> #[File sout("cmd", "/C args")]`

Receives data from standard streams for cmd process with arguments.

```{{site.sbelang1}}
string #[File cmd(string args [, integer timeout])]
```
* timeout - How long to wait the execution, in seconds. 0 value - infinitely

### get

Get all data from text file.

Syntax:

```{{site.sbelang1}}
string #[File get("filename")]
```

### write

To write data in a text file.

* Creates if the file does not exist.
* Overwrites content if it already exists.
* Allows use a [standard streams](#support-of-standard-streams)

Syntax:

```{{site.sbelang1}}
void #[File write("filename"): mixed data]
```

v0.10+:
 
```{{site.sbelang1}}
void #[File write(string name, boolean append, boolean line, string encoding): mixed data]
```
* name - File name.
* append - Flag to append the content to the end of the file.
* newline - To write with newline.
* encoding - Preferred [Encoding](https://msdn.microsoft.com/en-us/library/system.text.encoding.aspx#Anchor_5):

```text
utf-8
windows-1251
us or us-ascii	
utf-16
...
```

The `utf-8` is used by default.

[BOM](https://en.wikipedia.org/wiki/Byte_order_mark) (Byte-Order Mark) special names:

v0.12.6+

name      | BOM
----------|-----
utf-8     | no
utf-8-bom | yes - `0xEF 0xBB 0xBF`


### append

To append data to the end of a text file or create new if file does not exist.

Syntax:

```{{site.sbelang1}}
#[File append("filename"): mixed data]
```

### writeLine

To write data with newline in a text file.

* Creates if the file does not exist.
* Overwrites content if it already exists.
* Allows use a [standard streams](#support-of-standard-streams)

Syntax:

```{{site.sbelang1}}
#[File writeLine("filename"): mixed data]
```

### appendLine

To append data with newline to the end of a text file or create new if file does not exist.

Syntax:

```{{site.sbelang1}}
#[File appendLine("filename"): mixed data]
```

### replace

To replace data in files.

Syntax:

```{{site.sbelang1}}
void #[File replace(string file, string pattern, string replacement)]
```

```{{site.sbelang1}}
void #[File replace.Regexp(string file, string pattern, string replacement)]
```

* `pattern` should contain [Regular Expression Language](https://msdn.microsoft.com/en-us/library/az24scfc.aspx)
* `replacement` may contain the following [substitution elements and replacement patterns](https://msdn.microsoft.com/en-us/library/ewy2t5e0.aspx)

```{{site.sbelang1}}
void #[File replace.Wildcards(string file, string pattern, string replacement)]
```

`pattern` may contain wildcards:

```text
Matches any character:
 *   - 0 or more times.
 +   - 1 or more times.
 ?   - 1 single.
```

Alias for Regexp (v0.10+):

```{{site.sbelang1}}
void #[File replace.Regex(string file, string pattern, string replacement)]
```

Samples:

```{{site.sbelang1}}
#[File replace.Regexp("source.extension.vsixmanifest", "<Version>[0-9.]+</Version>", "<Version>#[var version]</Version>")]
```

```{{site.sbelang1}}
#[File replace.Regexp("file.log", "(\d+)", "~$1~")]
```

### exists

v0.10+

#### directory

Determines whether the given path refers to an existing directory on disk.

```{{site.sbelang1}}
boolean #[IO exists.directory(string path [, boolean environment])]
```
* path - Path to directory
* environment - Use Environment PATH (Associated for current process).

Samples:

```{{site.sbelang}}
#[( #[IO exists.directory("D:\tmp\log")] ){
   ...
}]
```

```{{site.sbelang}}
#[( #[IO exists.directory("System32", true)] ){
   ...
}]
```

#### file

Determines whether the specified file exists.

```{{site.sbelang1}}
boolean #[IO exists.file(string path [, boolean environment])]
```
* path - Path to file
* environment - Use Environment PATH (Associated for current process).

Samples:

```{{site.sbelang}}
#[( #[IO exists.file("git.exe", true)] ){
   ...
}]
```

```{{site.sbelang}}
#[( #[IO exists.file("D:\tmp\data.log")] ){
   ...
}]
```

### copy

v0.12.6+

#### file

To copy selected file to the destination. Creates the destination path if not exists.

```{{site.sbelang1}}
void copy.file((string src | object srclist), string dest, bool overwrite [, object except])
```

Arguments:

* src - Source file. May contain mask as **.dll, **.*, ...
* srclist - **v0.12.8+** List of source files as {\"f1\", \"path\\*.dll\", ..}
* dest - The destination path. May contain path to file (only when used `src` instead of `srclist`) or directory (end with \ or /).
* overwrite - Overwrite file/s if already exists.
* except - List of files to exclude from input source as {"f1", "path\\*.dll", ...}

Samples:

```{{site.sbelang1}}
#[IO copy.file("bin\release.7z", "$(out)dep\release.7z", true)]
```

```{{site.sbelang1}}
#[IO copy.file("D:\inc\*.h", "$(SolutionDir)inc/", false, {"ui.core.h", "http.h"})]
```

```{{site.sbelang1}}
#[IO copy.file({"bin\client.zip", "bin\server.zip"}, "$(out)dep\release", true)]
```

```{{site.sbelang1}}
#[IO copy.file({
                    "bin\client.zip", 
                    "bin\server\*.*"
               }, 
               "$(plugin)\beta", 
               true, 
               { 
                    "*debug*",
                    "*.pdb"
               })]
```

Examples:

* To rename file

```{{site.sbelang1}}
#[IO copy.file("bin\release.7z", "bin\release_[f5acf6f].7z", false)]
#[IO delete.files({"bin\release.7z"})]
```

#### directory

To copy selected directory and subdirectories to the destination.

```{{site.sbelang1}}
void copy.directory(string src, string dest, bool force [, bool overwrite])
```

Arguments:

* src - The source directory. Can be empty as new directory.
* dest - The destination directory.
* force - Create the destination path if not exists.
* overwrite - Overwrite files if already exists.

Samples:

```{{site.sbelang1}}
#[IO copy.directory("bin", "$(out)dep/mixed", true)]
```

Examples:

* To create empty directory:

```{{site.sbelang1}}
#[IO copy.directory("", "$(SolutionDir)bin\Releases", true)]
```

### delete

v0.12.6+

#### files

To delete selected files.

```{{site.sbelang1}}
void delete.files(object files [, object except])
```

Arguments:

* files - List of files to deletion as {"f1", "path\\*.dll", ..}
* except - List of files to exclude from input list.

Samples:

```{{site.sbelang1}}
#[IO delete.files({"$(out)dep\*.7z", "$(out)dep\std.zip"}, {"release.7z"})]
```

#### directory

To delete selected directory.

```{{site.sbelang1}}
void delete.directory(string dir, bool force)
```

Arguments:

* dir - Path to directory for deletion.
* force - To remove non-empty directories.

Samples:

```{{site.sbelang1}}
#[IO delete.directory("$(out)dep", true)]
```

### remote

v0.12.6+

Remote servers.

#### download

To download file from remote server.

```{{site.sbelang1}}
void #[IO remote.download(string addr, string output [, string user, string pwd])]
```
* addr - Full address to remote file. e.g.: ftp://... http://...
* output - Output file name.
* user - Username.
* pwd - Password.

Samples of addresses:

```text
ftp://192.168.17.04/non-api.png
ftp://192.168.17.04:2021/dir1/non-api.png
ftp://user1:mypass123@192.168.17.04:2021/dir1/non-api.png
https://www.nuget.org/api/v2/package/vsSBE.CI.MSBuild/1.5.1
http://example.com
```

Examples:

```{{site.sbelang1}}
#[IO remote.download("ftp://192.168.17.04:2021/dir1/non-api.png", "non-api.png", "user1", "mypass123")]
#[IO remote.download("https://nuget.org/api/v2/package/vsSBE.CI.MSBuild/1.5.1", "CIM.nupkg")]
#[IO remote.download("http://example.com", "example.com.html")]
```