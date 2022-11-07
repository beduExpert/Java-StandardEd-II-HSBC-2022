## Ejemplo 3: 

### Objetivo
- Comprobar la serialización/deserialización protocol buffers desde Spring Boot

### requicitos
- Reto 2
- JDK 8+

### desarrollo

1. Crea un test `SerializationTest` con la siguiente estructura:

```java
package org.bedu.ejemplo03;

import static org.assertj.core.api.Assertions.assertThat;

import org.bedu.ejemplo03.protos.models.LoginProto.Student;
import org.bedu.ejemplo03.protos.models.LoginProto.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.web.WebAppConfiguration;
import org.springframework.web.client.RestTemplate;

@SpringBootTest
@WebAppConfiguration
class SerializationTest {

	private static final String USER_URL = "http://localhost:8080/user/2";
	private static final String STUDENT_URL = "http://localhost:8080/student/1";

	@Autowired
	private RestTemplate restTemplate;

	@Test
	public void getUser() {
		ResponseEntity<User> user = restTemplate.getForEntity(USER_URL, User.class);
		System.out.println(String.format("\n%s \n", user.getBody()));
		assertThat(user).isNotNull();
	}
	
	@Test
	public void getStudent() {
		ResponseEntity<Student> student = restTemplate.getForEntity(STUDENT_URL, Student.class);
		System.out.println(String.format("\n%s \n", student.getBody()));
		assertThat(student).isNotNull();
	}
	
	@Test
	public void getUserOfStudent() {
		ResponseEntity<Student> student = restTemplate.getForEntity(STUDENT_URL, Student.class);
		System.out.println(String.format("\n%s \n", student.getBody()));
		assertThat(student.getBody().getUser().getEmail()).isNotEmpty();
	}

}

```

Como se muestra, se está haciendo uso de un RestTemplate para obtener los datos, se anexa una impresión de todos los datos en el System.out.println para que se observen todos los datos deserializados. 
Posteriormente se hacen las pruebas correspondientes.

2. Crea un Bean de configuración en el paquete de configuración con la siguiente estructura:

```java
package org.bedu.ejemplo03.protos.config;

import java.util.Arrays;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.http.converter.protobuf.ProtobufHttpMessageConverter;
import org.springframework.web.client.RestTemplate;

@Configuration
public class RestTemplateProtocolBufConfig {
	
	@Bean
	public RestTemplate restTemplate(ProtobufHttpMessageConverter hmc) {
	    return new RestTemplate(Arrays.asList(hmc));
	}
}

```

Esta marcada con la anotación `@Configuration` proporciona el Bean que se utiliza en los test. Como se muestra utiliza una clase `ProtobufHttpMessageConverter` que es la encargada de realizar el `parseo` de los datos.


3. Ejecuta la aplicación.

4. Ejecuta los test (corriendo la app). Si todo está correcto, los test de serialización deberían pasar.

5. Analiza los resultados.