## Ejemplo 03: Diferentes tipos de repositorios 

### Objetivos
* Reconocer otros tipos de repositorios que se pueden utilizar con Spring Data Rest


### Procedimiento

Nos centraremos en diferentes tipos de interfaces de repositorio de Spring Data y su funcionalidad. Tocaremos: 

- CrudRepository
- PagingAndSortingRepository
- JpaRepository

En pocas palabras, cada repositorio en Spring Data amplía la interfaz genérica del repositorio , pero más allá de eso, cada uno tiene una funcionalidad diferente.


1. Comencemos con `CrudRepository`

```java
public interface CrudRepository<T, ID extends Serializable>
  extends Repository<T, ID> {

    <S extends T> S save(S entity);

    T findOne(ID primaryKey);

    Iterable<T> findAll();

    Long count();

    void delete(T entity);

    boolean exists(ID primaryKey);
}
```

Observe la funcionalidad CRUD típica:

- `save(…)`guarda un Iterable de entidades. Aquí, podemos pasar múltiples objetos para guardarlos en un lote
- `findOne(…)`obtenga una sola entidad basada en el valor de la clave principal pasada
- `findAll()`: obtenga un Iterable de todas las entidades disponibles en la base de datos
- `count ()` devuelve el recuento de entidades totales en una tabla
- `delete(…)` elimina una entidad basada en el objeto pasado
- `exists (...)` verifica si existe una entidad en función del valor de clave principal pasado

Esta interfaz parece bastante genérica y simple, pero en realidad proporciona todas las abstracciones de consulta básicas necesarias en una aplicación.

2. Ahora veamos `PagingAndSortingRepository`

```java
public interface PagingAndSortingRepository<T, ID extends Serializable> 
  extends CrudRepository<T, ID> {

    Iterable<T> findAll(Sort sort);

    Page<T> findAll(Pageable pageable);
}
```

Esta interfaz proporciona un método findAll(Pageable pageable) , que es la clave para implementar Paginación.

Cuando usamos Pageable , creamos un objeto Pageable con ciertas propiedades y tenemos que especificar al menos:

- Tamaño de página
- Número de página actual
- Clasificación
Entonces, supongamos que queremos mostrar la primera página de un conjunto de resultados ordenado por lastName, ascendente, que no tenga más de cinco registros cada uno. Así es como podemos lograr esto usando una PageRequest y una definición de clasificación :

```java
Sort sort = new Sort(new Sort.Order(Direction.ASC, "lastName"));
Pageable pageable = new PageRequest(0, 5, sort);
```

Pasar el objeto paginable a la consulta de datos de Spring devolverá los resultados en cuestión (el primer parámetro de PageRequest tiene base cero).

3. Finalmente veamos `JpaRepository`

```java 
public interface JpaRepository<T, ID extends Serializable> extends
  PagingAndSortingRepository<T, ID> {

    List<T> findAll();

    List<T> findAll(Sort sort);

    List<T> save(Iterable<? extends T> entities);

    void flush();

    T saveAndFlush(T entity);

    void deleteInBatch(Iterable<T> entities);
}
```

- `findAll()`  obtenga una lista de todas las entidades disponibles en la base de datos
- `findAll(…`)  obtenga una Lista de todas las entidades disponibles y clasifíquelas usando la condición provista
- `save(…)` guarda un Iterable de entidades. Aquí, podemos pasar múltiples objetos para guardarlos en un lote
- `flush ()`  descarga todas las tareas pendientes a la base de datos
- `saveAndFlush(…)` guarda la entidad y elimina los cambios inmediatamente
- `deleteInBatch(…)` elimina un Iterable de entidades. Aquí, podemos pasar varios objetos para eliminarlos en un lote


Claramente, la interfaz anterior extiende PagingAndSortingRepository , lo que significa que también tiene todos los métodos presentes en CrudRepository .