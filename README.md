# easy-log

#### 介绍
一个便捷的业务分支构件模块。当你的业务需要同时支持A和B两种方式时，你可能用一个if..else...就解决了。但是当你需要同时支持持A、B、C、D..中业务时，难道你不觉得用if..else...来搞分支太辛苦了吗？而且，代码会变得又臭又长。这时候，你需要这样一个工具来简化你的业务。


#### 使用说明
##### 引入LogUtil对象
```
private static LogUtil logger = LoggerManager.getLogger(Demo.class);
```
##### 使用LogUtil对象
```
logger.info("这是一条日志！");   //支持输出List，Map，多个Object
```
##### Maven引用
```
<dependency>
    <groupId>io.github.xiaoyudeguang</groupId>
    <artifactId>easy-log</artifactId>
    <version>3.0.0-RELEASE</version>
</dependency>
```

##### 那么问题来了，日期采集器的采集能力体现在哪呢？采集的日志又去哪了？别急，我们定义这样一个类：
```
@Brancher(key = "db_log_handler", todo = { "数据库日志采集器" })
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
大致意思你也应该能猜出来了，doService()方法是真正的接收并处理日志的方法，需要使用者自己实现。而doFilter()方法是过滤条件，也就是DBLogHandler类能接收并处理什么样的数据（此处用到了easy-brancher，项目地址：https://gitee.com/xiaoyudeguang/easy-brancher）。