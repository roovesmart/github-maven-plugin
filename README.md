# github-maven-plugin [![Build Status](https://travis-ci.org/roovesmart/github-maven-plugin.svg?branch=master)](https://travis-ci.org/roovesmart/github-maven-plugin)

This Mojo creates a Maven repository on GitHub.

Usually creates a Maven repository on GitHub using com.github.maven.plugins.site.SiteMojo, 
but you can easily set the pom By using this Mojo.
Additional features can git push any file.

This was created to customize the com.github.maven.plugins.site.SiteMojo.

This is currently in development (2016.10.17) .  
So there is a possibility to change the Method name and Paramerter .

## Usage samplie

Run the following maven command

```
mvn deploy
```

## Setup

Set the pom as follows

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>mvntest</groupId>
	<artifactId>mvntest</artifactId>
	<version>1.0-SNAPSHOT</version>
	<packaging>jar</packaging>

	<name>mvntest</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<!-- [xx] Required settings -->
		<github.global.server>github</github.global.server>
	</properties>

	<!--
	 | [xx] Required settings
	 | It is set to local settings.xml
	 | <server>
	 | 	<id>github</id>
	 |	<username>github-username</username>
	 |	<password>github-password or github-token</password>
	 | </server>
	 | -->

	<!--
	 | [xx] Required settings
	 | Create a artifacts.
	 | -->
	<distributionManagement>
		<repository>
			<!-- any value -->
			<id>internal.repos</id>
			<!-- any value -->
			<name>Temporary Staging Repository</name>
			<!-- artifacts path -->
			<url>file://${project.build.directory}/mvn-repo</url>
		</repository>
	</distributionManagement>

	<build>
		<plugins>

			<!--
			 | [xx] Any settings
			 | If you want to remove a deploy target dir.
			 | -->
			<plugin>
				<artifactId>maven-clean-plugin</artifactId>
				<executions>
					<execution>
						<goals>
							<goal>clean</goal>
						</goals>
						<phase>validate</phase>
					</execution>
				</executions>
			</plugin>

			<!--
			 | [xx] Any settings
			 | If you want to include any of the files to deploy target
			 | -->
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-antrun-plugin</artifactId>
				<version>1.5</version>
				<configuration>
					<target>
						<echo message="copy files"/>
						<property name="fileDir" value="src/main/resources"/>
						<copy file="${basedir}/FileA.txt" tofile="${project.build.directory}/mvn-repo/FileA.txt"/>
					</target>
				</configuration>
				<executions>
					<execution>
						<goals>
							<goal>run</goal>
						</goals>
						<phase>deploy</phase>
					</execution>
				</executions>
			</plugin>

			<!--
			 | [xx] Required settings
			 | Push the artifacts to Github.
			 | -->
			<plugin>

				<!--
				 | [xx] Required settings
				 | Set as follows
				 | -->
				<groupId>com.appspot.roovemore.plugins</groupId>
				<!--
				 | [xx] Required settings
				 | Set as follows
				 | -->
				<artifactId>github-maven-plugin</artifactId>
				<!--
				 | [xx] Required settings
				 | -->
				<version>1.0-SNAPSHOT</version>

				<configuration>
					<!--
					 |  [xx] Any setting.
					 | The commit message used when committing the Deployed artifacts.
					 | <message>It was auto push by com.appspot.roovemore.plugins#github-maven-plugin</message>
					 | -->

					<!--
					 |  [xx] Any setting.
					 | Set it to true to always create a '.nojekyll' file at the root of the site
					 | if one doesn't already exist.
					 | <noJekyll>false</noJekyll>
					 | -->

					<!--
					 |  [xx] Any setting.
					 | Set it to true to merge with existing the existing tree that is referenced by the commit
					 | that the ref currently points to
					 | <merge>true</merge>
					 | -->

					<!--
					 |  [xx] Any setting.
					 | The base directory to commit files.
					 | Basically, you set the file path of the artifacts that have been deployed by distributionManagement
					 | <outputDirectory>${project.build.directory}/mvn-repo</outputDirectory>
					 | -->

					<!--
					 |  [xx] Any setting.
					 | Branch to update.
					 | <branch>refs/heads/mvn-repo</branch>
					 | -->

					<!--
					 |  [xx] Any setting.
					 | Paths and patterns to include
					 | <includes>
					 | 	<include>**/*</include>
					 | </includes>
					 -->

					<!--
					 |  [xx] Any setting.
					 | List for pushing any file to Github
					 | -->
					<pushOptionFiles>
						<pushOptionFile>
							<fileName>test2/hoge_fileName.txt</fileName>
							<text>hoge_value&#xA;hoge</text>
						</pushOptionFile>
						<pushOptionFile>
							<fileName>hoge_fileName2</fileName>
							<text>hoge_value2</text>
						</pushOptionFile>
					</pushOptionFiles>

					<!--
					<repositoryName>${project.artifactId}</repositoryName>
					 -->
					<!--
					 | [xx] Required settings
					 | set the github-repositoryName.
					 | -->
					<repositoryName>test1</repositoryName>

					<!--
					 | [xx] Required settings
					 | set the github-userName.
					 | -->
					<repositoryOwner>hogehoge</repositoryOwner>
				</configuration>

				<executions>
					<execution>
						<goals>
							<goal>site</goal>
						</goals>
						<phase>deploy</phase>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>
</project>
```
