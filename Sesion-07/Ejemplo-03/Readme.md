# Ejemplo 3 - TestNG

## :dart: Objetivos

- Comparar las herramientas JUnit y TestNG
- Identificar las características principales de TestNG

## ⚙ Requisitos

- IntelliJ IDEA
- Java
- Gradle
- JUnit
- TestNG

## Desarrollo

### ¿Qué es TestNG?

TestNG es un marco de pruebas de automatización en el que NG significa "Próxima Generación". TestNG está inspirado en
JUnit, que usa las anotaciones (@). TestNG supera las desventajas de JUnit y está diseñado para facilitar las pruebas de
un extremo a otro.

Con TestNG, puedes generar un informe adecuado y puedes saber fácilmente cuántos casos de prueba se pasan, fallan y se
omiten. Puedes ejecutar los casos de prueba fallidos por separado.

Por ejemplo:

- Imagina que tienes cinco casos de prueba, escribes un método para cada caso de prueba (supongamos que el programa se
  escribe usando el método principal sin usar testNG). Cuando ejecutas este programa primero, tres métodos se ejecutan
  correctamente y el cuarto método falla. Luego corriges los errores presentes en el cuarto método, ahora solo deseas
  ejecutar el cuarto método porque los primeros tres métodos se ejecutan de todos modos con éxito. Esto no es posible
  sin utilizar TestNG.
- Hablando de TestNG con Selenium se genera el archivo testng-failed.xml en la carpeta de salida de la prueba. Si deseas
  ejecutar solo casos de prueba fallidos, lo único que debes hacer es ejecutar este archivo XML. De esta forma se
  ejecutarán solo casos de prueba fallidos.

### ¿Por qué utilizar TestNG con Selenium?

Las pruebas de Selenium predeterminadas no generan un formato adecuado para los resultados. Usando TestNG con Selenium,
podemos generar mejores reportes de resultados de pruebas.

La mayoría de los usuarios de Selenium usan TestNg más que Junit debido a sus ventajas. Hay tantas características de
TestNG, pero solo nos centraremos en las más importantes que podemos usar en Selenium. Las siguientes son las
características clave de Selenium TestNG:

- Generar el informe en un formato adecuado que incluya una serie de ejecuciones de casos de prueba, la cantidad de
  casos de prueba aprobados, la cantidad de casos de prueba fallidos y la cantidad de casos de prueba omitidos.
- Se pueden agrupar varios casos de prueba más fácilmente convirtiéndolos en un archivo testng.xml. En el que puede
  establecer prioridades sobre qué caso de prueba debe ejecutarse primero.
- El mismo caso de prueba se puede ejecutar varias veces sin bucles con solo usar la palabra clave llamada _"invocation
  count"._
- Con TestNG, puedes ejecutar varios casos de prueba en varios navegadores.
- TestNG se puede integrar fácilmente con herramientas como TestNG Maven, Jenkins, etc.
- Las anotaciones utilizadas en las pruebas son muy fáciles de entender, por ejemplo: @BeforeMethod, @AfterMethod,
  @BeforeTest, @AfterTest
- WebDriver no tiene un mecanismo nativo para generar informes. TestNG puede generar el informe en un formato legible.

`build.gradle`
```
plugins {
    id 'java'
}

group 'org.example'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'junit:junit:4.13.1'
    testImplementation 'org.testng:testng:7.4.0'
    implementation 'org.seleniumhq.selenium:selenium-java:3.141.59'
    implementation 'org.seleniumhq.selenium:selenium-support:3.141.59'
}

test {
    useTestNG() {
        // To generate reports by TestNG library
        useDefaultListeners = true
    }
}
```

`TestGitHub.java`
```java

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.chrome.ChromeOptions;
import org.testng.annotations.AfterTest;
import org.testng.annotations.BeforeTest;
import org.testng.annotations.Test;

import static org.testng.Assert.assertEquals;


public class TestGitHub {

  private WebDriver driver;


  @BeforeTest
  public void setup() {
    System.setProperty("webdriver.chrome.driver", System.getProperty("user.dir") + "/chromedriver_linux64/chromedriver");
    ChromeOptions options = new ChromeOptions();
    options.addArguments("start-maximized"); // open Browser in maximized mode
    options.addArguments("disable-infobars"); // disabling infobars
    options.addArguments("--disable-extensions"); // disabling extensions
    options.addArguments("--disable-dev-shm-usage"); // overcome limited resource problems
    options.addArguments("--no-sandbox"); // Bypass OS security model
    driver = new ChromeDriver(options);
  }

  @AfterTest
  public void teardown() {
    if (driver != null) {
      driver.quit();
    }
  }

  @Test
  public void verifyGitHubTitle() {
    driver.get("https://www.github.com");
    assertEquals(driver.getTitle(), "GitHub");
  }
}
```
