# AppMap Maven plugin

- [AppMap Maven plugin](#appmap-maven-plugin)
  - [Quickstart](#quickstart)
  - [About](#about)
    - [Typical entry in pom.xml](#typical-entry-in-pomxml)
  - [Plugin goals](#plugin-goals)
  - [Plugin configuration](#plugin-configuration)
    - [Configuration example](#configuration-example)
  - [Running](#running)

## Quickstart

First, ensure you have a
[properly configured `appmap.yml`](https://github.com/applandinc/appmap-java#configuration)
in your root project directory. A basic configuration may look like:

```yml
# appmap.yml
name: my_organization/my_application

packages:
  # List the packages you'd like to record here.
  - path: com.myorganization.myapplication

  # Individual classes or methods can be chosen as well.
  #
  # - path: com.myorganization.myapplication.MyClass
  # - path: com.myorganization.myapplication.MyClass#myInstanceMethod
  # - path: com.myorganization.myapplication.MyClass.myStaticMethod
  #
  # Optionally, include paths of packages, classes
  # or methods that you'd like to exclude from recording
  #
  # - exclude: com.myorganization.myapplication.util
  # - exclude: com.myorganization.myapplication.MyClass
  # - exclude: com.myorganization.myapplication.MyClass#myInstanceMethod
  # - exclude: com.myorganization.myapplication.MyClass.myStaticMethod
```

Next, run your tests with the `com.appland:appmap-maven-plugin:prepare-agent`
goal to load the AppMap agent into your test execution and record tests:

```sh
mvn com.appland:appmap-maven-plugin:prepare-agent test
```

Continue on to add AppMap to your `pom.xml` as a plugin.

## About

The AppMap Maven Plugin provides simple method for recording AppMaps in running
tests in Maven projects and a seamless integration into CI/CD pipelines. The
recording agent requires `appmap.yml` configuration file, see
[appmap-java](https://github.com/applandinc/appmap-java/blob/master/README.md)
for details.

### Typical entry in pom.xml

```xml
<!-- the plugin element goes to build/plugins -->
<!-- AppMap agent, default parameters -->
<plugin>
    <groupId>com.appland</groupId>
    <artifactId>appmap-maven-plugin</artifactId>
    <executions>
        <execution>
            <phase>process-test-classes</phase>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## Plugin goals

- `prepare-agent` - adds the AppMap Java agent to the JVM

## Plugin configuration

- `configFile` Path to the `appmap.yml` config file. Default: _./appmap.yml_
- `outputDirectory` Output directory for `.appmap.json` files. Default:
  _./target/appmap_
- `skip` Agent won't record tests when set to true. Default: _false_
- `debug` Enable debug flags as a comma separated list. Accepts: `info`, `hooks`, `http`, `locals` Default: _null_ (disabled)
- `debugFile` Specify where to output debug logs. Default: _target/appmap/agent.log_
- `eventValueSize` Specifies the length of a value string before truncation
  occurs. If set to 0, truncation is disabled. Default: _1024_

### Configuration example

```xml
<!-- AppMap Java agent, all parameters -->
<plugin>
    <groupId>com.appland</groupId>
    <artifactId>appmap-maven-plugin</artifactId>
    <configuration>
        <configFile>appmap.yml</configFile>
        <outputDirectory>target/appmap</outputDirectory>
        <skip>false</skip>
        <debug></debug>
        <debugFile>target/appmap/agent.log</debugFile>
        <eventValueSize>1024</eventValueSize>
    </configuration>
    <executions>
        <execution>
            <phase>process-test-classes</phase>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

## Running

With the [above configuration](#configuration-example) added to your `pom.xml`,
AppMaps will be automatically recorded when the agent is active and tests are
run.

Alternatively, you can load the AppMap agent without modifying your `pom.xml`.
See the [Quickstart](#quickstart) section for more information.
