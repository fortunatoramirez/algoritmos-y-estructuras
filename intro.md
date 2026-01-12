
# Práctica 1

## Introducción a C++ y Algoritmos de Ordenamiento aplicados a Videojuegos

---

## 1. Objetivo

Que el estudiante conozca la estructura básica del lenguaje **C++**, comprenda su uso en la resolución de problemas algorítmicos y sea capaz de implementar **algoritmos de ordenamiento simples**, aplicándolos a situaciones típicas del desarrollo de videojuegos, como puntajes, tiempos y niveles.

---

## 2. Introducción teórica

C++ es un lenguaje de programación de propósito general ampliamente utilizado en el desarrollo de videojuegos debido a su alto rendimiento, control de memoria y cercanía con el hardware. Estas características lo convierten en una herramienta ideal para el estudio de **algoritmos y estructuras de datos**, ya que permite comprender con mayor claridad cómo se procesan y organizan los datos en memoria.

En los videojuegos, los algoritmos se emplean constantemente para:

* Ordenar puntajes.
* Clasificar jugadores.
* Administrar niveles.
* Procesar tiempos y eventos.
* Optimizar la ejecución del juego.

En esta práctica se abordarán los fundamentos del lenguaje C++ y se introducirá el concepto de **ordenamiento de datos**, uno de los pilares del análisis algorítmico.

---

## 3. Estructura básica de un programa en C++

Todo programa en C++ cuenta con una función principal denominada `main`, la cual representa el punto de inicio de la ejecución.

### Código base

```cpp
#include <iostream>

using namespace std;

int main() {
    cout << "Introduccion a C++ para videojuegos" << endl;
    return 0;
}
```

### Explicación

* `#include <iostream>` permite el uso de entrada y salida estándar.
* `using namespace std;` evita escribir el prefijo `std::`.
* `main()` es la función principal del programa.
* `cout` imprime información en pantalla.
* `return 0;` indica que el programa finalizó correctamente.

---

## 4. Variables y arreglos en contexto de videojuegos

En un videojuego, es común manejar múltiples valores del mismo tipo, como puntajes de jugadores o tiempos de carrera. Para ello se utilizan **arreglos**.

### Ejemplo: arreglo de puntajes

```cpp
#include <iostream>
using namespace std;

int main() {
    int puntajes[5] = {120, 90, 150, 60, 110};

    for (int i = 0; i < 5; i++) {
        cout << "Puntaje " << i + 1 << ": " << puntajes[i] << endl;
    }

    return 0;
}
```

---

## 5. Introducción a los algoritmos de ordenamiento

Un **algoritmo de ordenamiento** es un procedimiento que organiza un conjunto de datos siguiendo un criterio determinado, como de menor a mayor o de mayor a menor.

En videojuegos, los algoritmos de ordenamiento se utilizan para:

* Mostrar tablas de mejores puntajes.
* Clasificar jugadores por tiempo.
* Priorizar eventos o acciones.
* Organizar niveles o enemigos.

---

## 6. Algoritmo de ordenamiento Burbuja (Bubble Sort)

El método de la burbuja es uno de los algoritmos de ordenamiento más simples. Consiste en comparar elementos adyacentes e intercambiarlos si están en el orden incorrecto.

### Ejemplo: ordenar puntajes de menor a mayor

```cpp
#include <iostream>
using namespace std;

int main() {
    int puntajes[5] = {120, 90, 150, 60, 110};
    int aux;

    for (int i = 0; i < 5 - 1; i++) {
        for (int j = 0; j < 5 - 1 - i; j++) {
            if (puntajes[j] > puntajes[j + 1]) {
                aux = puntajes[j];
                puntajes[j] = puntajes[j + 1];
                puntajes[j + 1] = aux;
            }
        }
    }

    cout << "Puntajes ordenados:" << endl;
    for (int i = 0; i < 5; i++) {
        cout << puntajes[i] << endl;
    }

    return 0;
}
```

---

## 7. Interpretación en contexto de videojuego

En este ejemplo:

* El arreglo `puntajes` representa los puntajes obtenidos por distintos jugadores.
* El algoritmo ordena los puntajes para mostrar una tabla de resultados.
* El ciclo externo controla el número de pasadas.
* El ciclo interno realiza las comparaciones e intercambios.

Este tipo de lógica es utilizada en sistemas de ranking y tablas de líderes.

---

## 8. Ejercicios propuestos

### Ejercicio 1 — Ordenamiento de tiempos de carrera

Solicitar al usuario **5 tiempos** (en segundos) de jugadores que terminaron una carrera y ordenarlos de **menor a mayor**, mostrando el tiempo más rápido al inicio.

---

### Ejercicio 2 — Tabla de puntajes descendente

Utilizar un arreglo con **10 puntajes** e implementar el algoritmo de burbuja para ordenarlos de **mayor a menor**, simulando una tabla de mejores jugadores.

---

### Ejercicio 3 — Identificación del mejor jugador

Una vez ordenados los puntajes:

* Mostrar el puntaje más alto.
* Mostrar el puntaje más bajo.

---

### Ejercicio 4 — Extra (reto)

Modificar el programa para que:

* Solicite al usuario la cantidad de jugadores.
* Capture dinámicamente los puntajes.
* Ordene los datos y muestre el ranking final.

---

## 9. Actividad de comprobación

El estudiante deberá explicar por escrito:

* Qué es un algoritmo de ordenamiento.
* Para qué sirve el algoritmo de burbuja.
* En qué situaciones reales de un videojuego se aplicaría este algoritmo.
* Qué desventajas tiene este algoritmo frente a otros más avanzados.

---

## 10. Conclusiones de la práctica

Mediante esta práctica, el estudiante:

* Conoció la estructura básica de un programa en C++.
* Comprendió el uso de arreglos para almacenar datos.
* Implementó su primer algoritmo de ordenamiento.
* Relacionó los conceptos de algoritmos con situaciones reales del desarrollo de videojuegos.

Estos conocimientos constituyen la base para abordar algoritmos de mayor complejidad y estructuras de datos más avanzadas en prácticas posteriores.


