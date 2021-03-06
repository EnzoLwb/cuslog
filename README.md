##### `EnzoLwb/cuslog` 是个开箱即用日志包，基于 zap 包封装。具有如下特性：
 - 基本功能
   - [x] 能够将事件记录到文件中，而不是应用程序控制台
   - [x] 日志切割：（依赖file-rotatelogs ）
     - 不同级别的日志输出到不同的日志文件中
     - 按时间日期切割日志文件 
   - [x] 能够打印基本信息，如调用文件/函数名和行号，日志时间等
   - [x] 配置控制台颜色输出开关
   - [x] 支持不同的日志级别。例如INFO，DEBUG，ERROR等（官方log只有print、fatal、panic级别
   - [x] 兼容官方log 直接在import时 添加log别名
 - 高级功能
   - [x] 高性能结构化打印 方便Filebeat、Logstash Shipper等工具记录 ：借助zap的Logger 超高性能避免反射
   - [x] zap的hook功能
   - [x] 日志投递：投递到队列中，再利用elk组件 读取队列内容展示。(见example/mq示例)
   - [x] zap动态开关日志级别：无需重启服务，通过http请求等方式改变日志级别(见example/senior示例)

### Usage
```go
func main()  {
   //开箱即用
   // Debug、Info(with field)、Warnf、Errorw的使用 和zap一样
   log.Debug("This is a debug message")
   log.Info("This is a info message", log.Int32("int_key", 10))
   log.Warnf("This is a formatted %s message", "warn")
   log.Errorw("Message printed with Errorw", "X-Request-ID", "fbf54504-64da-4088-9b86-67824a7fb508")

    // logger配置 可有可无
    opts := &log.Options{
        Level:            "debug",
        Formatter:        "json",
        EnableColor:      false,
        DisableCaller:    true,
        OutputPaths:      []string{"test.log", "stdout"},
    }
   
    // 初始化全局logger后再使用也可
    log.Init(opts)
    defer log.Flush()
}
```
#### 更多功能介绍请查看example
example中的第三方(rabbitmq,redis,file-rotatelogs等内容 会从 go.mod 中删除。减少依赖)
#### 作者说
> 其实写到后来发现还是离不开zapcore，也不由感叹zap的设计和易用性，zap一生推！
> 
> 也可以当做是zap的学习记录吧 :blush: 之后我会把笔记放到里面

### Star！！ 求Star！
