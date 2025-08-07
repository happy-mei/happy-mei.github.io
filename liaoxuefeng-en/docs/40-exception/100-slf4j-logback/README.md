<!-- TRANSLATED by md-translate -->
# Using SLF4J and Logback

Previously introduced Commons Logging and Log4j this pair of good friends, they are responsible for acting as a logging API, one is responsible for the implementation of the logging layer, with the use of very easy to develop.

Some of you may have heard of SLF4J and Logback, which also look like logs, but what are they?

In fact, SLF4J is similar to Commons Logging, which is also a logging interface, and Logback is similar to Log4j, which is a logging implementation.

Why is it that with Commons Logging and Log4j, SLF4J and Logback pop up? It's because Java has a very long history of open source, not only is the OpenJDK itself open source, but also the third party libraries that we use, almost all of which are open source. One particular aspect of the rich open source ecosystem is that you can find several competing open source libraries for the same function.

Because of dissatisfaction with Commons Logging's interface, someone got SLF4J. Because of dissatisfaction with Log4j's performance, someone got Logback.

Let's first look at how SLF4J has improved the interface to Commons Logging. In Commons Logging, we want to print the log, and sometimes we have to write it like this:

```java
int score = 99;
p.setScore(score);
log.info("Set score " + score + " for Person " + p.getName() + " ok.");
```

Spelling strings is a real pain in the ass, so SLF4J's logging interface was improved to look like this:

```java
int score = 99;
p.setScore(score);
logger.info("Set score {} for Person {} ok.", score, p.getName());
```

We can guess that SLF4J's logging interface passes in a string with a placeholder, and automatically replaces the placeholder with the variable that follows it, so it looks more natural.

How to use SLF4J Its interface is actually almost exactly the same as Commons Logging:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

class Main {
    final Logger logger = LoggerFactory.getLogger(getClass());
}
```

Compare the interfaces of Commons Logging and SLF4J:

| Commons Logging | SLF4J |
|---------------------------------------|-------------------------|
| org.apache.commons.logging.Log | org.slf4j.Logger | org.apache.commons.logging.
| org.apache.commons.logging.LogFactory | org.slf4j.LoggerFactory |

The difference is that `Log` becomes `Logger` and `LogFactory` becomes `LoggerFactory`.

The use of SLF4J and Logback is similar to the use of Commons Logging and Log4j as mentioned earlier, first download [SLF4J](https://www.slf4j.org/download.html) and [Logback](https://logback.qos.ch /download.html), and then put the following jar package to the classpath:

* slf4j-api-1.7.x.jar
* logback-classic-1.2.x.jar
* logback-core-1.2.x.jar

Then just use SLF4J's `Logger` and `LoggerFactory`.

Similar to Log4j, we still need a Logback configuration file, put `logback.xml` under the classpath and configure it as follows:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
    	<encoder>
    		<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    	</encoder>
    </appender>

    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
    	<encoder>
    		<pattern>%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
    		<charset>utf-8</charset>
    	</encoder>
    	<file>log/output.log</file>
    	<rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
    		<fileNamePattern>log/output.log.%i</fileNamePattern>
    	</rollingPolicy>
    	<triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
    		<MaxFileSize>1MB</MaxFileSize>
    	</triggeringPolicy>
    </appender>

    <root level="INFO">
    	<appender-ref ref="CONSOLE" />
    	<appender-ref ref="FILE" />
    </root>
</configuration>
```

Run it to get an output similar to the following:

```plain
13:15:25.328 [main] INFO com.itranswarp.learnjava.Main - Start process...
```

From the current trend , more and more open source projects from Commons Logging plus Log4j to SLF4J plus Logback .

### Exercise

Observe the log files written by Logback according to the configuration file.

[Download exercise](logging-slf4j.zip)

### Summary

SLF4J and Logback can replace Commons Logging and Log4j;

Always use SLF4J's interface to write logs, using Logback requires only configuration and no code changes.