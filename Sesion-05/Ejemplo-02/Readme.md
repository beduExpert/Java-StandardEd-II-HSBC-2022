## Ejemplo 2.

### Objetivo
- Crear una clase de seguridad personalizada.
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

3. En este ejemplo usaremos una versión en especifico de spring security por lo que es necesario indicarla en la sección de `properties` del archivo `pom.xml`.

```xml
<spring-security.version>5.6.0</spring-security.version>
```

4. Crearemos un paquete `model`, `controller` y `configuration` dentro del paquete principal de la aplicación.

5. Para crear una clase de seguridad personalizada, necesitamos usar `@EnableWebSecurity` y extender la clase con `WebSecurityConfigurerAdapter` para que podamos redefinir algunos de los métodos proporcionados. Spring Security te fuerza a hashear las contraseñas para que no se guarden en texto plano. 

> Para los siguientes ejemplos, vamos a usar PasswordEncoder, aunque no debe ser una opción para proyectos reales, pero para este ejercicio es mas que suficiente. Una alternativa podría ser BCryptPasswordEncoder.

6. Dentro de `configuration` agregamos una nueva clase llamada `WebSecurity` e indicamos que dicha clase extiende a `WebSecurityConfigurerAdapter`.

```java
@EnableWebSecurity
public class WebSecurity extends WebSecurityConfigurerAdapter {

}
```

7. En intelliJ, podemos hacer cmd + N y comprobar los métodos que podemos sobrescribir(override methods). Vamos a utilizar el método configure que recibe como parámetro `AuthenticationManagerBuilder.auth` tiene diferentes métodos como `jdbcAuthentication`, `ldapAuthentication`, `userDetailsService` pero usaremos `inMemoryAuthentication` para este ejemplo. Como su nombre lo indica, las credenciales del usuario se almacenan en memoria.

```java
				@Override
        protected void configure(AuthenticationManagerBuilder auth) throws Exception {
            auth.inMemoryAuthentication()
                    .withUser("user1").password("123").roles("APPRENTICE")
                    .and()
                    .withUser("user2").password("123").roles("SENSEI");
        }
 
        @Bean
        public PasswordEncoder getPasswordEncoder() {
            return NoOpPasswordEncoder.getInstance();
        }
```

8. Definimos qué recursos deben estar securizados y cuales no. Ahora hacemos uso del método configure que `recibeHttpSecurity` como parámetro.

```java
		@Override
    public void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .antMatchers("/**").hasRole("SENSEI")
            .and().formLogin();
    }
```

Si intentamos iniciar sesión con user2, accederemos correctamente ya que que  hemos otorgado permisos a cualquier usuario que tenga el rol ‘SENSEI’. Incluso podemos crear varios endpoints y otorgar distintas restricciones como vemos en el siguiente ejemplo. Debemos tener en cuenta que las reglas más restrictivas deben estar en la parte superior.

```java
		@Override
    public void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
                .antMatchers("/sensei").hasRole("SENSEI")
                .antMatchers("/apprentice").hasRole("APPRENTICE")
                .antMatchers("/").permitAll()
                .and().formLogin();
    }
```

Si quisiéramos añadir diferentes roles para un mismo recurso podríamos usar hasAnyRole(). También podemos añadir filtros, cuyo objetivo es interceptar las peticiones y cada uno tiene su propia responsabilidad (añadiremos uno mas adelante)

```java
.hasAnyRole("ROLE1", "ROLE2", "ROLE3")
.addFilterBefore()
.addFilterAfter()
```
