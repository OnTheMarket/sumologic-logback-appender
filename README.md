# sumologic-logback-appender

A Logback appender that sends straight to Sumo Logic.

For the original Log4j appender, please see https://github.com/SumoLogic/sumo-log4j-appender

For the Log4j2 appender, please see https://github.com/SumoLogic/sumologic-log4j2-appender

## Installation

The library can be added to your project using Maven Central by adding the following dependency to a POM file:

```
<dependency>
    <groupId>com.sumologic.plugins.logback</groupId>
    <artifactId>sumologic-logback-appender</artifactId>
    <version>1.0</version>
</dependency>
```

## Usage

### Set up HTTP Hosted Collector Source in Sumo Logic

Follow these instructions for [setting up an HTTP Source](http://help.sumologic.com/Send_Data/Sources/HTTP_Source) in Sumo Logic.

### Logback XML Configuration
Be sure to replace [collector-url] with the URL after creating an HTTP Hosted Collector Source in Sumo Logic.

`logback.xml`:

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="SumoAppender" class="com.sumologic.logback.SumoLogicAppender">
        <encoder>
            <Pattern>
                %date{ISO8601} [%t] %-5p %c - %m%n
            </Pattern>
        </encoder>
        <url>[collector-url]</url>
    </appender>
    <Logger name="SumoLogger" level="info">
        <appender-ref ref="SumoAppender" />
    </Logger>
</configuration>
```

### Parameters
| Parameter             | Required? | Default Value | Description                                                                                                                                |
|-----------------------|----------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| encoder               | Yes      |               | Encoder to format the log message. This will usually specify a Pattern.                                                                                       |
| url                   | Yes      |               | HTTP collection endpoint URL                                                                                                               |
| sourceName            | No       | "Http Input"              | Source name to appear when searching on Sumo Logic by `_sourceName`                                                                                                        |
| sourceHost            | No       | Client IP Address              | Source host to appear when searching on Sumo Logic by `_sourceHost`                                                                                                         |
| sourceCategory        | No       | "Http Input"              | Source category to appear when searching on Sumo Logic by `_sourceCategory`                                                                                                         |
| proxyHost             | No       |               | Proxy host IP address                                                                                                                      |
| proxyPort             | No       |               | Proxy host port number                                                                                                                     |
| proxyAuth             | No       |               | For basic authentication proxy, set to "basic". For NTLM authentication proxy, set to "ntlm". For no authentication proxy, do not specify. |
| proxyUser             | No       |               | Proxy host username for basic and NTLM authentication. For no authentication proxy, do not specify.                                        |
| proxyPassword         | No       |               | Proxy host password for basic and NTLM authentication. For no authentication proxy, do not specify.                                        |
| proxyDomain           | No       |               | Proxy host domain name for NTLM authentication only                                                                                        |
| retryIntervalMs         | No       | 10000         | Retry interval (in ms) if a request fails                                                                                                  |
| connectionTimeoutMs     | No       | 1000          | Timeout (in ms) for connection                                                                                                             |
| socketTimeoutMs         | No       | 60000         | Timeout (in ms) for a socket                                                                                                               |
| messagesPerRequest    | No       | 100           | Number of messages needed to be in the queue before flushing                                                                               |
| maxFlushIntervalMs      | No       | 10000         | Maximum interval (in ms) between flushes                                                                                                   |
| flushingAccuracyMs      | No       | 250           | How often (in ms) that the flushing thread checks the message queue                                                                        |
| maxQueueSizeBytes     | No       | 1000000       | Maximum capacity (in bytes) of the message queue
| flushAllBeforeStopping| No       | true         | Flush all messages before stopping regardless of flushingAccuracyMs Be sure to call `loggerContext.stop();` when your application stops.

#### Example with Optional Parameters

`logback.xml`:

```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <appender name="SumoAppender" class="com.sumologic.logback.SumoLogicAppender">
        <encoder>
            <Pattern>
                %date{ISO8601} [%t] %-5p %c - %m%n
            </Pattern>
        </encoder>
        <url>[collector-url]</url>
        <messagesPerRequest>10000</messagesPerRequest>
        <maxFlushIntervalMs>1000</maxFlushIntervalMs>
        <flushingAccuracyMs>100</flushingAccuracyMs>
        <sourceName>mySource</sourceName>
        <sourceHost>myHost</sourceHost>
        <sourceCategory>myCategory</sourceCategory>
        <flushAllBeforeStopping>true</flushAllBeforeStopping>
    </appender>
    <Logger name="SumoLogger" level="info">
        <appender-ref ref="SumoAppender" />
    </Logger>
</configuration>
```

### TLS Support

In keeping with industry standard security best practices, as of May 31, 2018, the Sumo Logic service will only support TLS version 1.2 going forward. Verify that all connections to Sumo Logic endpoints are made from software that supports TLS 1.2.


## Development

To compile the plugin:
- Run "mvn clean package" on the pom.xml in the main level of this project.
- To test running a locally built JAR file, you may need to manually add the following dependencies to your project:
```
    <dependencies>
        <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>1.2.3</version>
        </dependency>

        <dependency>
            <groupId>com.sumologic.plugins.http</groupId>
            <artifactId>sumologic-http-core</artifactId>
            <version>1.0</version>
        </dependency>
    </dependencies>
```

## License

The Sumo Logic Logback Appender is published under the Apache Software License, Version 2.0. Please visit http://www.apache.org/licenses/LICENSE-2.0.txt for details.
