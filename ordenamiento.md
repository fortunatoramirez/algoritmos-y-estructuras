# Práctica 2

## Algoritmos de Búsqueda en C++ aplicados a escenarios de videojuegos

---

## 1. Objetivo de la práctica

Que el estudiante sea capaz de implementar en **C++** algoritmos de búsqueda sobre arreglos, comprendiendo su lógica, diferencias y costo computacional, mediante ejercicios contextualizados a videojuegos (búsqueda de puntajes, ítems, niveles, IDs de jugadores).

Al finalizar la práctica, el estudiante podrá:

* Capturar y almacenar datos en arreglos.
* Implementar **búsqueda lineal (secuencial)**.
* Implementar **búsqueda binaria** (sobre datos ordenados).
* Comparar resultados y número de comparaciones realizadas.

---

## 2. Introducción teórica

En programación, un **algoritmo de búsqueda** permite localizar un elemento específico dentro de una colección de datos. En videojuegos, esto aparece de manera constante, por ejemplo:

* Encontrar el jugador con cierto **ID**.
* Verificar si un ítem existe en el inventario.
* Confirmar si un puntaje ya está registrado.
* Buscar el nivel actual dentro de una lista de niveles desbloqueados.

Los dos algoritmos más importantes para iniciar son:

1. **Búsqueda lineal:** recorre uno por uno hasta encontrar el dato.
2. **Búsqueda binaria:** reduce el espacio de búsqueda a la mitad en cada paso, pero requiere que los datos estén **ordenados**.

---

## 3. Material y software requerido

* Computadora con Windows / Linux / macOS.
* Visual Studio Code.
* Compilador C++ (g++ mediante MinGW-w64 o MSYS2 en Windows).
* Terminal para compilar y ejecutar.

---

## 4. Código base y estructura del proyecto

Se trabajará en un archivo llamado:

* `practica2_busqueda.cpp`

Para compilar:

```bash
g++ practica2_busqueda.cpp -o practica2
```

Para ejecutar:

* Windows:

```bash
practica2
```

* Linux/macOS:

```bash
./practica2
```

---

## 5. Bloque 1 — Captura de datos de “jugadores” (arreglo)

En este bloque el estudiante capturará IDs de jugadores (enteros). En un videojuego, estos IDs podrían representar usuarios registrados.

### Código

```cpp
#include <iostream>
using namespace std;

int main() {
    const int N = 8;
    int ids[N];

    cout << "Captura " << N << " IDs de jugadores (enteros):" << endl;
    for (int i = 0; i < N; i++) {
        cout << "ID[" << i << "]: ";
        cin >> ids[i];
    }

    cout << "\nIDs capturados:" << endl;
    for (int i = 0; i < N; i++) {
        cout << ids[i] << " ";
    }
    cout << endl;

    return 0;
}
```

---

## 6. Bloque 2 — Implementación de búsqueda lineal (secuencial)

La búsqueda lineal recorre el arreglo desde el inicio y compara cada elemento con el valor buscado.

### Código

```cpp
#include <iostream>
using namespace std;

int main() {
    const int N = 8;
    int ids[N];
    int buscado;

    cout << "Captura " << N << " IDs de jugadores:" << endl;
    for (int i = 0; i < N; i++) {
        cout << "ID[" << i << "]: ";
        cin >> ids[i];
    }

    cout << "\nIngresa el ID a buscar: ";
    cin >> buscado;

    int pos = -1;
    int comparaciones = 0;

    for (int i = 0; i < N; i++) {
        comparaciones++;
        if (ids[i] == buscado) {
            pos = i;
            break;
        }
    }

    if (pos != -1) {
        cout << "Encontrado. Posicion: " << pos << endl;
    } else {
        cout << "No encontrado." << endl;
    }
    cout << "Comparaciones realizadas: " << comparaciones << endl;

    return 0;
}
```

---

## 7. Bloque 3 — Ordenamiento previo para búsqueda binaria

La búsqueda binaria requiere datos ordenados. En esta práctica se ordenará el arreglo usando **Bubble Sort** (ya visto en la Práctica 1) para cumplir el requisito.

### Código (ordenamiento burbuja)

```cpp
void bubbleSort(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                int aux = a[j];
                a[j] = a[j + 1];
                a[j + 1] = aux;
            }
        }
    }
}
```

---

## 8. Bloque 4 — Implementación de búsqueda binaria

La búsqueda binaria divide el arreglo en mitades, reduciendo el número de comparaciones, pero solo si el arreglo está ordenado.

### Código completo con ordenamiento + búsqueda binaria

```cpp
#include <iostream>
using namespace std;

void bubbleSort(int a[], int n) {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - 1 - i; j++) {
            if (a[j] > a[j + 1]) {
                int aux = a[j];
                a[j] = a[j + 1];
                a[j + 1] = aux;
            }
        }
    }
}

int busquedaBinaria(int a[], int n, int x, int &comparaciones) {
    int izq = 0;
    int der = n - 1;
    comparaciones = 0;

    while (izq <= der) {
        int mid = (izq + der) / 2;
        comparaciones++;

        if (a[mid] == x) return mid;
        if (x < a[mid]) der = mid - 1;
        else izq = mid + 1;
    }
    return -1;
}

int main() {
    const int N = 8;
    int ids[N];
    int buscado;

    cout << "Captura " << N << " IDs de jugadores:" << endl;
    for (int i = 0; i < N; i++) {
        cout << "ID[" << i << "]: ";
        cin >> ids[i];
    }

    bubbleSort(ids, N);

    cout << "\nIDs ordenados (requisito para busqueda binaria):" << endl;
    for (int i = 0; i < N; i++) cout << ids[i] << " ";
    cout << endl;

    cout << "\nIngresa el ID a buscar: ";
    cin >> buscado;

    int comparaciones = 0;
    int pos = busquedaBinaria(ids, N, buscado, comparaciones);

    if (pos != -1) {
        cout << "Encontrado. Posicion: " << pos << endl;
    } else {
        cout << "No encontrado." << endl;
    }
    cout << "Comparaciones realizadas (binaria): " << comparaciones << endl;

    return 0;
}
```

---

## 9. Ejercicios propuestos (en contexto de videojuegos)

### Ejercicio 1 — Inventario (búsqueda lineal)

El estudiante deberá simular un inventario con 12 códigos de ítems (enteros).
Se solicitará un código a buscar y se mostrará si existe y en qué posición.

---

### Ejercicio 2 — Ranking (búsqueda binaria)

Se capturarán 15 puntajes.
Se ordenarán y luego se buscará un puntaje específico usando búsqueda binaria.

---

### Ejercicio 3 — Comparación de eficiencia

Para un mismo arreglo de 20 elementos:

* Buscar un valor con búsqueda lineal y contar comparaciones.
* Buscar el mismo valor con búsqueda binaria (ordenando antes) y contar comparaciones.
* Concluir cuál hizo menos comparaciones.

---

### Ejercicio 4 — Extra (reto)

Modificar el programa para que:

* Si el ID no existe, se indique: “Jugador no registrado”.
* Si sí existe, se muestre: “Jugador encontrado: ID = X”.

---

## 10. Actividad de comprobación

El estudiante deberá responder por escrito:

1. ¿Cuál es la diferencia entre búsqueda lineal y binaria?
2. ¿Por qué la búsqueda binaria requiere un arreglo ordenado?
3. ¿En qué situaciones dentro de un videojuego sería aceptable usar búsqueda lineal?
4. ¿Qué ventaja principal ofrece la búsqueda binaria?


