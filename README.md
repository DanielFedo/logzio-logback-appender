# Logzio logback appender
This appender sends logs to your [Logz.io](http://logz.io) account, using non-blocking threading, bulks, and HTTPS encryption.

### Technical Information
This appender uses [BigQueue](https://github.com/bulldog2011/bigqueue) implementation of persistent queue, so all logs are backed up to a local file system before being sent. Once you send a log, it will be enqueued in the buffer and 100% non-blocking. There is a background task that will handle the log shipment for you.

### Installation from maven
```xml
<dependency>
    <groupId>io.logz.logback</groupId>
    <artifactId>logzio-logback-appender</artifactId>
    <version>1.0.0</version>
</dependency>
```

### Logback Example Configuration
```xml
<configuration debug="true" scan="true" scanPeriod="30 seconds">
    <appender name="LogzioLogbackAppender" class="io.logz.logback.LogzioLogbackAppender">
        <encoder>
            <pattern>%d{yy/MM/dd HH:mm:ss} {%t} %p %c{2}: %m</pattern>
        </encoder>
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>DEBUG</level>
        </filter>
        <token>yourlogziopersonaltokenfromsettings</token>
        <type>myAwesomeType</type>
    </appender>
</configuration>
```

### Parameters
| Parameter          | Default                              | Explained  |
| ------------------ | ------------------------------------ | ----- |
| **token**              | *None*                                 | Your Logz.io token, which can be found under "settings" in your account |
| **type**               | *java*                                 | The [log type](http://support.logz.io/support/solutions/articles/6000103063-what-is-type-) for that appender |
| **drainTimeoutSec**       | *5*                                    | How often the appender should drain the buffer (in seconds) |
| **fileSystemFullPercentThreshold** | *98*                                   | The percent of used file system space at which the appender will stop buffering. When we will reach that percentage, the file system in which the buffer rests will drop all new logs until the percentage of used space drops below that threshold. Set to -1 to never stop processing new logs |
| **bufferDir**          | *System.getProperty("java.io.tmpdir")* | Where the appender should store the buffer |
| **socketTimeout**       | *10 * 1000*                                    | The socket timeout during log shipment |
| **connectTimeout**       | *10 * 1000*                                    | The connection timeout during log shipment |
| **debug**       | *false*                                    | Print some debug messages to stdout to help to diagnose issues |


### Code Example
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class LogzioLogbackExample {

    public static void main(String[] args) {
        Logger logger = LoggerFactory.getLogger(LogzioLogbackExample.class);
        
        logger.info("Testing logz.io!");
        logger.warn("Winter is coming");
    }
}
```

### Contribution
 - Fork
 - Code
 - ```mvn test```
 - Issue a PR :)
