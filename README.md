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
    <version>3.0.0-RELEASE</version>
</dependency>
```

## 日志处理器
那么问题来了，日期采集器的采集能力体现在哪呢？采集的日志又去哪了？别急，我们定义这样一个类：
```
@Brancher(key = "db_log_handler", todo = { "数据库日志处理器" })
public class DBLogHandler extends AbstractBrancher{

	@Override
	public Object doService(EasyMap params) {
		List<LogInfo> infos = params.get(LoggerManager.KEY);
		System.out.println(infos);
		return null;
	}

	@Override
	public boolean doFilter(String key, EasyMap params) {
		return super.doFilter(key, params)&&params.have(LoggerManager.KEY);
	}
}
```
大致意思你也应该能猜出来了，doService()方法是真正的接收并处理日志的方法，需要使用者自己实现。而doFilter()方法是过滤条件，也就是DBLogHandler类能接收并处理什么样的数据。你可以定义多个日志处理类，都用上述的doFilter()方法即可，注意@Brancher注解的key不能一样。
（此处用到了easy-brancher，项目地址：https://gitee.com/xiaoyudeguang/easy-brancher）。

## 日志处理器模板

这是一波福利。你可以把下面这个类复制到你的系统，，然后让你的日志处理器类继承它就可以了。这样，就不用再每个日志处理器类里都写一遍doFilter()方法了。
```
public abstract class AbstractLogHandler extends AbstractBrancher{

	@Override
	public boolean doFilter(String key, EasyMap params) {
		return super.doFilter(key, params)&&params.have(LoggerManager.KEY);
	}
}
```
