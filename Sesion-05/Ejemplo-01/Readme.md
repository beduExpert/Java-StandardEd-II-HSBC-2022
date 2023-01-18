## Ejemplo 1: 

### Objetivo
- Agregar Seguridad a la aplicación usando Spring Security
- Agregar un manejo de usuarios dentro de la aplicación.
---

### Requisitos
- JDK 8 (o superior)
---

### Desarrollo

1. Crea un proyecto en [spring initializr](https://start.spring.io/) con las siguientes dependencias:
  - Spring Web
  - Lombok
  - Spring Security

2. Abre el proyecto en IntelliJ

3. Con tan solo añadir la dependencia de Spring Boot Starter Security habilita la autenticación básica. Si creamos un endpoint cualquiera e intentamos consumirlo, se mostrará un formulario de inicio de sesión proporcionado por Spring Security.

4. Crearemos un paquete de `model` y uno `controller` dentro del paquete principal de la aplicación.

5. Creamos una clase `Producto` dentro del paquete `model`

```java
@Data
public class Producto {
    private long id;
    private String name;
    private double price;
}
```

La anotamos con `@Data` para no tener que agregar manualmente los getters y setters

6. Ahora agregamos un controlador `ProductoController`

```java
@RestController
@RequestMapping("/productos")
public class ProductoController {
    
}
```

7. Agregamos un servicio de consulta de productos

```java
    @GetMapping
    public ResponseEntity<Producto> getProducto (){
        Producto p = new Producto();
        p.setId(0);
        p.setName("planta");
        p.setPrice(235.5);

        return ResponseEntity.ok(p);
    }
```

8. Ejecutamos la aplicación y notemos como en la consola de IntelliJ nos genera una contraseña, el usuario por defecto es `user`

9. Al intentar acceder al servicio de consulta de productos nos aparece como de acceso restringido por lo que nos redirige al login. Iniciamos sesión con las credenciales adecuadas. 