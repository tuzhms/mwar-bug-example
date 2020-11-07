# When it is necessary to build more than one WAR archive at the same time, it fails to set different web.xml
[![JIRA issue](https://img.shields.io/jira/issue/MWAR-440?baseUrl=https%3A%2F%2Fissues.apache.org%2Fjira)](https://issues.apache.org/jira/browse/MWAR-440)

In an assembly, I need to create 2 WAR archives with different web.xml files at a time.
For this I use the following plugin setting:
```xml
<plugin>
    <artifactId>maven-war-plugin</artifactId>
    <version>3.2.2</version>
    <configuration>
        <packagingExcludes>web?.xml</packagingExcludes>
        <skip>true</skip>
    </configuration>
    <executions>
        <execution>
            <id>1</id>
            <phase>package</phase>
            <goals>
                <goal>war</goal>
            </goals>
            <configuration>
                <skip>false</skip>
                <webXml>src/main/webapp/web1.xml</webXml>
                <warName>web1</warName>
            </configuration>
        </execution>
        <execution>
            <id>2</id>
            <phase>package</phase>
            <goals>
                <goal>war</goal>
            </goals>
            <configuration>
                <skip>false</skip>
                <webXml>src/main/webapp/web2.xml</webXml>
                <warName>web2</warName>
            </configuration>
        </execution>
    </executions>
</plugin>
```
```
Project structure:

src/main/webapp/web1.xml
src/main/webapp/web2.xml
pom.xml
```
```xml
<!-- web1.xml -->
<web-app>
    <!--    Content web1.xml-->
</web-app>
```
```xml
<!-- web2.xml -->
<web-app>
    <!--    Content web2.xml-->
</web-app>
```
But the first of the specified files gets into both archives, despite the different paths \<webXml\>.

```shell script
$ unzip -p web1.war WEB-INF/web.xml
<web-app>
    <!--    Content web1.xml-->
</web-app>

$ unzip -p web2.war WEB-INF/web.xml
<web-app>
    <!--    Content web1.xml-->
</web-app>
```
