## 시간, 용량, 히스토리 롤링 정책
```
<appender name="DEBUG-ROLLING-FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
	<file>/data/logs/service/${server.port}/recommend_debug.log</file>
	<rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
		<fileNamePattern>/data/logs/service/${server.port}/%d{yyyyMMdd,aux}/recommend_debug.%d{yyyy-MM-dd_HH_mm}.%i.log.gz</fileNamePattern>

		<!-- fileNamePattern 에서 %i 0,1,2,3,.. 으로 롤링되는 트리거 정책으로 용량 제한 정책 사용 -->
		<timeBasedFileNamingAndTriggeringPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedFNATP">
			<maxFileSize>100MB</maxFileSize>
	</timeBasedFileNamingAndTriggeringPolicy>

	<!-- fileNamePattern 기준으로 최대 히스트리 보관 개수 제한 -->
		<maxHistory>3</maxHistory>

		<!-- 롤링한 압축 파일들의 합산 용량 제한, 압축 파일들이 용량 합이 제한 용량을 초과 하는 경우, 가장 오래된 압축파일 부터 지워짐 -->
		<totalSizeCap>100MB</totalSizeCap>
	</rollingPolicy>
	<encoder>
		<charset>UTF-8</charset>
		<Pattern>%d{yy-MM-dd HH:mm:ss.SSS} ${PID} [%-23t] %6p %-40.40logger{39}:%-5L - %msg%n</Pattern>
	</encoder>
	<filter class="ch.qos.logback.classic.filter.LevelFilter">
		<level>DEBUG</level>
		<onMatch>ACCEPT</onMatch>
		<onMismatch>DENY</onMismatch>
	</filter>
</appender>
```

## 
