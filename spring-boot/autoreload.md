# Autoreload - Spring Boot

1. Agregar la siguiente dependencia en <b>pom.xml</b>.

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-devtools</artifactId>
        <optional>true</optional>
    </dependency>
</dependencies>
```

2. Modificar el archivo <b>application.properties</b> que se encuentra en el paquete <b>src.main.resources</b> con el siguiente código.

```properties
spring.devtools.livereload.enabled=true
spring.devtools.restart.exclude=static/**,public/**
spring.devtools.restart.enabled=true
```

3. Agregar el siguiente código en el archivo <b>.vscode/task.json</b>.

```json

{
    "label": "autoreload",
    "type": "shell",
    "command": ".\mvnw spring-boot:run",
    "group": "build",
    "problemMatcher": []
},

```
En caso que Maven esté agregado en el PATH del sistema, se podrá ejecutar con el siguiente código:

    $ mvn spring-boot-run

---

Fuentes:

+ https://www.codejava.net/frameworks/spring-boot/spring-boot-auto-reload-changes-using-livereload-and-devtools <br>
+ https://docs.spring.io/spring-boot/docs/1.5.16.RELEASE/reference/html/using-boot-devtools.html <br>
+ https://github.com/pepeul1191/spring-boot
