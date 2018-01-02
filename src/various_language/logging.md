# log日志

在写代码开发期间，往往会涉及到打印Log日志。

在此总结log相关的内容。

`日志` == `log` == `Logger` == `logging`

用来打印日志输出信息用于调试。

## log的等级
log日志有一些常见的登记，最基本的几种有：
* `info`: 用于输出一些阶段性的告知用户的信息，比如用户已成功登录之类的
* `debug`: 用于调试目的，而输出一些信息，比如打印某些变量的值
* `warn`/`warning`: 用于输出一些不正常的情况，需要引起用户注意了，比如app检测存储空间快满了
* `error`: 显示一些错误级别的信息，往往意味着程序也要终止运行了。需要用户处理错误，或者退出程序之类的

其他不是很常见的几种：
* `finest`/`fine`：表示程序正常级别输出的信息
* `trace`: 类似于debug，用于输出一些追踪调试信息
* `verbose`：事无巨细的，最详尽的信息
* `severe`：严重的问题，类似于error
* `critical`：关键的问题，类似于severe

## log相关的库

### log4xxx系列
有个通用的log的库：log4xxx

针对于不同语言的log4xxx分别是：

* Java语言：Log4j
  * [log4j](https://logging.apache.org/log4j/1.2/)
  * 新版本：[Log4j – Log4j 2 Guide - Apache Log4j 2](http://logging.apache.org/log4j/2.x/)
* C语言：log4c
  * [log4c](http://log4c.sourceforge.net)
* Go语言：log4go
  * [alecthomas/log4go: Logging package similar to log4j for the Go programming language](https://github.com/alecthomas/log4go)
* 微软的.NET框架（C#等语言）：[log4net](https://logging.apache.org/log4net/)
  * [Open Source Logging Tools in C#](https://csharp-source.net/open-source/logging)
* Delphi语言：Log4Delphi
  * [Welcome to Log4Delphi](http://log4delphi.sourceforge.net)
* C++: [log4cpp](http://log4cpp.sourceforge.net/)
  * 其他相关：
    * [log4cplus](https://sourceforge.net/projects/log4cplus/)
    * [log4cxx](https://logging.apache.org/log4cxx/)

### Python
Python的log库：自带的库[logging](https://docs.python.org/2/library/logging.html)

自己也整理和封装成基本的函数：

```python
def loggingInit(filename = None,
                fileLogLevel = logging.DEBUG,
                fileLogFormat = '%(asctime)s LINE %(lineno)-4d %(levelname)-7s %(message)s',
                fileLogDateFormat = '%Y/%m/%d %I:%M:%S',
                enableConsole = True,
                consoleLogLevel = logging.INFO,
                consoleLogFormat = "%(asctime)s LINE %(lineno)-4d %(levelname)-7s %(message)s",
                consoleLogDateFormat = '%Y/%m/%d %I:%M:%S',
                ):
    """
    init logging for both log to file and console
    :param logFilename: input log file name
        if not passed, use current script filename
    :return: none
    """
    logFilename = ""
    if filename:
        logFilename = filename
    else:
        logFilename = getInputFileBasenameNoSuffix() + ".log"

    logging.basicConfig(
                    level    = fileLogLevel,
                    format   = fileLogFormat,
                    datefmt  = fileLogDateFormat,
                    filename = logFilename,
                    filemode = 'w')
    if enableConsole :
        # define a Handler which writes INFO messages or higher to the sys.stderr
        console = logging.StreamHandler()
        console.setLevel(consoleLogLevel)
        # set a format which is simpler for console use
        formatter = logging.Formatter(fmt=consoleLogFormat, datefmt=consoleLogDateFormat)
        # tell the handler to use this format
        console.setFormatter(formatter)
        logging.getLogger('').addHandler(console)
```

详见：
`https://github.com/crifan/crifanLib/blob/master/python/crifanLib.py`

#### Flask

Python的Flask中也可以打印log

详见：
* ［未解决］Flask中给app的输出到命令行中的debugger添加函数名
* ［已解决］Flask中输出log日志到文件且自定义输出格式

### Swift

iOS的swift中也遇到过log库：[XCGLogger](https://github.com/DaveWoodCom/XCGLogger)

其中的log的定义是：
```swift
    /// Enum defining our log levels
    public enum Level : Int, Comparable, CustomStringConvertible {
        case verbose
        case debug
        case info
        case warning
        case error
        case severe
        case none

        ...
        public var description: String { get }
        public static let all: [XCGLogger.XCGLogger.Level]
    }
```

和基本的封装

```swift
import XCGLogger

func initLog() {
    gLog = XCGLogger.default//XCGLogger.defaultInstance()
    
    //var logLevel:XCGLogger.LogLevel = XCGLogger.LogLevel.debug

    var logLevel:XCGLogger.Level = XCGLogger.Level.debug
    if LogLevelIsVerbose {
        logLevel = XCGLogger.Level.verbose
    }
    
    var fileLogLevel:XCGLogger.Level = XCGLogger.Level.debug
    if FileLogLevelIsVerbose {
        fileLogLevel = XCGLogger.Level.verbose
    }
    
    gLog.setup(
        level: logLevel,
        showThreadName: true,
        showLevel: true,
        showFileNames: true,
        showLineNumbers: true,
        writeToFile: LogFile,
        fileLevel: fileLogLevel)
}
```

之后即可正常log了：

```swift
  gLog.info("UIScreen.mainScreen().bounds=\(UIScreen.main.bounds)")
  gLog.debug("forgetPasswordVC:\(forgetPasswordVC)”)
  gLog.warning("jpush set alias \(alias) failed")
  gLog.error("上传附件失败: \(encodingError)”)
```


