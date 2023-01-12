## Ejemplo 01: Fundamentos de Spring Data Rest

### Objetivos
* Entender el funcionamiento básico de Spring Data Rest

### Prerequisitos
* Maven
* JDK 11

### Procedimiento

1. Abrir [spring initializr](https://start.spring.io/) y crear un nuevo proyecto con las siguientes características: 
- Maven
- Java
- 3.0.1
- Jar
- 11 

y agregar las dependencias:

- Rest Repositories
- H2
- JPA

2. Abre el proyecto en IntelliJIDEA.
3. Verifica que en el archivo `pom.xml` estén declaradas las siguientes dependencias.
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId
    <artifactId>spring-boot-starter-data-rest</artifactId></dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
</dependency>
```

4. Comenzaremos escribiendo un objeto de dominio para representar a un usuario de nuestro sitio web:

```java
@Entity
public class WebsiteUser {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private long id;

    private String name;
    private String email;

    // standard getters and setters
}
```

5. Ahora podemos definir un repositorio simple para esta colección:
```java
@RepositoryRestResource(collectionResourceRel = "users", path = "users")
public interface UserRepository extends CrudRepository<WebsiteUser, Long> {
    List<WebsiteUser> findByName(@Param("name") String name);
}
```

Esta es una interfaz que le permite realizar varias operaciones con objetos WebsiteUser . También definimos una consulta personalizada que proporcionará una lista de usuarios basada en un nombre dado.

La anotación @RepositoryRestResource es opcional y se usa para personalizar el punto final REST. Si decidiéramos omitirlo, Spring crearía automáticamente un punto final en `/websiteUsers` en lugar de `/users`.

¡Eso es todo! Ahora tenemos una API REST totalmente funcional.

6. Probemos como funciona la API, entramos a [](http://localhost:8080/), el resultado debe ser el siguiente JSON.
```json
{
  "_links" : {
    "users" : {
      "href" : "http://localhost:8080/users{?page,size,sort}",
      "templated" : true
    },
    "profile" : {
      "href" : "http://localhost:8080/profile"
    }
  }
}
```



