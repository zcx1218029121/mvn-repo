# mvn-repo

# 添加仓库

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



# 添加 jar包

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

# 添加的jar包

### Sms

  腾讯云短信发送的封装jar包



1. 添加依赖

   ```java
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
       
       <dependency>
               <groupId>com.sqjz</groupId>
               <artifactId>Sms</artifactId>
               <version>1.0-SNAPSHOT</version>
       </dependency>
   ```

   

2. 创建配置读取类

   ```java
   @Configuration
   @ConfigurationProperties(
       prefix = "tencent.sms"
   )
   public class SmsConfigurationProperties extends SmsConfig  {
   }
   ```

   

3. 注入bean 

   比如在config中注入

   ```java
   @Configuration
   public class SmsConfiguration {
       @Resource
       private SmsConfigurationProperties smsConfigurationProperties;
   
       @Bean
       public TencentSms tencentSms() {
           return TencentSms.singleton(smsConfigurationProperties);
       }
   
   }
   ```

   

4. 配置application.yml

   ```yml
   tencent:
       sms:
           appid: 123
           appKey: "123"
           configs:
               user-auth:
                   templateId: 123
               user-tz:
                   templateId: 123
                   smsSign: "我是签名"
                   region: "86"
   
   ```

5.使用

  ```java
tencentSms.sendSingleSmsWithTemplate("user-auth","123456",new String[]{"code"});
  ```

