# 编译和链接

此处介绍编译和链接方面的通用的知识、概念和逻辑。

之前接触过嵌入式和Linux，断断续续接触过很多编译和链接的东西，主要是命令行界面中用`makefile`、`gcc`等各种工具去做编译和链接的工作。

后来又接触过移动端开发，包括`Xcode`等IDE集成开发环境

> #### info::编辑器和IDE
> 
> 详见：【整理Book】文本编辑器和IDE集成开发环境

发现其实IDE背后都是利用了编译和链接方面的通用知识。

对此编译和链接方面的通用的知识、逻辑、概念，去尝试总结，以供参考。

## Xcode中底层是用swiftc去编译swift代码

之前在：

［已解决］Xcode项目编译出错：Command failed due to signal: Segmentation fault: 11

期间，通过错误的log中的：

```bash
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swiftc -incremental -module-name XxxApp -Onone -sdk /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator9.3.sdk -target i386-apple-ios9.0 -g -module-cache-path /Users/crifan/Library/Developer/Xcode/DerivedData/ModuleCache -Xfrontend -serialize-debugging-options -enable-testing -I /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Products/Debug-iphonesimulator -F /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Products/Debug-iphonesimulator -F /Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS/Carthage/Build/iOS -c -j4 /Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS/XxxApp/CrifanLibHttp.swift
...
/Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS/XxxApp/CrifanLibDemo.swift /Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS/XxxApp/AddTaskViewController.swift -output-file-map /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/Objects-normal/i386/XxxApp-OutputFileMap.json -parseable-output -serialize-diagnostics -emit-dependencies -emit-module -emit-module-path /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/Objects-normal/i386/XxxApp.swiftmodule -Xcc -I/Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/swift-overrides.hmap -Xcc -iquote -Xcc /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/XxxApp-generated-files.hmap -Xcc -I/Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/XxxApp-own-target-headers.hmap 
...
-Xcc -I/Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Products/Debug-iphonesimulator/include -Xcc -I/Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/DerivedSources/i386 -Xcc -I/Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/DerivedSources -Xcc -DDEBUG=1 -emit-objc-header -emit-objc-header-path /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Intermediates/XxxApp.build/Debug-iphonesimulator/XxxApp.build/Objects-normal/i386/XxxApp-Swift.h -import-objc-header /Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS/XxxApp/XxxApp-Bridging-Header.h -Xcc -working-directory/Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS
```

而知道：

用的swiftc这个编译器，传递的参数是：

* 要编译的是：各个swift文件
* 输出是：XxxApp
* 期间利用到的各种：
  * SDK库是：`-sdk /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator9.3.sdk`
  * include的头文件的路径是：`-I xxx`

等等

即：

对于Xcode这个IDE，你点击了编译按钮后，内部的实际过程是：

调用swiftc这个编辑器去swift代码，期间涉及到

* 引用哪些库
* 设置哪些编译参数
* 引用include哪些头文件: xxx.h
* 包含哪些路径
  * 去哪些路径中寻找头文件：-I xxx
  * 去哪些路径中寻找所要包含的库文件:-L xxx
* 设置哪些环境变量
  * 供后续过程去调用

-> 而这些都是编译和链接方面的通用知识。

-> 而其他的IDE的功能的内部过程大同小异，只不过用了不同的编译器，链接参数不同而已。

-> 而编译链接方面的知识，越是接近于底层开发，越是接触的多，理解的深入，比如嵌入式开发，Linux开发等等

-> 而对于编译和链接这方面的知识，如果之前折腾过嵌入式，Linux开发等方面的内容，则更能深切的体会到各种编译器、等内容。

## Xcode中通过设置头文件搜索路径而解决了头文件引用找不到的问题

比如：

[【已解决】Xcode9代码出错：Realm/Realm.h file not found with angled include use quotes instead](http://www.crifan.com/xcode_9_ream_h_file_not_found_with_angled_include_use_quotes_instead)

期间，对于错误提示：

`<Realm/Realm.h> file not found with angled include use quotes instead`

如果对于编译链接知识不了解的话，则很容易被误导，而去一个个的修改（对应的几十个）swift文件，把：

`<Realm/Realm.h>`

改为：

`Realm/Realm.h`

但是却没能从根本上解决问题。实际上是，搜索头文件的路径却少了，需要去设置：

`Headers Search Paths`

把Realm的头文件的路径，添加进去，即可真正彻底解决头文件找不到的问题。

## Xcode中如何引入已改名的和已改变链接方式的库
之前在：

[【已解决】Xcode9.2编译出错：ld library not found for -lMobClickLibrary](http://www.crifan.com/xcode_9_link_fail_ld_library_not_found_for_i_mobclicklibrary)

就遇到：

一个第三方库：友盟统计

* 之前是3.6.6的的静态库：libMobClickLibrary.a
* 现在版本升级为4.0.1，且功能拆分为2个库文件了，且是动态库：
  * 统计功能的Analytics：UMMobClick.framework
  * 社交功能的Social

所以，搞清楚这点后，才知道去把Xcode中的：
`Build Settings-》Linking-〉Other Linker Flags`
中的：
`-l"MobClickLibrary"`
改为：
`-framework "UMMobClick"`
表示：
引入新的`UMMobClick`的`framework`动态库，解决：
`ld library not found for -lMobClickLibrary`
的问题。

## 苹果的swiftc编译器和`-j4`多线程编译

之前在：

［已解决］Xcode项目编译出错：Command failed due to signal: Segmentation fault: 11

期间，看到错误的log中的：

```bash
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swiftc -incremental -module-name XxxApp -Onone -sdk /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/SDKs/iPhoneSimulator9.3.sdk -target i386-apple-ios9.0 -g -module-cache-path /Users/crifan/Library/Developer/Xcode/DerivedData/ModuleCache -Xfrontend -serialize-debugging-options -enable-testing -I /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Products/Debug-iphonesimulator -F /Users/crifan/Library/Developer/Xcode/DerivedData/XxxApp-fxzzwvtqreanqsgzqmztnatyyjjd/Build/Products/Debug-iphonesimulator -F /Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS/Carthage/Build/iOS -c -j4 /Users/crifan/dev/dev_root/daryun/Projects/Xxx/Zzz/Sourcecode/XxxAppiOS/XxxApp/CrifanLibHttp.swift
```

其中有`-j4`，猜测估计是类似于之前Linux中`makefile`中的`make`的`-j4`，估计是**4个线程**去并行执行的效果。

所以先去看看对应的编译器
`/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/swiftc`

结果是：

```bash
➜  crifanLib git:(master) swiftc --version
Apple Swift version 4.0.3 (swiftlang-900.0.74.1 clang-900.0.39.2)
Target: x86_64-apple-macosx10.9
```

果然是苹果的Swift语言的编译器。然后再去：

```bash
➜  crifanLib git:(master) which swiftc
/usr/bin/swiftc
```

可以看到，此处swiftc编译器是放在常见的`/usr/bin`目录下面的

再去通过：

```bash
➜  crifanLib git:(master) swiftc --help
...
  -I <value>              Add directory to the import search path
  -j <n>                  Number of commands to execute in parallel
  -L <value>              Add directory to library link search path
  -l<value>               Specifies a library which should be linked against
...
  -Xcc <arg>              Pass <arg> to the C/C++/Objective-C compiler
  -Xlinker <value>        Specifies an option which should be passed to the linker
```

中的：
`  -j <n>                  Number of commands to execute in parallel`

可以看到：
`-jN`表示用多少个命令并行执行

-> 和Make中的-j是类似的效果。

-> 另外，此处也说明了对于Xcode这个IDE，内部编译代码，链接库等操作，底层对应的逻辑，都是**编译链接**方面的知识，都是通用的。

-> 只不过用了不同的编译器，链接参数而异。

-> 而对于编译和链接这方面的知识，如果之前折腾过嵌入式，Linux开发等方面的内容，则更能深切的体会到各种编译器、库函数、参数、头文件、包含路径、环境变量等内容。



