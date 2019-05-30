# Help查看帮助信息

用help查看帮助信息

用help获取主命令和子命令的帮助信息

想要搞懂命令的基本语法，可以用：

`some_command --help`

或：

`some_command help`

甚至是命令下面的字参数，子命令的更详细的语法，可以用：

`some_command help sub_command`

往往可以输出对应的命令的帮助信息

举例：

## 嵌入式的uboot的语法
[U-Boot: Quick reference - CompuLab Wiki](https://compulab.co.il/workspace/mediawiki/index.php5/U-Boot_quick_reference)
> help - print online help

-> help xxx

可以输出子命令的具体的详细的语法

xxx是uboot的子命令，比如ls，cp，md等等

## iOS中包管理器Carthage的语法

[Carthage/Artifacts.md at master · Carthage/Carthage](https://github.com/Carthage/Carthage/blob/master/Documentation/Artifacts.md#cartfile)

[Carthage/Carthage: A simple, decentralized dependency manager for Cocoa](https://github.com/Carthage/Carthage#installing-carthage)

```bash
licrifandeMacBook-Pro:QorosSales crifan$ carthage help
Available commands:

   archive           Archives built frameworks into a zip that Carthage can use
   bootstrap         Check out and build the project's dependencies
   build             Build the project's dependencies
   checkout          Check out the project's dependencies
   copy-frameworks   In a Run Script build phase, copies each framework specified by a SCRIPT_INPUT_FILE environment variable into the built app bundle
   fetch             Clones or fetches a Git repository ahead of time
   help              Display general or command-specific help
   outdated          Check for compatible updates to the project's dependencies
   update            Update and rebuild the project's dependencies
   version           Display the current version of Carthage

licrifandeMacBook-Pro:QorosSales crifan$ carthage help build
Build the project's dependencies

[--configuration (string)]
the Xcode configuration to build

[--platform (platform)]
the platforms to build for (one of ‘all’, ‘Mac’, ‘iOS’, ‘watchOS’, 'tvOS', or comma-separated values of the formers except for ‘all’)

[--derived-data (string)]
path to the custom derived data folder

[--no-skip-current]
don't skip building the Carthage project (in addition to its dependencies)

[--color (color)]
whether to apply color and terminal formatting (one of ‘auto’, ‘always’, or ‘never’)

[--verbose]
print xcodebuild output inline

[--project-directory (string)]
the directory containing the Carthage project

[[]]
the dependency names to build
```

## 苹果的Swift编译器`swiftc`
之前在：

［已解决］Xcode项目编译出错：Command failed due to signal: Segmentation fault: 11

期间，注意到点苹果的swift语言的编译器swiftc，对应help结果是：

```bash
➜  crifanLib git:(master) swiftc --help
OVERVIEW: Swift compiler

USAGE: swiftc [options] <inputs>

MODES:
  -dump-ast              Parse and type-check input file(s) and dump AST(s)
  -dump-parse            Parse input file(s) and dump AST(s)
  -dump-scope-maps <expanded-or-list-of-line:column>
                         Parse and type-check input file(s) and dump the scope map(s)
  -dump-type-refinement-contexts
                         Type-check input file(s) and dump type refinement contexts(s)
  -emit-assembly         Emit assembly file(s) (-S)
  -emit-bc               Emit LLVM BC file(s)
...
  -parse                 Parse input file(s)
  -print-ast             Parse and type-check input file(s) and pretty print AST(s)
  -typecheck             Parse and type-check input file(s)

OPTIONS:
  -api-diff-data-file <path>
                          API migration data is from <path>
  -application-extension  Restrict code to those available for App Extensions
  -assert-config <value>  Specify the assert_configuration replacement. Possible values are Debug, Release, Unchecked, DisableReplacement.
...
  -whole-module-optimization
                          Optimize input files together instead of individually
  -Xcc <arg>              Pass <arg> to the C/C++/Objective-C compiler
  -Xlinker <value>        Specifies an option which should be passed to the linker
```

## sftp命令不了解，用help去查看有哪些命令

之前：

[【已解决】命令行中ftp操作上传到远程服务器crifan.com](http://www.crifan.comterminal_use_ftp_upload_file_to_remote_server_crifan_com)

期间，对于sftp的命令有哪些不是很了解，所以可以通过help去查看：

```bash
sftp> help
Available commands:
bye                                Quit sftp
cd path                            Change remote directory to 'path'
chgrp grp path                     Change group of file 'path' to 'grp'
chmod mode path                    Change permissions of file 'path' to 'mode'
chown own path                     Change owner of file 'path' to 'own'
df [-hi] [path]                    Display statistics for current directory or
                                   filesystem containing 'path'
exit                               Quit sftp
get [-afPpRr] remote [local]       Download file
reget [-fPpRr] remote [local]      Resume download file
reput [-fPpRr] [local] remote      Resume upload file
help                               Display this help text
lcd path                           Change local directory to 'path'
lls [ls-options [path]]            Display local directory listing
lmkdir path                        Create local directory
ln [-s] oldpath newpath            Link remote file (-s for symlink)
lpwd                               Print local working directory
ls [-1afhlnrSt] [path]             Display remote directory listing
lumask umask                       Set local umask to 'umask'
mkdir path                         Create remote directory
progress                           Toggle display of progress meter
put [-afPpRr] local [remote]       Upload file
pwd                                Display remote working directory
quit                               Quit sftp
rename oldpath newpath             Rename remote file
rm path                            Delete remote file
rmdir path                         Remove remote directory
symlink oldpath newpath            Symlink remote file
version                            Show SFTP version
!command                           Execute 'command' in local shell
!                                  Escape to local shell
?                                  Synonym for help
sftp>
```

