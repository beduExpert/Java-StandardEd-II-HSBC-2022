## Ejemplo 1

### Objetivo
1. Construir una aplicación con Spring Boot para publicarla.
2. Utilizar spring-boot-maven-plugin para la creación de un jar ejecutable que contenga una aplicación web.

### Requisitos previos
- JDK
- Apache Maven

### Desarrollo
1. Apoyándonos de Spring Initializr crearemos una aplicación web sencilla que nos retorne un Hello World! al acceder a ella, para ello entraremos al sitio de [Spring Initializr](https://start.spring.io/)
2. Llenaremos los datos necesarios para crear nuestra aplicación, ingresando el grupo (org.bedu) y nombre de nuestro artefacto (ejemplo-jar).
3. Dentro del apartado de opciones revisaremos que el empaquetado esté especificado en jar y dentro de las dependencias agregaremos _Spring Web_ para generar una aplicación Web que incluya un servidor embebido.
4. Crearemos el proyecto mediante el botón _Generate_ o presionando Ctrl + Enter, lo que nos dará un archivo zip para descargar.
5. Agregaremos un controlador sencillo que nos salude al ingresar a nuestra aplicación para verificar su funcionamiento, para ello crearemos una nueva clase llamada SaludoController, cuyo código será el siguiente:
```java
@RestController
public class SaludoController {
    @RequestMapping("")
    public String hola(){
        return "Hola mundo desde un JAR";
    }
}
```
6. En terminal ejecutaremos alguno de los siguientes comandos según sea el caso:

- **Mac**

```bash
./mvnw package
```
- **Windows**

```bash
mvnw.cmd package
```
- **Linux**

```bash
./mvnw package
```

7. Ahora puede ejecutar la aplicación en el contenedor web integrado de Tomcat ejecutando el archivo JAR que creó:

```bash
java -jar target/guide-spring-boot-0.1.0.jar
```

Ve a la http://localhost:8080/ para acceder a la aplicación.

8. Cuando necesite detener la aplicación, presione CTRL+Cen la sesión de línea de comandos donde ejecutó la aplicación.
