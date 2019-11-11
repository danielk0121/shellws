### 프로젝트 폴더 구조
- src/main/groovy 폴더를 생성해 준다
```
├─src
│  ├─main
│  │  ├─groovy         <---- *.groovy 파일 위치
│  │  │  └─com
│  │  │      └─enliple
│  │  │          └─recommend
│  │  │              └─study
│  │  │                  └─domain
│  │  ├─java           <---- *.java 파일 
│  │  │  └─com
│  │  │      └─enliple
│  │  │          └─recommend
│  │  │              └─study
│  │  └─resources
│  │      ├─static
│  │      └─templates
│  └─test
│      ├─groovy
│      └─java
└─target
    ├─classes
    │  └─com
    │      └─enliple
    │          └─recommend
    │              └─study
    │                  └─domain
    ├─maven-archiver
    └─maven-status
        └─maven-compiler-plugin
            └─compile
                └─default-compile
```

### pom.xml
- maven-compiler-plugin 과 groovy-eclipse 플러그인 버전이 맞지 않으면 compile phase 에서 애러 발생함 !
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">

	<modelVersion>4.0.0</modelVersion>

	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.0.4.RELEASE</version>
		<relativePath /> <!-- lookup parent from repository -->
	</parent>

	<groupId>com.enliple.recommend.study</groupId>
	<artifactId>spring_groovy_study</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>spring_groovy_study</name>
	<description>spring_groovy_study</description>
	<packaging>jar</packaging>

	<properties>
		<java.version>1.8</java.version>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
		<version.groovy-eclipse-compiler>2.9.1-01</version.groovy-eclipse-compiler>
		<version.groovy-eclipse-batch>2.3.7-01</version.groovy-eclipse-batch>
	</properties>

	<dependencies>
		<!-- spring-boot-configuration -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-configuration-processor</artifactId>
			<optional>true</optional>
		</dependency>
		
		<!-- lombok -->
		<dependency>
			<groupId>org.projectlombok</groupId>
			<artifactId>lombok</artifactId>
			<optional>true</optional>
		</dependency>
		
		<!-- spring-boot-starter -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>

		<!-- Embedded tomcat -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-tomcat</artifactId>
		</dependency>

		<!-- spring mvc -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<!-- for groovy -->
		<dependency>
			<groupId>org.codehaus.groovy</groupId>
			<artifactId>groovy</artifactId>
		</dependency>

	</dependencies>

	<pluginRepositories>
		<pluginRepository>
			<id>bintray</id>
			<name>Groovy Bintray</name>
			<url>https://dl.bintray.com/groovy/maven</url>
			<releases>
				<!-- avoid automatic updates -->
				<updatePolicy>never</updatePolicy>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>

			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<!-- <version>3.7.0</version> -->
				<configuration>
					<encoding>${project.build.sourceEncoding}</encoding>
					<source>${maven.compiler.source}</source>
					<target>${maven.compiler.target}</target>
					<compilerId>groovy-eclipse-compiler</compilerId>
					<verbose>true</verbose>
					<fork>true</fork>
					<compilerArguments>
						<javaAgentClass>lombok.launch.Agent</javaAgentClass>
					</compilerArguments>
				</configuration>
				<dependencies>
					<dependency>
						<groupId>org.codehaus.groovy</groupId>
						<artifactId>groovy-eclipse-compiler</artifactId>
						<version>${version.groovy-eclipse-compiler}</version>
					</dependency>
					<dependency>
						<groupId>org.codehaus.groovy</groupId>
						<artifactId>groovy-eclipse-batch</artifactId>
						<version>${version.groovy-eclipse-batch}</version>
					</dependency>
					<dependency>
						<groupId>org.projectlombok</groupId>
						<artifactId>lombok</artifactId>
						<version>${lombok.version}</version>
					</dependency>
				</dependencies>
			</plugin>
			
			<plugin>
				<groupId>org.codehaus.groovy</groupId>
				<artifactId>groovy-eclipse-compiler</artifactId>
				<version>${version.groovy-eclipse-compiler}</version>
				<extensions>true</extensions>
			</plugin>

			<plugin>
				<groupId>org.codehaus.mojo</groupId>
				<artifactId>build-helper-maven-plugin</artifactId>
				<!-- <version>1.12</version> -->
				<executions>
					<execution>
						<id>add-source</id>
						<phase>generate-sources</phase>
						<goals>
							<goal>add-source</goal>
						</goals>
						<configuration>
							<sources>
								<source>src/main/groovy</source>
							</sources>
						</configuration>
					</execution>
					<execution>
						<id>add-test-source</id>
						<phase>generate-test-sources</phase>
						<goals>
							<goal>add-test-source</goal>
						</goals>
						<configuration>
							<sources>
								<source>src/test/groovy</source>
							</sources>
						</configuration>
					</execution>
				</executions>
			</plugin>

		</plugins>
	</build>

</project>
```
