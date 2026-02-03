# Práctica 3

## Uso de estructuras (`struct`) y algoritmos de búsqueda y ordenamiento en C++

---

## 1. Objetivo de la práctica

Que el estudiante comprenda y aplique el uso de **estructuras de datos compuestas (`struct`)** en C++, integrando los algoritmos de **búsqueda** y **ordenamiento** estudiados previamente, mediante la simulación de un sistema básico de gestión de jugadores en un videojuego.

Al finalizar la práctica, el estudiante será capaz de:

* Definir estructuras para representar entidades de un videojuego.
* Crear arreglos de estructuras.
* Acceder y modificar los campos de una estructura.
* Implementar búsqueda lineal sobre estructuras.
* Implementar ordenamiento por criterio utilizando Bubble Sort.
* Mostrar reportes organizados de información.

---

## 2. Introducción teórica

En prácticas anteriores se trabajó con arreglos de datos simples, como enteros o flotantes. Sin embargo, en aplicaciones reales —como los videojuegos— los elementos no suelen representarse con un solo dato, sino con **conjuntos de información relacionados**.

Por ejemplo, un jugador puede tener:

* Un identificador
* Un puntaje
* Un nivel

Para representar este tipo de entidades se utilizan las **estructuras (`struct`)**, las cuales permiten agrupar múltiples variables bajo un solo nombre. Este enfoque facilita la organización de la información y hace posible aplicar algoritmos de búsqueda y ordenamiento sobre entidades completas, en lugar de sobre datos aislados.

---

## 3. Material y software requerido

* Computadora con sistema operativo Windows, Linux o macOS.
* Visual Studio Code.
* Compilador C++ (`g++`).
* Terminal para compilación y ejecución.

---

## 4. Estructura del proyecto

Archivo a crear:

* `practica3_struct_jugadores.cpp`

Compilación:

```bash
g++ practica3_struct_jugadores.cpp -o practica3
```

Ejecución:

* Windows:

```bash
practica3
```

* Linux / macOS:

```bash
./practica3
```

---

## 5. Bloque 1 — Definición de la estructura Jugador

En este bloque se define una estructura que representará a un jugador dentro de un videojuego.

### Código

```cpp
#include <iostream>
using namespace std;

struct Jugador {
    int id;
    int puntaje;
    int nivel;
};

int main() {
    return 0;
}
```

### Explicación

* `struct Jugador` define un nuevo tipo de dato.
* Cada campo representa una característica del jugador.
* En este punto el programa aún no realiza ninguna acción.

---

## 6. Bloque 2 — Arreglo de estructuras y captura de datos

A continuación se crea un arreglo de jugadores y se capturan sus datos desde el teclado.

### Código

```cpp
#include <iostream>
using namespace std;

struct Jugador {
    int id;
    int puntaje;
    int nivel;
};

int main() {
    const int N = 5;
    Jugador jugadores[N];

    cout << "Registro de jugadores\n";
    for (int i = 0; i < N; i++) {
        cout << "\nJugador " << i + 1 << endl;
        cout << "ID: ";
        cin >> jugadores[i].id;
        cout << "Puntaje: ";
        cin >> jugadores[i].puntaje;
        cout << "Nivel: ";
        cin >> jugadores[i].nivel;
    }

    cout << "\nJugadores registrados:\n";
    for (int i = 0; i < N; i++) {
        cout << "ID: " << jugadores[i].id
             << " | Puntaje: " << jugadores[i].puntaje
             << " | Nivel: " << jugadores[i].nivel << endl;
    }

    return 0;
}
```

### Explicación

* Se declara un arreglo de estructuras `Jugador`.
* Se accede a los campos mediante el operador punto (`.`).
* Cada posición del arreglo representa un jugador distinto.

---

## 7. Bloque 3 — Búsqueda lineal de un jugador por ID

En este bloque se implementa un algoritmo de búsqueda lineal para localizar un jugador a partir de su identificador.

### Código

```cpp
#include <iostream>
using namespace std;

struct Jugador {
    int id;
    int puntaje;
    int nivel;
};

int main() {
    const int N = 5;
    Jugador jugadores[N];
    int idBuscado;
    int posicion = -1;

    for (int i = 0; i < N; i++) {
        cout << "\nJugador " << i + 1 << endl;
        cout << "ID: ";
        cin >> jugadores[i].id;
        cout << "Puntaje: ";
        cin >> jugadores[i].puntaje;
        cout << "Nivel: ";
        cin >> jugadores[i].nivel;
    }

    cout << "\nIngrese el ID del jugador a buscar: ";
    cin >> idBuscado;

    for (int i = 0; i < N; i++) {
        if (jugadores[i].id == idBuscado) {
            posicion = i;
            break;
        }
    }

    if (posicion != -1) {
        cout << "\nJugador encontrado:\n";
        cout << "ID: " << jugadores[posicion].id
             << " | Puntaje: " << jugadores[posicion].puntaje
             << " | Nivel: " << jugadores[posicion].nivel << endl;
    } else {
        cout << "\nJugador no encontrado.\n";
    }

    return 0;
}
```

---

## 8. Bloque 4 — Ordenamiento de jugadores por puntaje (Bubble Sort)

Se adapta el algoritmo Bubble Sort para ordenar **estructuras completas** según el puntaje del jugador.

### Código

```cpp
#include <iostream>
using namespace std;

struct Jugador {
    int id;
    int puntaje;
    int nivel;
};

int main() {
    const int N = 5;
    Jugador jugadores[N];
    Jugador aux;

    for (int i = 0; i < N; i++) {
        cout << "\nJugador " << i + 1 << endl;
        cout << "ID: ";
        cin >> jugadores[i].id;
        cout << "Puntaje: ";
        cin >> jugadores[i].puntaje;
        cout << "Nivel: ";
        cin >> jugadores[i].nivel;
    }

    for (int i = 0; i < N - 1; i++) {
        for (int j = 0; j < N - 1 - i; j++) {
            if (jugadores[j].puntaje < jugadores[j + 1].puntaje) {
                aux = jugadores[j];
                jugadores[j] = jugadores[j + 1];
                jugadores[j + 1] = aux;
            }
        }
    }

    cout << "\nRanking de jugadores (por puntaje):\n";
    for (int i = 0; i < N; i++) {
        cout << i + 1 << ". ID: " << jugadores[i].id
             << " | Puntaje: " << jugadores[i].puntaje
             << " | Nivel: " << jugadores[i].nivel << endl;
    }

    return 0;
}
```

---

## 9. Bloque 5 — Identificación del mejor y peor jugador

Una vez ordenados los jugadores, se identifican los extremos del ranking.

### Concepto

* El primer jugador tiene el puntaje más alto.
* El último jugador tiene el puntaje más bajo.

Este resultado se obtiene directamente después del ordenamiento.

---

## 10. Ejercicios propuestos

### Ejercicio 1

Modificar el programa para ordenar los jugadores por **nivel** en lugar de puntaje.

---

### Ejercicio 2

Buscar un jugador por ID y mostrar su posición dentro del ranking.

---

### Ejercicio 3

Mostrar únicamente los jugadores cuyo puntaje sea mayor a un valor ingresado por el usuario.

---

### Ejercicio 4 — Extra (reto)

Permitir al usuario elegir el criterio de ordenamiento:

* 1 → Puntaje
* 2 → Nivel

---

## 11. Actividad de comprobación

El estudiante deberá responder por escrito:

1. ¿Qué es una estructura (`struct`)?
2. ¿Qué ventaja tiene usar estructuras frente a variables individuales?
3. ¿Cómo se adapta un algoritmo de ordenamiento para trabajar con estructuras?
4. ¿Por qué es útil este enfoque en videojuegos?


