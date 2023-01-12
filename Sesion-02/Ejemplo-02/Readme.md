## Ejemplo 2: Anidamiento de datos

### Objetivos
* Definir servicios que regresen datos anidados como respuesta.

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
- Lombok

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
4. Agregamos lo siguiente al archivo `application.properties`

```
#H2
spring.h2.console.enabled=true
spring.h2.console.path=/h2
#JPA
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=create-drop
# Datasource
spring.datasource.url=jdbc:h2:file:~/test
spring.datasource.username=sa
spring.datasource.password=
spring.datasource.driver-class-name=org.h2.Driver
# Data Rest
spring.data.rest.base-path: /api
```

De este fichero la única entrada relativa a la librería Data Rest es la última. En ella especificamos la dirección donde se deben implementar las llamadas REST para acceder a las diferentes tablas que implemente nuestra aplicación. Es decir, en nuestro caso, se accederá a través de la URL: http://localhost:8080/api

Las demás líneas configuran la base de datos H2 que usaremos, así como ciertas propiedades de JPA. Por supuesto dejaremos a JPA que cree la estructura de la base de datos a través de las entidades definidas

5. Creamos un archivo `CustomerEntity`

```java
@Entity
@Data
@RestResource(rel="customers", path="customer")
public class CustomerEntity {	
	@Id
	long id;	
	@Column
	String name;	
	@Column
	String address;	
	@Column
	String telephone;	
        @Column @JsonIgnore
	String secret;
	@OneToOne
	CityEntity city;    
}
```

En donde se tiene una referencia a `CityEntity`.

6. Ahora definimos `CityEntity` como sigue:

```java
@Entity
@Data
@RestResource(exported=false)
public class CityEntity {		
	@Id 
	int id;
	@Column
	String name;
	@Column
	String province;
}
```

el parámetro `exported = false` sirve para que esta entidad no sea tratada por  Data Rest y no sera accesible a traves de ninguna API.

7. Creamos el repositorio correspondiente para `CustomerEntity`

```java
public interface CustomerRepository  extends CrudRepository<CustomerEntity, Long>;  {	
	public List<CustomerEntity>; findByNameIgnoreCaseContaining(@Param("name") String name);
}
```

8. Agregamos el siguiente cliente desde postman:

```json
{
	"id": 1, 
	"name":"nombre cliente 1",
	"address":"direccion cliente 1",
	"telephone":"telefono cliente 1", 
	"secret": "no guardar"
}
```

9. Podemos comprobar que ha realizado la inserción gracias a la consola de H2. Para ello iremos a la URL http://localhost:8080/h2/ y pulsaremos el botón Connect. Una vez conectados, si realizamos una query sobre la tabla CUSTOMER_ENTITY

Observar que aunque hemos añadido el valor para el campo “secret” en la anterior petición POST, este no se ha guardado en la base de datos.

10. Como no podemos acceder a la tabla city_entity a través de nuestra API pues así lo hemos especificado, Vamos a aprovechar que estamos en la consola y añadir un registro en la tabla, el cual asignaremos al cliente insertado.

```sql
insert into city_entity values(1,'Logroño','La Rioja');
update customer_entity set city_id=1;

```

11. Ahora realizamos una petición GET desde postman

12. Podemos consultar clientes también por su id 

```
http://localhost:8080/api/customer/1
```

13. O también probar el servicio de obtención de cliente por nombre ignorando mayúsculas y minúsculas

```
http://localhost:8080/api/customer/search/findByNameIgnoreCaseContaining{?name}
```

Por ejemplo 

```
http://localhost:8080/api/customer/search/findByNameIgnoreCaseContaining?name=Clien
```
