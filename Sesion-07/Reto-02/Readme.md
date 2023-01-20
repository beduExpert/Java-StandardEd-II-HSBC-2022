# Reto 2 - Pruebas unitarias con JUnit

## :dart: Objetivo

- Utilizar JUnit
- Desarrollar pruebas utilizando distintas características de JUnit

## ⚙ Requisitos

- IntelliJ IDEA
- Java
- Gradle
- JUnit
- JUnit 5
- Code with me
- Una cuenta de GitHub

## Desarrollo

Utilizando como base el siguiente código de la calculadora realiza las siguientes pruebas

- Una prueba que solo se ejecute cunado nos encontramos en nuestro sistema operativo actual
- Una prueba que solo se ejecute en algún sistema operativo diferente a nuestro sistema operativo actual
- Una prueba que solo se ejecute en el ambiente de desarrollo y configurar nuestras variables de entorno para indicar
  que estamos en el ambiente de desarrollo
- Una prueba parametrizada para la suma
- Una prueba parametrizada para la multiplicación
- Una prueba con un display name distinto al nombre de la función

`Calculator.java`

```java
public class Calculator {

    public int add(int a, int b) {
        return a + b;
    }

    public int multiply(int a, int b) {
        int result = 0;
        for (int i = 0; i < b; i++) {
            result += add(result, a);
        }
        return result;
    }
}
```

