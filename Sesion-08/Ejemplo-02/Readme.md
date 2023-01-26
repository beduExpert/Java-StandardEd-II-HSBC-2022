## Ejemplo 2: Ejecutando la aplicación en Open Liberty

### Objetivo
1. Instalar el servidor Open Liberty para poder desplegar aplicaciones web

### Requisitos previos
- Java SE

### Desarrollo 
1. A continuación, ejecutará la aplicación Spring Boot localmente en Open Liberty actualizando el `pom.xml`.

2. Agregue el `liberty-maven-pluginal` **pom.xml**

```xml 
	<plugin>
        <groupId>io.openliberty.tools</groupId>
        <artifactId>liberty-maven-plugin</artifactId>
        <version>3.5.1</version>
        <configuration>
          <appsDirectory>apps</appsDirectory>
          <installAppPackages>spring-boot-project</installAppPackages>
        </configuration>
    </plugin>
```

`liberty-maven-plugin` Descarga e instala Open Liberty en el directorio **target/liberty**. El elemento `installAppPackages` de configuración en el **pom.xml**  generalmente toma los siguientes parámetros: dependencies, projecto all. El valor predeterminado es dependencies, pero para instalar la aplicación Spring Boot en Open Liberty, el valor debe ser `spring-boot-project`. Este valor permite que Maven empaquete, reduzca y copie la aplicación `guide-spring-boot-0.1.0.jar`  en el directorio de tiempo de ejecución de Open Liberty y en el applicationsdirectorio de la biblioteca compartida.

3. Para ejecutar la aplicación Spring Boot, el servidor Open Liberty debe estar configurado correctamente. De forma predeterminada,` liberty-maven-pluginrecoge` el archivo de configuración del servidor del directorio **src/main/liberty/config**.

Creamos el archivo: **src/main/liberty/config/server.xml**.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<server description="new server">

    <featureManager>
        <feature>servlet-4.0</feature>
        <feature>springBoot-2.0</feature>
    </featureManager>

    <httpEndpoint id="defaultHttpEndpoint"
                  host="*"
                  httpPort="9080"
                  httpsPort="9443" />

    <springBootApplication id="guide-spring-boot" 
                           location="thin-guide-spring-boot-0.1.0.jar"
                           name="guide-spring-boot" />

</server>
``` 

Las funciones `servlety` `springBoot` son necesarias para que el servidor Liberty ejecute la aplicación Spring Boot. El puerto de la aplicación se especifica como `9080` y la aplicación se configura como un elemento `springBootApplication`.

4. ejecute el `package`

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

5. A continuación, ejecute la meta `liberty:run`. Este objetivo crea el servidor Open Liberty, instala las funciones necesarias, implementa la aplicación Spring Boot en el servidor Open Liberty e inicia la aplicación.

- **Mac**

```bash
./mvnw liberty:run
```

- **Windows**

```bash
mvnw.cmd liberty:run
```

- **Linux**

```bash
./mvnw liberty:run
```

6. Ve a la URL http://localhost:9080/ para acceder a la aplicación.

7. Después de terminar de explorar la aplicación, presione CTRL+C para detener el servidor Open Liberty. Alternativamente, puede ejecutar el objetivo `liberty:stop` desde el directorio en una sesión de línea de comandos separada:

- **Mac**

```bash
./mvnw liberty:stop
```

- **Windows**

```bash
mvnw.cmd liberty:stop
```

- **Linux**

```bash
./mvnw liberty:stop
```

