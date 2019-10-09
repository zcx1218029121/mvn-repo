# mvn-repo
mavn 仓库
添加仓库
```
 <repositories>
        <repository>
            <id>mvn-repo</id>
            <url>https://github.com/zcx1218029121/mvn-repo/master</url>
            <snapshots>
                <enabled>true</enabled>
                <updatePolicy>always</updatePolicy>
            </snapshots>
        </repository>
    </repositories>
```




添加 jar包
```
 <plugin>
                <artifactId>maven-deploy-plugin</artifactId>
                <version>2.8.1</version>
                <configuration>
                    <altDeploymentRepository>internal.repo::default::file://${project.build.directory}/mvn-repo</altDeploymentRepository>
                </configuration>
            </plugin>
```

```
 <plugin>
                <groupId>com.github.github</groupId>
                <artifactId>site-maven-plugin</artifactId>
                <version >0.12</version>
                <configuration>
                    <message >Maven artifacts for ${project.version}</message>
                    <noJekyll>true</noJekyll>
                    <outputDirectory>${project.build.directory}/mvn-repo</outputDirectory><!--本地jar地址-->
                    <branch>refs/heads/master</branch><!--分支的名称-->
                    <merge>true</merge>
                    <includes>
                        <include>**/*</include>
                    </includes>
                    <repositoryName>mvn-repo</repositoryName><!--对应github上创建的仓库名称 name-->
                    <repositoryOwner>zcx1218029121</repositoryOwner><!--github 仓库所有者即登录用户名-->
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
```

```
<dependencies>
        <dependency>
            <groupId>com.github.qcloudsms</groupId>
            <artifactId>qcloudsms</artifactId>
            <version>1.0.6</version>
        </dependency>
    </dependencies>
```
