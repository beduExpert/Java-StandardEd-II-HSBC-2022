## Ejemplo 1: TDD

<div style="text-align: justify;">

### 1. Objetivos :dart:

* Realizar pruebas de clases y métodos aislados  en específico en la JVM.

### 2. Requisitos :clipboard:

* JUnit y Thruth instalados.

### 3. Desarrollo :computer:

1. Creamos un nuevo proyecto y agregamos las dependencias de pruebas unitarias

```xml
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <version>5.8.2</version>
            <scope>test</scope>
        </dependency>
```

es muy importante notar que las bibliotecas que estamos agregando vienen de dos fuentes distintas.

2. Vamos a definir pruebas unitarias sobre una función de registro de usuarios. Para esto vamos a seguir una metodología en la que primero se definen las pruebas unitarias y posteriormente se da la especificación de la función. 

3. Creamos una nueva clase de Java con el nombre *RegistrationUtil* . Dentro de este nuevo archivo agregamos la firma de la función sobre la cual vamos a definir las pruebas unitarias.

```java
    public boolean valida (String username, String password, String confirmP) {
        return true;
    }
```

Esta función por ahora simplemente regresa True sin importar cuales sean sus argumentos.

4. Vamos a agregar una especificación informal del funcionamiento correcto de nuestra aplicación en forma de comentario. Esto no representa ningún avance en la verificación del código simplemente es una guía para nosotros, que nos ayudará a saber cuales son los aspectos a testear sobre esta función.

```Java
    /**
     * La entrada no es válida si ...
     * ... username/password es vacío
     * ...El username contiene el caracter #
     * ...Las contraseña no coinciden
     * ...La contraseña tiene una longitud menor a 8 caracteres
     */

```

5. Ahora generaremos un archivo de testing para esta clase. Para eso damos click derecho sobre el nombre del Objeto y seleccionamos la opción de Generate > Test...

Vamos a usar los siguientes valores para el archivo de testing:

- Testing Library : JUnit5
- Class name: RegistrationUtilTest

el resto de los valores los dejamos en blanco. Damos click en Ok. 

7. Definamos la prueba unitaria para el caso en el que el usuario o contraseña son vacíos. Como sigue:

```java
    @Test
    public void username1 (){
        boolean result = registro.valida("", "123","123");
        assertFalse(result);
    }
```

La anatomía de una pruieba unitaria es la siguiente:

- La anotación `@Test`.
- Se define como una función sin parámetros ni tipo de regreso.
- En el cuerpo de la función se hace la llamada a la funcionalidad que estamos testeando, con ciertos parámetros.
- Se hace una declaración 

La declaración va a depender del tipo de dato que arroje como resultado la funcionalidad testeada.

Al ejecutar esta prueba unitaria, vemos que no pasa pues estamos esperando un valor de regreso False y nuestra función siempre regresa True. 

8. Definimos ahora una función que si pase la prueba, pero sin cumplir la segunda condición.

```java
    @Test
    public void username2 (){
        boolean result = registro.valida("Jose#", "123","123");
        assertFalse(result);
    }
```

En este caso todos los campos estan presentes por lo que la prueba unitaria debería pasar.

9. Damos una prueba unitaria para el caso en el que la contraseña sea de longitud menor a 8.

```java
    @Test
    public void password1 (){
        boolean result = registro.valida("Jose", "123","123");
        assertFalse(result);
    }

```

10. Escribimos la prueba que falla cuando las contraseñas no coinciden.

```java
    @Test
    public void password2 (){
        boolean result = registro.valida("Jose","12345678","abcdefghi");
        assertFalse(result);
    }
```

11.  La siguiente prueba pasa todos los criterios.

```java
    @Test
    public void password3 (){
        boolean result = registro.valida("Jose","12345678","abcdefghi");
        assertTrue(result);
    }
```


14. Recordemos que estamos trabajando con un desarrollo a la inversa en donde a partir de las pruebas unitarias se da la implementación para la función que estamos testeando. Entonces regresamos a `RegistrationUtil` e implementamos la función.

```java
    public boolean valida (String username, String password, String confirmP) {
        if (username.isEmpty() || password.isEmpty() || confirmP.isEmpty()) return false;
        if (username.contains("#")) return false;
        if (password != confirmP) return false;
        if (password.length() < 8) return false;
        return true;
    }
```  

</div>

