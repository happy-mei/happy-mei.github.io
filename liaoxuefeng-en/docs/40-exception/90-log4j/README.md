<!-- TRANSLATED by md-translate -->
# Using Log4j

Previously introduced Commons Logging, can be used as a "logging interface". The real "logging implementation" can use Log4j.

Log4j is a very popular logging framework, the latest version is 2.x.

Log4j is a componentized design of the logging system, its architecture is roughly as follows:

```ascii
log.info("User signed in.");
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 ├──▶│ Appender │───▶│  Filter  │───▶│  Layout  │───▶│ Console  │
 │   └──────────┘    └──────────┘    └──────────┘    └──────────┘
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 ├──▶│ Appender │───▶│  Filter  │───▶│  Layout  │───▶│   File   │
 │   └──────────┘    └──────────┘    └──────────┘    └──────────┘
 │
 │   ┌──────────┐    ┌──────────┐    ┌──────────┐    ┌──────────┐
 └──▶│ Appender │───▶│  Filter  │───▶│  Layout  │───▶│  Socket  │
     └──────────┘    └──────────┘    └──────────┘    └──────────┘
```

When we output a log using Log4j, Log4j automatically outputs the same log to different destinations through different Appenders. For example:

* console: output to screen;
* file: output to file;
* socket: output to a remote computer over a network;
* jdbc: output to database

In the process of outputting logs, Filter is used to filter which logs need to be output and which logs do not need to be output. For example, only `ERROR` level logs are output.

Finally, the log information is formatted via Layout, e.g., by automatically adding information such as date, time, and method name.

The above structure is complex, but we don't need to care about the Log4j API when we actually use it, but rather configure it through configuration files.

Taking XML configuration as an example, when using Log4j, we put a `log4j2.xml` file under `classpath` to allow Log4j to read the configuration file and output logs according to our configuration. Here is an example of a configuration file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
    <Properties>
        <!-- 定义日志格式 -->
    	<Property name="log.pattern">%d{MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36}%n%msg%n%n</Property>
        <!-- 定义文件名变量 -->
    	<Property name="file.err.filename">log/err.log</Property>
    	<Property name="file.err.pattern">log/err.%i.log.gz</Property>
    </Properties>
    <!-- 定义Appender，即目的地 -->
    <Appenders>
        <!-- 定义输出到屏幕 -->
    	<Console name="console" target="SYSTEM_OUT">
            <!-- 日志格式引用上面定义的log.pattern -->
    		<PatternLayout pattern="${log.pattern}" />
    	</Console>
        <!-- 定义输出到文件,文件名引用上面定义的file.err.filename -->
    	<RollingFile name="err" bufferedIO="true" fileName="${file.err.filename}" filePattern="${file.err.pattern}">
    		<PatternLayout pattern="${log.pattern}" />
    		<Policies>
                <!-- 根据文件大小自动切割日志 -->
    			<SizeBasedTriggeringPolicy size="1 MB" />
    		</Policies>
            <!-- 保留最近10份 -->
    		<DefaultRolloverStrategy max="10" />
    	</RollingFile>
    </Appenders>
    <Loggers>
    	<Root level="info">
            <!-- 对info级别的日志，输出到console -->
    		<AppenderRef ref="console" level="info" />
            <!-- 对error级别的日志，输出到err，即上面定义的RollingFile -->
    		<AppenderRef ref="err" level="error" />
    	</Root>
    </Loggers>
</Configuration>
```

Although configuring Log4j is a bit tedious, once it is configured, it is very easy to use. For the above configuration file, all `INFO` level logs will be automatically output to the screen, while `ERROR` level logs will not only be output to the screen, but also to the file at the same time. And, once the log file reaches the specified size (1MB), Log4j will automatically cut the new log file and keep up to 10 copies.

Having a configuration file is not enough, because Log4j is also a third-party library, we need to download Log4j from [here](https://logging.apache.org/log4j/2.x/download.html), unzip it, and put the following 3 jar packages into the `classpath`:

* log4j-api-2.x.jar
* log4j-core-2.x.jar
* log4j-jcl-2.x.jar

Since Commons Logging automatically discovers and uses Log4j, put the `commons-logging-1.2.jar` you downloaded in the previous section into the `classpath` as well.

To print the logs, just write it the way Commons Logging writes it, without changing any code, to get Log4j log output, similarly:

```plain
03-03 12:09:45.880 [main] INFO com.itranswarp.learnjava.Main
Start process...
```

### Best practices

In the development phase, always use the Commons Logging interface to write logs, and there is no need to introduce Log4j in the development phase. if you need to write logs to a file, you just need to put the correct configuration file and Log4j-related jar packages into the `classpath`, and then you can automatically switch the logs to be written using Log4j, without modifying any code.

### Exercise

Observe the log files written by Log4j according to the configuration file.

[Download exercise](logging-log4j.zip)

### Summary

Logging is implemented through Commons Logging and no code changes are required to use Log4j;

To use Log4j you just need to put log4j2.xml and related jar into the classpath;

To replace Log4j, simply remove log4j2.xml and the associated jar;

Only when you extend Log4j, you need to refer to the Log4j interface (for example, the function of writing logs encrypted to the database needs to be developed by yourself).