# 应用云 Crashlytics iOS 编程手册

## 服务启动与停止

当您集成了 Crashlytics 服务之后，系统将会在程序启动的时候自动启动服务。

如果您不希望在启动的时候默认启动 Crashlytics 服务，您可以在配置中设置关掉 (例如在AppDelegate中加入如下代码)：

~~~
    TACApplicationOptions* options = [TACApplicationOptions defaultApplicationOptions];
    options.crashOptions.enable = NO;
~~~

## 设置 Crashlytics 委托

> 您可以通过设置 delegate 来提供更多的信息以辅助定位分析问题

~~~
[TACCrashService shareService].delegate = <#the instance of TACCrashServiceDelegate#>
~~~

其中协议 TACCrashServiceDelegate 实现拥有以下接口：

~~~
@optional
/**
 当放生异常的时候，需要附带上的环境信息。您可以通过该接口提供更多的信息以辅助您定位问题。

 @param service 当前的Crash服务实例
 @param exception 异常信息
 */
- (NSString*) crashService:(TACCrashService*)service  attachmentForException:(NSException*)exception;
~~~

## 手动上报异常


~~~
/**
 上报异常信息

 @param exception 异常
 */
- (void) reportException:(NSException*)exception;

~~~

## 定制 崩溃 服务

### 开启卡顿监控

卡顿监控用于监控主线程卡顿问题，默认关闭。

~~~

/**
 *  卡顿监控开关，默认关闭
 */
 options.crashOptions.blockMonitorEnable = YES;
~~~

## 用户策略行为

### 设置标签
自定义标签，用于标明App的某个“场景”。在发生Crash时会显示该Crash所在的“场景”，以最后设置的标签为准，标签id需大于0。例：当用户进入界面A时，打上9527的标签：

```
[TACCrashService shareService].userSenceTag =999;
```

打标签之前，需要在Bugly产品页配置中添加标签，取得标签ID后在代码中上报。

### 设置自定义Map参数

自定义Map参数可以保存发生Crash时的一些自定义的环境信息。在发生Crash时会随着异常信息一起上报并在页面展示。

```
[[TACCrashService shareService] setUserValue:@"value" forKey:@"key"];
```

最多可以有9对自定义的key-value（超过则添加失败）；
key限长50字节，value限长200字节，过长截断；
key必须匹配正则：[a-zA-Z[0-9]]+。


## 其他功能

其他功能请参考 TACCrashService.h 中的定义。
