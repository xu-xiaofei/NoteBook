
1. 引入plugin
'''java

			<plugin>
				<groupId>org.mybatis.generator</groupId>
				<artifactId>mybatis-generator-maven-plugin</artifactId>
				<version>1.3.1</version>
				<configuration>
					<configurationFile>src/main/resources/generatorConfig.xml</configurationFile>
					<verbose>true</verbose>
					<overwrite>true</overwrite>
				</configuration>
				<executions>
					<execution>
						<id>Generate MyBatis Artifacts</id>
						<goals>
							<goal>generate</goal>
						</goals>
						<phase>none</phase>
					</execution>
				</executions>
				<dependencies>
					<dependency>
						<groupId>org.mybatis.generator</groupId>
						<artifactId>mybatis-generator-core</artifactId>
						<version>1.3.1</version>
					</dependency>
					<dependency>
						<groupId>mysql</groupId>
						<artifactId>mysql-connector-java</artifactId>
						<version>5.1.41</version>
					</dependency>
				</dependencies>
			</plugin>
'''

2. 配置generatorConbfig.xml 文件
  文件位于resources 下面