# Práctica: Creación de bibliotecas propias en C++ con archivos `.h` y `.cpp`

## Objetivo

Que el estudiante aprenda a separar su código en varios archivos mediante la creación de bibliotecas propias en C++, primero utilizando funciones sencillas y posteriormente utilizando una clase orientada a objetos. Al finalizar, el estudiante será capaz de declarar funciones y clases en archivos de encabezado `.h`, implementar su lógica en archivos `.cpp` y utilizarlas desde un programa principal.

---

## Introducción teórica

Cuando un programa empieza a crecer, colocar todo el código en un solo archivo puede volverlo difícil de leer, mantener y reutilizar. Por esta razón, en C++ es común dividir el proyecto en varios archivos.

Una forma básica de organizar un programa es crear una biblioteca propia. En este contexto, una biblioteca no significa algo complejo o externo, sino simplemente un conjunto de funciones o clases que el programador agrupa en archivos separados para reutilizarlas y mantener mejor organizado el código.

Normalmente se utilizan dos tipos de archivos:

* El archivo `.h` contiene las declaraciones.
* El archivo `.cpp` contiene las implementaciones.

El archivo principal, por ejemplo `main.cpp`, utiliza esa biblioteca mediante la directiva `#include`.

En esta práctica se trabajará en dos etapas:

1. Una biblioteca sencilla con funciones.
2. Una biblioteca con una clase usando Programación Orientada a Objetos.

Así, el estudiante podrá observar que la idea de separar el código aplica tanto para programación estructurada como para programación orientada a objetos.

---

## Material necesario

* Computadora con compilador de C++.
* Entorno de desarrollo como Code::Blocks, Visual Studio Code o similar.
* Conocimientos básicos de funciones, clases, objetos y archivos en C++.

---

## Parte 1. Biblioteca sencilla con funciones

## Propósito de esta parte

Que el estudiante comprenda la estructura básica de una biblioteca propia separando funciones matemáticas simples en archivos `.h` y `.cpp`.

---

## Estructura del proyecto

Para esta primera parte, el proyecto tendrá tres archivos:

* `main.cpp`
* `operaciones.h`
* `operaciones.cpp`

---

## Paso 1. Crear el archivo de encabezado `operaciones.h`

En este archivo se colocarán únicamente los prototipos de las funciones.

```cpp
#ifndef OPERACIONES_H
#define OPERACIONES_H

int sumar(int a, int b);
int restar(int a, int b);
int multiplicar(int a, int b);

#endif
```

### Explicación

* `#ifndef`, `#define` y `#endif` evitan que el archivo se incluya más de una vez.
* Aquí no se escribe la lógica de las funciones.
* Solo se indica qué funciones existen y qué parámetros reciben.

---

## Paso 2. Crear el archivo de implementación `operaciones.cpp`

En este archivo se escribe la lógica real de las funciones.

```cpp
#include "operaciones.h"

int sumar(int a, int b) {
    return a + b;
}

int restar(int a, int b) {
    return a - b;
}

int multiplicar(int a, int b) {
    return a * b;
}
```

### Explicación

* `#include "operaciones.h"` permite que este archivo conozca las declaraciones.
* Aquí sí se define lo que hace cada función.

---

## Paso 3. Crear el archivo principal `main.cpp`

Este archivo utilizará la biblioteca creada.

```cpp
#include <iostream>
#include "operaciones.h"
using namespace std;

int main() {
    int x, y;

    cout << "Ingrese el primer numero: ";
    cin >> x;

    cout << "Ingrese el segundo numero: ";
    cin >> y;

    cout << "Suma: " << sumar(x, y) << endl;
    cout << "Resta: " << restar(x, y) << endl;
    cout << "Multiplicacion: " << multiplicar(x, y) << endl;

    return 0;
}
```

### Explicación

* `main.cpp` no necesita conocer la lógica interna de las funciones.
* Solo necesita incluir el encabezado `.h`.
* Esto permite reutilizar la biblioteca en otros programas.

---

## Resultado esperado de la Parte 1

Al ejecutar el programa, el usuario debe poder capturar dos números y observar el resultado de varias operaciones realizadas por funciones que están organizadas en archivos separados.

---

## Parte 2. Biblioteca con una clase usando POO

## Propósito de esta parte

Que el estudiante observe cómo una clase también puede separarse en archivos `.h` y `.cpp`, siguiendo la misma idea de organización utilizada con funciones.

---

## Nuevo contexto del ejercicio

Ahora se desarrollará una clase sencilla llamada `Jugador`. Esta clase almacenará datos básicos de un jugador y tendrá métodos para mostrar información y aumentar su puntaje.

---

## Estructura del proyecto

Para esta segunda parte, el proyecto tendrá tres archivos nuevos:

* `main.cpp`
* `Jugador.h`
* `Jugador.cpp`

---

## Paso 1. Crear el archivo `Jugador.h`

```cpp
#ifndef JUGADOR_H
#define JUGADOR_H

#include <string>
using namespace std;

class Jugador {
private:
    int id;
    string nombre;
    int puntaje;

public:
    Jugador(int nuevoId, string nuevoNombre, int nuevoPuntaje);
    void mostrar();
    void agregarPuntos(int puntos);
};

#endif
```

### Explicación

* Aquí se declara la clase.
* Se indican sus atributos y sus métodos.
* Aún no se escribe la lógica de cada método.

---

## Paso 2. Crear el archivo `Jugador.cpp`

```cpp
#include <iostream>
#include "Jugador.h"
using namespace std;

Jugador::Jugador(int nuevoId, string nuevoNombre, int nuevoPuntaje) {
    id = nuevoId;
    nombre = nuevoNombre;
    puntaje = nuevoPuntaje;
}

void Jugador::mostrar() {
    cout << "ID: " << id << endl;
    cout << "Nombre: " << nombre << endl;
    cout << "Puntaje: " << puntaje << endl;
}

void Jugador::agregarPuntos(int puntos) {
    if (puntos > 0) {
        puntaje += puntos;
    }
}
```

### Explicación

* `Jugador::` indica que esos métodos pertenecen a la clase `Jugador`.
* El constructor inicializa los atributos.
* `mostrar()` imprime la información del objeto.
* `agregarPuntos()` modifica el estado interno del objeto.

---

## Paso 3. Crear el archivo principal `main.cpp`

```cpp
#include <iostream>
#include "Jugador.h"
using namespace std;

int main() {
    Jugador jugador1(1, "Carlos", 100);

    cout << "Datos iniciales del jugador:" << endl;
    jugador1.mostrar();

    jugador1.agregarPuntos(50);

    cout << endl;
    cout << "Datos despues de agregar puntos:" << endl;
    jugador1.mostrar();

    return 0;
}
```

### Explicación

* En `main.cpp` se crea un objeto de la clase `Jugador`.
* El programa principal usa la clase, pero la lógica de sus métodos está separada en otro archivo.
* Esto permite construir programas más ordenados.

---

## Comparación entre ambas partes

En la primera parte se separaron funciones sencillas en una biblioteca.
En la segunda parte se separó una clase completa.

La idea en ambos casos es la misma:

* El archivo `.h` declara.
* El archivo `.cpp` implementa.
* El archivo principal utiliza lo declarado.

Esto permite:

* Mejor organización.
* Mayor reutilización.
* Programas más fáciles de mantener.
* Separación clara entre interfaz y lógica.

---

## Actividad de comprobación

El estudiante deberá modificar ambos ejercicios.

### Actividad 1. Biblioteca con funciones

Agregar a la biblioteca `operaciones.h` y `operaciones.cpp` una nueva función llamada:

* `int dividirEntero(int a, int b);`

Condición:

* Si `b` vale 0, la función debe regresar 0.

Después, utilizarla desde `main.cpp`.

---

### Actividad 2. Biblioteca con clase

Modificar la clase `Jugador` para agregar:

* Un método `void disminuirPuntos(int puntos);`

Condición:

* Si los puntos a quitar son mayores que 0, se deben restar.
* El puntaje no debe quedar negativo.

Después, llamar ese método desde `main.cpp` y mostrar nuevamente el estado del jugador.

---

## Preguntas de reflexión

1. ¿Qué diferencia existe entre declarar una función o método y definirlo?
2. ¿Cuál es la función del archivo `.h`?
3. ¿Cuál es la función del archivo `.cpp`?
4. ¿Qué ventaja tiene separar una clase en archivos diferentes?
5. ¿Qué pasaría si se escribe toda la lógica en un solo archivo cuando el programa crece demasiado?

---

## Conclusión

En esta práctica el estudiante aprendió a construir sus propias bibliotecas en C++. Primero trabajó con funciones sencillas para comprender la estructura básica de separación en archivos `.h` y `.cpp`. Posteriormente aplicó la misma idea a una clase orientada a objetos, observando que este modelo de organización permite desarrollar programas más limpios, modulares y reutilizables.


