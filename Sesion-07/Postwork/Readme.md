# Postwork

[Ver en Google docs](https://docs.google.com/document/d/1WCMWMi-sLFNS9DE5Mph2jx67Ng4pvS_Ra4cAl2EqVO8/edit?usp=sharing)


## 🎯 OBJETIVOS

- Comparar el funcionamiento de JUnit y TestNG en escenarios similares
- Analizar cómo funciona la ejecución en paralelo en JUnit 5
- Analizar cómo funciona la ejecución en paralelo en TestNG

## 🚀 DESARROLLO

Antes de comenzar con la actividad te recomendamos revisar el siguiente recurso para que comprendas que hay detrás de la ejecución de pruebas en paralelo utilizando JUnit 5:

[Ejecución Paralela](https://junit.org/junit5/docs/5.3.0-M1/user-guide/index.html#writing-tests-parallel-execution)

Continuaremos con el desarrollo del software de entrevistadores y exploraremos la paralelización de pruebas con JUnit realizando el siguiente ejercicio:

- Configura tu proyecto e instala las dependencias necesarias 

- Paralelizar la ejecución de tus pruebas

### Sigue estas instrucciones:

- En tu archivo build.grade asegurate de tener junit en su versión 5.3 o superior
- En el mismo archivo asegurate que la configuración test luzca de la siguiente forma


```
test {
    useJUnitPlatform()
    systemProperty 'junit.jupiter.execution.parallel.enabled', 'true'
}
```

-Ejecuta las pruebas
-Cambia la configuración anterior a false y observa las diferencias en los tiempos de ejecución


Para cerrar nuestro postwork, revisemos el siguiente video que realiza un benchmark de la ejecución en paralelo entre diferentes frameworks de pruebas:

[¿Cuál tecnología es más eficiente para ejecutar los tests en paralelo?](https://www.youtube.com/watch?v=omX2IQvHA1A)


## Indicaciones generales

¿Terminaste la actividad? responde las siguientes preguntas:

¿Notaste alguna diferencia en el tiempo de ejecución de las pruebas?
_________________________________________________________________________________________________________________________________________________________________________________________________________________________________

¿Qué ventajas consideras que aporta la ejecución en paralelo?
_________________________________________________________________________________________________________________________________________________________________________________________________________________________________
¿Crees que existe alguna desventaja de ejecutar nuestras pruebas en paralelo?
_________________________________________________________________________________________________________________________________________________________________________________________________________________________________


¡Mucho éxito y reta tu potencial!

## ✅ Checklist 

Asegúrate que tu postwork contenga todo lo siguiente, ya que esto se evaluará al término del módulo.

- [ ] Tu archivo de configuración build.grade cuenta la configuración  systemProperty 'junit.jupiter.execution.parallel.enabled', 'true'  




- [ ] Tus pruebas se ejecutan en paralelo




- [ ] Al cambiar la configuración `systemProperty 'junit.jupiter.execution.parallel.enabled', 'false’` tus pruebas se ejecutan secuencialmente  







