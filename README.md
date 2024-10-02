# JEP 477: Implicitly Declared Classes and Instance Main Methods (Third Preview) 

Es el tercero de una serie de JEP's, que persiguen reducir la ceremonia de escribir programas simples en Java y permitirle a los programadores juniors introducirse a conceptos de programación de manera gradual.

- [JEP 445: Unnamed Classes and Instance Main Methods (Preview)](https://openjdk.org/jeps/445)
- [JEP 463: Implicitly Declared Classes and Instance Main Methods (Second Preview)](https://openjdk.org/jeps/463)
- Hoy estamos aquí ¡[JEP 477](https://openjdk.org/jeps/477)! 

### Objetivos

- Ofrecer una vía de acceso fluida a Java para que los educadores puedan introducir conceptos de programación de manera gradual.

- Ayudar a los estudiantes a escribir programas básicos de manera concisa. 

- Reducir la ceremonia de escribir programas simples como: scripts y utilidades de línea de comandos. 

- No introducir un dialecto diferente de Java para principiantes.

- No introducir un conjunto de herramientas separadas para principiantes, Los programas de los principiantes deben compilarse y ejectuarse con las mismas herramientas que cualquier otro programa escrito en Java.

#### Instance Main Methods

##### El usual _public static void main(String[] args)_

```java
public class HolaMundo {
    public static void main(String[] args) {
        System.out.println(greetings());
    }

    static String greetings(){
        return "Hola, mundo!";
    }
}
```

En el bloque de código anterior se envuelven demasiados conceptos para la función que realiza.

- Clases 
- Modificadores de acceso 
- String[] como parámetro  

##### El nuevo _void main()_

El protocolo utilizado para lanzar programas Java fue modificado para permitir _instance main methods_, estos métodos: 

1. __No__ son estáticos
2. __No__ necesitan ser públicos 
3. __No__ necesitan tener un parámetro String[]

```java
class HolaMundo {
    void main(){
        System.out.println(greetings());
    }

    String greetings(){
	    return "Hola, mundo!";
    }
}
```
> Se debe habilitar el modo preview para ejecutar el bloque de código anterior.

```shell
$ java --source 23 --enable-preview HolaMundo.java
```

##### Flexibilidad en el protocolo de lanzamiento

Los cambios realizados en el protocolo de lanzamiento de programas Java, nos permite una mayor flexbilidad en cuanto los métodos main. 

Los nuevos _instance main methods_ permiten lo siguiente: 

- El método main puede tener los siguientes modificadores de acceso _(public, protected, o el modificador por defecto del package)_ 

El nuevo protocolo de lanzamiento también contiene una especie de regla de especificidad, que se compone de la siguiente manera, en orden de prioridad: 

1. Si existe un método main que recibe un arreglo de String _(String[])_ como parámetros, se elige ese método.

2. Si existe un método main sin parámetros, se elige ese método. 

Estos cambios son los que nos permiten escribir métodos main sin necesidad de incluir ningún concepto de programación avanzado o innecesario para los fines del programa a escribir.

```java
class HolaMundo {
    void main() {
        System.out.println("Hola, Mundo!");
    }
}
```

#### Implicitly declared classes

En Java, cada clase se aloja dentro de un paquete y cada paquete dentro de un módulo. 

Con los nuevos cambios realizados, si el compilador encuentra algún método que no está envuelto dentro de una clase lo considera parte del cuerpo de una clase implícitamente declarada. 

Otros métodos y campos que no se encuentren envueltos dentro de cualquier otra clase, en un archivo, también serán considerados por el compilador como parte de esa clase declarada implícitamente. 

>Una clase declarada implícitamente siempre es miembro del unnamed package. También es final y no implementa ninguna interfaz, ni extiende ninguna clase que no sea Object. No se puede hacer referencia a una clase implícita por su nombre, por lo que no puede haber referencias a sus métodos estáticos; Sin embargo, la palabra clave __this__ todavía se puede utilizar, al igual que las referencias a métodos de instancia.

__Cada clase implícita debe contener un método main.__

```java
String greetingMessage = "Hola, mundo!"; 

String greetings() {
	return greetingMessage;
} 

void main() {
	System.out.println(greetings());
}
```
> Se debe habilitar el modo preview para ejecutar el bloque de código anterior.

En el caso de las clases declaradas implícitamentes, el compilador de Java compilará ese archivo en el archivo de clase ejecutable basado en el nombre del archivo .java

```shell 
$ javac --source 23 --enable-preview HolaMundo.java
```
La compilación del bloque anterior producirá un archivo HolaMundo.class 

##### Interactuando con la consola

Interactuar con la consola en Java, involucra diferentes conceptos que de manera inicial para un programador junior pueden resultar innecesarios dada la finalidad de los programas que escribe mientras aprende de manera gradual conceptos de programación en general y de Java. 

Escribir por consola debería ser simplemente una llamada a un método, pero la realidad de Java envuelve otros conceptos dentro de la llamada a ese famoso método __println__. 

> Para escribir en la consola desde un programa en Java, necesitamos hacer _System.out.println()_

Un principiante curioso se preguntaría para que son todas las demás cuestiones incluidas en la simple llamada al método __println__ 

__¿Qué es System?, ¿Qué es out?, ¿Para qué son los puntos?__ 

Leer desde la consola debería igual, ser una simplme llamada a un método, pero la realidad de Java envuelve otros conceptos para poder leer desde la consola utilizando el famoso __System.in__

```java
try {
    BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
    String line = reader.readLine();
    ...
} catch (IOException ioe) {
    ...
}
```

__Qué es un try-catch?, ¿Qué es un BufferedReader?, ¿Qué es un InputStreamReader?__ 

Para reducir esa curva de aprendizaje y facilitar la vida de los principiantes en el lenguaje y en la programación en general, en este [JEP-477](https://openjdk.org/jeps/477), se han incluido tres métodos que pueden ser utilizados en el cuerpo de cualquier clase implícitamente declarada: 

1. public static void println(Object obj);
2. public static void print(Object obj);
3. public static String readln(String prompt);

El uso de estos métodos, en conjunto con la habilidad de no tener que declarar una clase explícitamente y poder escribir instance main method, reducen considerablemnte los conceptos envueltos en la construcción de programas simples en Java y facilitan el proceso de aprendizaje desde el punto de vista del docente y el estudiante. 

Ahora podemos simplemente escribir lo siguiente para tener el famoso Hola, mundo! en Java: 

```java 
void main(){
    println("Hola, mundo!";
}
```

> Recordar que para ejecutar estas funcionalidades, debemos habilitar el modo preview. 

```shell 
$ java --source 23 --enable-preview HolaMundo.java
```
