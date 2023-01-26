## Ejemplo 3: Despliegue 

### Objetivo
1. Desplegar una aplicación Spring Boot 

### Requisitos previos
- El JAR generado durante el Ejemplo 2

### Desarrollo 

Hay muchas opciones para implementar aplicaciones Spring Boot. En este articulo. Cubriremos tres de ellos:

- Azur
- Servicios web de Amazon
- Autohospedado

### AZUR

Microsoft ha invertido en Java últimamente; una asociación con Azul y la adquisición de jClarity , naturalmente, la implementación en Azure no es difícil.

Asegúrese de tener una cuenta activa registrándose en el sitio web de Azure . El primer paso es instalar la CLI de Azure en su computadora. Lo usaremos para iniciar sesión en Azure desde una terminal de línea de comandos.

En el proyecto, agregue el complemento de Azure como parte de la compilación en el archivo **pom.xml**. Pruebe la configuración de ejemplo a continuación y asegúrese de reemplazar appNamecon un nombre único para su aplicación. Configure también el region, para definir dónde se implementará geográficamente su aplicación. La lista de regiones disponibles está disponible en Azure Region Availability . Finalmente, pricingTierdetermina qué tipo de instancias usará y cuánto costará. La lista de niveles de precios disponibles está disponible en Precios de Azure App Service . El nivel de precios configurado en el siguiente ejemplo es "F1", que representa su nivel gratuito , así que no se preocupe si solo quiere probar Azure; no se le facturará si realiza la implementación en el nivel F1.

```xml
<plugin>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-webapp-maven-plugin</artifactId>
    <version>1.7.0</version>
    <configuration>
     <schemaVersion>v2</schemaVersion>
     <resourceGroup>maven-plugin</resourceGroup>
     <appName>Spring-App</appName>
     <region>East US</region>
     <runtime>
         <os>linux</os>
         <javaVersion>jre8</javaVersion>
         <webContainer></webContainer>
     </runtime>
     <deployment>
         <resources>
             <resource>
                 <directory>${project.basedir}/target</directory>
                 <includes>
                     <include>*.jar</include>
                 </includes>
             </resource>
         </resources>
     </deployment>
     <pricingTier>F1</pricingTier>
    </configuration>
</plugin>
```

Dado que Azure maneja HTTPS por nosotros y nos ejecutaremos detrás de su proxy, también debemos actualizar nuestra configuración de Spring para recoger los encabezados HTTP estándar y crear correctamente las URL de redireccionamiento. Simplemente agregue esta línea al archivo `application.properties`

```
server.forward-headers-strategy=FRAMEWORK
```

Una vez que haya configurado el `pom.xml` y el `application.properties`, la implementación consiste en abrir un terminal de línea de comandos, iniciar sesión en Azure y ejecutar el objetivo de Maven para realizar la implementación. Para iniciar sesión en Azure, use el comando az login. Esto abrirá su navegador web para el proceso de inicio de sesión y luego imprimirá la información de su cuenta en la consola. Luego, ejecute mvn packagepara asegurarse de que su aplicación esté construida. Después de iniciar sesión, use el comando `mvn azure-webapp:deploy` para implementar su aplicación en la nube de Azure. Entonces, todo el proceso de implementación se ve así:

```bash
$ az login
$ mvn package
$ mvn azure-webapp:deploy
```

Después de eso, su aplicación se implementará en Azure.

[https://<your_app_name>.azurewebsites.net/](https://<your_app_name>.azurewebsites.net/)

### AWS

Amazon AWS sigue siendo la opción por defecto para las nubes públicas con aproximadamente la mitad de la cuota de mercado . Las aplicaciones Java se pueden implementar en AWS Elastic Beanstalk con unos pocos clics de un botón.

Dado que AWS manejará el tráfico de la aplicación por nosotros y ejecutaremos detrás de su proxy, debemos actualizar Spring para aceptar los encabezados HTTP estándar y crear correctamente las URL de redireccionamiento. Simplemente agregue esta línea al archivo `application.properties`

```
server.forward-headers-strategy=FRAMEWORK
```

Después de configurar el `application.properties`, asegúrese de que su aplicación esté compilada haciendo un archivo `mvn package`. También necesitaremos el archivo JAR generado. Para implementar en Amazon Web Services, iremos a la consola de administración de AWS y seguiremos la guía de implementación paso a paso:

1. Busque y haga clic en Elastic Beanstalk .
2. Esto lo llevará a un asistente paso a paso para implementar la aplicación.
3. Haga clic en Comenzar .
4. Ingrese el nombre de su aplicación.
5. Seleccione Java como plataforma.
6. Seleccione Cargar su código .
7. Haga clic en Cargar .
8. Seleccione Archivo local .
9. Busque y seleccione el archivo JAR de su aplicación, ubicado en<your_app_path>/target/demo-0.0.1-SNAPSHOT.jar
10. Puede dejar la etiqueta Versión como está, pero deberá usar etiquetas únicas cada vez que implemente una versión más nueva de su aplicación.
11. Haga clic en Cargar .
12. Haz clic en Configurar más opciones
13. En Software, haga clic en Modificar
14. En Propiedades del entorno , cree una nueva propiedad con Nombre SERVER_PORT y Valor 5000 .
15. Haga clic en Guardar .
16. Haz clic en Crear aplicación .

Llegará a una página que muestra el progreso de la creación de la aplicación. Espera hasta que termine. Cuando esté completo, la URL de su aplicación debería mostrarse en la barra superior de la pantalla.

[http://<your_app_name>-env.<app_id>.<region>.elasticbeanstalk.com/](http://<your_app_name>-env.<app_id>.<region>.elasticbeanstalk.com/)

### AutoHost

Si decide alojar la aplicación usted mismo, primero debe decidir si desea ejecutar su aplicación por sí misma como un JAR o implementarla en un servidor de aplicaciones como Tomcat como WAR. Recomendaría un JAR, de modo que las versiones de su aplicación y el servidor de aplicaciones se administren juntas, lo que simplifica su implementación.

Para ejecutar como un JAR independiente, simplemente cree su aplicación mvn packagepara generar un archivo JAR en la targetcarpeta. Luego, puedes ejecutarlo con `java -jar <your_app_name>-<your_app_version>.jar`.

Si desea usar una instalación existente de Tomcat o prefiere usar un servidor de aplicaciones, su proceso de compilación debe generar un WAR en lugar de un JAR y también conectarse con el servidor de aplicaciones. Para eso, debe eliminar el `spring-boot-maven-plugin` de su **pom.xml**. Luego añádele la siguiente línea:

```xml 
<packaging>war</packaging>
```

Asegúrese de que Tomcat no esté incrustado en su aplicación, ya que lo ejecutará dentro de un Tomcat externo. Agregue estas líneas a la sección de dependencias de su **pom.xml**:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```

Ahora, cambie la clase principal para que registre la aplicación en el servidor, haga que se extienda `SpringBootServletInitializery` anule el método `configure`. Por ejemplo:

```java
package com.oktadeveloper.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class DemoApplication extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(DemoApplication.class);
    }
}
```

Ahora, si ejecuta `mvn clean package`, debería obtener un archivo WAR en la targetcarpeta. Copie ese archivo WAR en su instalación de Tomcat **webapps/ROOT.wary** elimine el directorio **webapps/ROOT**  existente. Tomcat lo recogerá automáticamente y lo implementará. Una vez completada la implementación, su aplicación debería estar disponible en una dirección como http://localhost:8080/.

Hay algunos desafíos nuevos cuando se aloja uno mismo. Por ejemplo, si desea ejecutar su aplicación en varios servidores, debe asegurarse de que cada servidor ejecute la misma versión de la aplicación y querrá que otro servidor maneje el equilibrio de carga. Puede usar **NGINX** como balanceador de carga, solo asegúrese de actualizar siempre su configuración a medida que escala sus servidores. Una forma de asegurarse de que todos sus servidores estén configurados de la misma manera es usar algunas soluciones de automatización como Chef . El alojamiento propio significa que tiene que hacer todo usted mismo, como la administración de certificados TLS para asegurarse de que sus servidores escuchen en HTTPS. Azure y Amazon tienen ofertas específicas para administrar el certificado por usted.

