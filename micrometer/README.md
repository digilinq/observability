# Micrometer

Add `actuator` to your project

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

```shell
$ mvn dependency:tree -Dincludes=io.micrometer:micrometer-core
```

Add Micrometer registry dependency

```xml
<dependency>
  <groupId>io.micrometer</groupId>
  <artifactId>micrometer-registry-prometheus</artifactId>
  <scope>runtime</scope>
</dependency>
```


So we add this line to application.properties:

```yaml
management.endpoints.web.exposure.include=health,info,prometheus
```

## Creating a custom metric
```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

```java
// io.micrometer.core.aop.TimedAspect
// org.springframework.context.annotation.Bean
// io.micrometer.core.instrument.MeterRegistry

@Bean
public TimedAspect timedAspect(MeterRegistry registry) {
    return new TimedAspect(registry);
}
```

```java 
// io.micrometer.core.annotation.Timed

@Timed(value = "get.all.time", description = "Time taken to return all users")
public Greeting getGreeting() {
    return new Greeting());
}
```

http://localhost:8081/actuator/prometheus

```
# HELP get_all_time_seconds_max Time taken to return all users
# TYPE get_all_time_seconds_max gauge
get_all_time_seconds_max{class="org.digilinq.platform.users.web.resources.UsersResource",exception="none",method="findUsers",} 0.075319554
# HELP get_all_time_seconds Time taken to return all users
# TYPE get_all_time_seconds summary
get_all_time_seconds_count{class="org.digilinq.platform.users.web.resources.UsersResource",exception="none",method="findUsers",} 1.0
get_all_time_seconds_sum{class="org.digilinq.platform.users.web.resources.UsersResource",exception="none",method="findUsers",} 0.075319554
```


