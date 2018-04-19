# spring-angular-demo

基于Spring Boot构建的应用程序，前端使用Angular。详细Angular查看[文档](src/main/angular/README.md)。

## 安装事项

1. 按照Angular安装说明进行安装 NodeJs、NPM。
2. 控制台输入 `npm install -g @angular/cli` 全局安装 Angular CLI 。
3. 目录切换到 `src/main` 执行命令 `ng new my-app`，my-app是自定义的名称，这里已经执行了该步骤，生成的目录是`src/main/angular`。
4. 项目目录结构

        src/main/
        --------angular
        --------java
        --------resources
        pom.xml

5. 由于springboot的工程中要加入静态html文件等需要放在resources下面的static目录下，然后直接通过localhost:8080/index.html即可访问static目录下的index.html文件。所以我们需要将angular的编译代码放在该static目录中。

        <resources>
            <resource>
                <directory>src/main/resources</directory>
            </resource>
            <resource>
                <directory>${project.basedir}/src/main/angular/dist</directory>
                <targetPath>static</targetPath>
            </resource>
        </resources>

6. 设置Maven build的时候buildAngular。

        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.6.0</version>
                <executions>
                    <execution>
                        <phase>generate-sources</phase>
                        <goals>
                            <goal>exec</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <executable>ng</executable>
                    <workingDirectory>src/main/angular</workingDirectory>
                    <arguments>
                        <argument>build</argument>
                    </arguments>
                </configuration>
            </plugin>
        </plugins>

    然后执行`mvn clean package`后，在target/classes目录下的就会看到static目录以及angular/dist目录中的所有文件。最终生成的jar包中也会包含这些内容。
    `mvn clean spring-boot:run`即可启动该项目，并且会加载angular的编译文件。
