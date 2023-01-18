## Ejemplo 3

### Objetivo
- Crear una clase de seguridad usando la nueva versión de Spring Security.

### Requicitos
- JDK 8+

### Desarrollo

Desde Spring Security 5.7.0-M2  el `WebSecurityConfigurerAdapter` quedo desactualizado, ya que se alienta a los usuarios a pasar a una configuración de seguridad basada en componentes. En este ejemplo veremos la forma de hacer esta configuración basada en componentes.

1. Crea un proyecto en [spring initializr](https://start.spring.io/) con las siguientes dependencias:
  - Spring Web
  - Lombok
  - Spring Security

2. Abre el proyecto en IntelliJ

3. Crearemos un paquete `model`, `controller` y `configuration` dentro del paquete principal de la aplicación.

4. Primero, definiremos nuestra clase de configuración de usuarios `UserDetailServiceConfig`:


```java
@Configuration
public class UserDetailServiceConfig {
    
}
```
5. Ahora, podemos definir un componente `UserDetailsManager` o `UserDetailsService` y de esta forma definir los usuarios de la aplicación :

```java
	@Bean
    public UserDetailsService userDetailsService (BCryptPasswordEncoder bCryptPasswordEncoder){
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(User.withUsername("javi")
                .password(bCryptPasswordEncoder.encode("1234"))
                .roles("USER")
                .build());
        manager.createUser(User.withUsername("admin")
                .password(bCryptPasswordEncoder.encode("admin"))
                .roles("USER","ADMIN")
                .build());
        return manager;
    }
```

6. Por último creamos el método que indica la forma de cifrado para las contraseñas.

```java
	@Bean
    public BCryptPasswordEncoder bCryptPasswordEncoder() {
        return new BCryptPasswordEncoder();
    }
```

7. Ahora creamos la clase de configuración de seguridad `SecurityConfig`

```java
@EnableWebSecurity
@EnableGlobalMethodSecurity(prePostEnabled = true, securedEnabled = true, jsr250Enabled = true)
public class SecurityConfig {

    // config

}
```

8. Agregamos un atributo de debugeo en falso para evitar este proceso.

```java
	@Value("${spring.security.debug:false}")
    boolean securityDebug;
```

9. Muy importante, si queremos evitar la desaprobación de la seguridad HTTP, podemos crear un bean SecurityFilterChain .

Por ejemplo, supongamos que queremos proteger los puntos finales según los roles y dejar un punto de entrada anónimo solo para iniciar sesión. También restringiremos cualquier solicitud de eliminación a un rol de administrador. Usaremos la autenticación básica:

```java 
	@Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http.csrf()
                .disable()
                .authorizeHttpRequests()
                .requestMatchers(HttpMethod.DELETE)
                .hasRole("ADMIN")
                .requestMatchers("/admin/**")
                .hasRole("ADMIN")
                .requestMatchers("/user/**")
                .hasAnyRole("USER", "ADMIN")
                .requestMatchers("/login/**")
                .anonymous()
                .requestMatchers("/all/**")
                .anonymous()
                .anyRequest()
                .authenticated()
                .and()
                .httpBasic()
                .and()
                .sessionManagement()
                .sessionCreationPolicy(SessionCreationPolicy.STATELESS);

        return http.build();
    }
```
La seguridad HTTP creará un objeto `DefaultSecurityFilterChain` para cargar filtros y comparadores de solicitudes. 

10. Agregaremos un nivel de depuración e ignoraremos algunas rutas, como imágenes o scripts:

```java
	@Bean
    public WebSecurityCustomizer webSecurityCustomizer() {
        return (web) -> web.debug(securityDebug)
                .ignoring()
                .requestMatchers("/css/**", "/js/**", "/img/**", "/lib/**", "/favicon.ico");
    }
```

11. Ahora definiremos una clase de controlador REST simple para nuestra aplicación dentro del paquete controllers.

```java 
@RestController
public class ResourceController {
    @GetMapping("/login")
    public String loginEndpoint(){
        return "Login!";
    }

    @GetMapping("/admin")
    public String adminEndpoint() {
        return "Admin!";
    }

    @GetMapping("/user")
    public String userEndpoint() {
        return "User!";
    }

    @GetMapping("/all")
    public String allRolesEndpoint() {
        return "All Roles!";
    }

    @DeleteMapping("/delete")
    public String deleteEndpoint(@RequestBody String s) {
        return "I am deleting " + s;
    }
}
```