# easy-log

## 介绍
一个实用的日志采集器。你可以向以往使用日志模块一样输出日志，不同的是，如果引进了easy-log日志采集器模块，你的日志不再像以往一样只是日志了。easy-log会把系统运行期间产生的所有日志储存起来并发送出去，你可以自定义多个日志处理器来接收并处理日志，比如放到数据库指定表，比如利用mq发送到指定服务器下，这样，你再也不需要远程线上服务器找日志bug了。码云地址：https://gitee.com/xiaoyudeguang/easy-log

## 使用说明
##### 引入LogUtil对象
```
private static LogUtil logger = LoggerManager.getLogger(Demo.class);
```
##### 使用LogUtil对象
```
logger.info("这是一条日志！");   //支持输出List，Map，Object数组
```
## Maven引用(如果版本有变化，请自行去maven中央仓库引用)
```
<dependency>
    <groupId>io.github.xiaoyudeguang</groupId>
    <artifactId>easy-log</artifactId>
    <version>3.0.2-RELEASE</version>
</dependency>
```

## 日志处理器
那么问题来了，日志采集器的采集能力体现在哪呢？采集的日志又去哪了？别急，我们定义这样一个(或多个)类（此处用到了easy-brancher，项目地址：https://gitee.com/xiaoyudeguang/easy-brancher）：
```
@Brancher(key = "db_log_handler", todo = { "数据库日志处理器" }, args = {LogConfig.KEY})
public class DBLogHandler extends AbstractLogBrancher{
	
	@Override
	public Object doService(List<LogInfo> infos) {
		Console.log("日志", infos);
		return "我最棒！";
	}

	@Override
	public void addLogConfig(EasyList keys) {
		keys.add("db_log_handler");
	}
}
```
> 自定义的日志处理器需要继承easy-log提供的AbstractLogBrancher抽象类。需要实现两个方法：doService()方法是真正的接收并处理日志的方法；addLogConfig()方法中将自定义的日志处理器中的@Brancher注解中的key参数注册到easy-log中，这样就可以在doService()方法接收日志信息了。
