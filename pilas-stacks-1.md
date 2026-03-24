# Sistema de historial y deshacer acciones en un videojuego usando pilas (Stack) en C++

---

## 1. Objetivo de la práctica

Que el estudiante implemente una **pila** en C++ como parte de un **mini motor lógico de videojuego**, utilizándola para registrar y deshacer acciones recientes del jugador. Al finalizar, el estudiante será capaz de:

* Comprender el comportamiento **LIFO** (*Last In, First Out*), es decir, último en entrar, primero en salir.
* Relacionar la estructura de pila con un problema real de software: el sistema de historial y deshacer acciones.
* Implementar una pila utilizando nodos y memoria dinámica.
* Registrar acciones del jugador dentro del historial.
* Consultar la acción más reciente.
* Deshacer una o varias acciones.
* Recorrer e imprimir el historial desde la acción más reciente hasta la más antigua.

---

## 2. Introducción teórica

En un videojuego real no basta con dibujar personajes o mover objetos. Internamente, el sistema debe llevar control de múltiples eventos: movimientos, ataques, recolección de objetos, interacción con puertas, uso de inventario, activación de poderes, entre otros. Cuando un juego o una aplicación desea permitir la operación **deshacer**, necesita recordar qué ocurrió recientemente y en qué orden ocurrió.

Una estructura muy útil para este problema es la **pila**.

Una pila organiza la información siguiendo una regla fundamental:

* **LIFO**: *Last In, First Out*.
* En español: **último en entrar, primero en salir**.

Esto significa que el elemento más reciente almacenado será el primero que podrá recuperarse o eliminarse.

### Ejemplo conceptual dentro de un videojuego

Supóngase que un jugador realiza las siguientes acciones en este orden:

1. Caminar a la derecha.
2. Recoger una moneda.
3. Atacar a un enemigo.
4. Abrir una puerta.

Si el sistema necesita deshacer la última acción, no debe eliminar primero la caminata ni la recolección de moneda. Debe eliminar primero la acción más reciente, es decir:

* Abrir una puerta.

Si después se vuelve a deshacer, entonces se elimina:

* Atacar a un enemigo.

Ese comportamiento corresponde exactamente a una pila.

---

## 3. Relación con un motor de videojuego del

En las prácticas anteriores se ha ido construyendo, paso a paso, la lógica de un pequeño sistema de videojuego:

* En la práctica de Programación Orientada a Objetos se modelaron entidades como `Jugador`, `Enemigo` e `Item`.
* En la práctica de lista enlazada se almacenaron dinámicamente entidades activas del juego.

Ahora se agregará un nuevo componente:

* **El historial de acciones del jugador**.

Este historial permitirá registrar eventos recientes y simular una característica típica de muchos sistemas interactivos: **deshacer**.

Con esto, el estudiante no verá la pila solo como una estructura abstracta, sino como una herramienta útil dentro de una arquitectura más amplia de software.

---

## 4. ¿Por qué una pila y no una lista general?

En una lista enlazada general se pueden realizar operaciones como:

* Insertar al inicio.
* Insertar al final.
* Buscar por ID.
* Eliminar un nodo específico.

Sin embargo, en un historial de acciones no interesa insertar o eliminar en cualquier parte. Lo importante es respetar el orden temporal de las acciones recientes.

Por ello, la pila restringe las operaciones a un solo extremo:

* Se inserta siempre en la cima.
* Se elimina siempre desde la cima.
* Se consulta siempre la cima.

Gracias a esa restricción, la estructura se adapta perfectamente al problema de historial y deshacer.

---

## 5. Operaciones fundamentales de una pila

Las operaciones clásicas de una pila son:

* `push`: insertar un elemento en la cima.
* `pop`: eliminar el elemento de la cima.
* `top` o `peek`: consultar el elemento de la cima sin eliminarlo.
* `isEmpty`: verificar si la pila está vacía.

En esta práctica se usarán nombres más descriptivos para que el estudiante relacione mejor la estructura con el videojuego:

* `registrarAccion()` equivale a `push()`.
* `deshacerAccion()` equivale a `pop()`.
* `ultimaAccion()` equivale a `top()` o `peek()`.
* `estaVacia()` equivale a `isEmpty()`.

---

## 6. Material y software requerido

* Visual Studio Code.
* Compilador `g++`.
* Archivo de trabajo: `practica6_historial_pila.cpp`.

Compilación:

```bash
g++ practica6_historial_pila.cpp -o practica6
```

Ejecución:

* Windows:

```bash
practica6
```

* Linux / macOS:

```bash
./practica6
```

---

## 7. Escenario de la práctica

Se desea implementar un sistema básico donde un jugador pueda realizar acciones dentro del juego y el programa registre esas acciones para consultarlas o deshacerlas después.

Las acciones que el sistema podrá almacenar son ejemplos como:

* mover_derecha
* mover_izquierda
* atacar
* recoger_item
* usar_pocion
* abrir_puerta

Cada vez que el jugador realice una acción, esta quedará guardada en la pila. La última acción realizada será la primera que pueda deshacerse.

---

## 8. Bloque 1. Modelado de una acción del jugador

En lugar de almacenar solamente cadenas de texto, se utilizará una clase llamada `AccionJuego`.

### Código

```cpp
#include <iostream>
#include <string>
using namespace std;

class AccionJuego {
private:
    int id;
    string tipo;
    string descripcion;
    int energiaConsumida;

public:
    AccionJuego(int nuevoId, string nuevoTipo, string nuevaDescripcion, int nuevaEnergia) {
        id = (nuevoId > 0) ? nuevoId : 1;
        tipo = nuevoTipo;
        descripcion = nuevaDescripcion;
        energiaConsumida = (nuevaEnergia >= 0) ? nuevaEnergia : 0;
    }

    int getId() const {
        return id;
    }

    string getTipo() const {
        return tipo;
    }

    string getDescripcion() const {
        return descripcion;
    }

    int getEnergiaConsumida() const {
        return energiaConsumida;
    }

    void mostrar() const {
        cout << "Accion | ID: " << id
             << " | Tipo: " << tipo
             << " | Descripcion: " << descripcion
             << " | Energia: " << energiaConsumida << endl;
    }
};
```

### Explicación

* La clase `AccionJuego` representa una acción ejecutada por el jugador.
* `id` identifica la acción.
* `tipo` clasifica la acción, por ejemplo: `movimiento`, `combate`, `interaccion`.
* `descripcion` describe la acción realizada.
* `energiaConsumida` agrega un dato adicional del contexto del juego.
* El método `mostrar()` imprime la información completa.

Este diseño permite que el historial guarde objetos más ricos que un simple texto.

---

## 9. Bloque 2. Estructura del nodo del historial

La pila se implementará mediante nodos enlazados. Cada nodo almacenará un apuntador a una acción y una referencia al nodo siguiente.

### Código

```cpp
struct NodoHistorial {
    AccionJuego* accion;
    NodoHistorial* sig;

    NodoHistorial(AccionJuego* nuevaAccion) {
        accion = nuevaAccion;
        sig = nullptr;
    }
};
```

### Explicación

* `accion` apunta a un objeto de tipo `AccionJuego`.
* `sig` enlaza con el siguiente nodo de la pila.
* El nodo que esté en la cima apuntará al siguiente elemento del historial.
* El último nodo tendrá `sig = nullptr`.

En esta práctica, la pila se visualizará como una columna de acciones, donde la parte superior corresponde al evento más reciente.

---

## 10. Bloque 3. Clase HistorialAcciones del mini motor

Ahora se implementará la clase que administrará el historial completo.

### Código

```cpp
class HistorialAcciones {
private:
    NodoHistorial* top;

public:
    HistorialAcciones() {
        top = nullptr;
    }

    bool estaVacia() const {
        return top == nullptr;
    }

    ~HistorialAcciones() {
        while (!estaVacia()) {
            NodoHistorial* borrar = top;
            top = top->sig;
            delete borrar->accion;
            delete borrar;
        }
    }
};
```

### Explicación

* `top` representa la cima de la pila.
* Si `top` es `nullptr`, el historial está vacío.
* El constructor inicializa la estructura vacía.
* El destructor elimina todos los nodos y libera la memoria dinámica.

Este historial se integrará como si fuera un módulo interno del mini motor de videojuego.

---

## 11. Bloque 4. Registrar una acción en el historial

Cada vez que el jugador realice una nueva acción, esta se registrará en la cima de la pila.

### Código para agregar dentro de `HistorialAcciones`

```cpp
void registrarAccion(AccionJuego* nuevaAccion) {
    NodoHistorial* nuevo = new NodoHistorial(nuevaAccion);
    nuevo->sig = top;
    top = nuevo;
}
```

### Explicación

* Se crea un nodo nuevo.
* Ese nodo apunta a la acción que ya estaba en la cima.
* Después, el nuevo nodo pasa a ser la nueva cima.

Esta operación equivale a `push()`.

### Ejemplo conceptual

Si el historial contiene:

* cima → atacar
* debajo → recoger_item
* debajo → mover_derecha

y se registra la acción **abrir_puerta**, entonces la pila quedará:

* cima → abrir_puerta
* debajo → atacar
* debajo → recoger_item
* debajo → mover_derecha

---

## 12. Bloque 5. Consultar la última acción realizada

Ahora se implementará un método para observar la acción más reciente sin eliminarla.

### Código para agregar dentro de `HistorialAcciones`

```cpp
AccionJuego* ultimaAccion() const {
    if (estaVacia()) {
        return nullptr;
    }
    return top->accion;
}
```

### Explicación

* Si no existen elementos, se retorna `nullptr`.
* Si la pila tiene contenido, se devuelve la acción ubicada en la cima.
* Esta operación permite conocer qué acción sería deshecha si se ejecutara el siguiente paso.

Esta operación equivale a `top()` o `peek()`.

---

## 13. Bloque 6. Deshacer la acción más reciente

El objetivo central de esta práctica es implementar la lógica de “deshacer”. Para ello, se elimina la acción ubicada en la cima.

### Código para agregar dentro de `HistorialAcciones`

```cpp
bool deshacerAccion() {
    if (estaVacia()) {
        return false;
    }

    NodoHistorial* borrar = top;
    top = top->sig;

    delete borrar->accion;
    delete borrar;

    return true;
}
```

### Explicación

* Si la pila está vacía, no hay nada que deshacer.
* Se guarda temporalmente el nodo de la cima.
* Se actualiza `top` para que apunte al siguiente nodo.
* Se libera la memoria del objeto `AccionJuego`.
* Se libera también la memoria del nodo.

Esta operación equivale a `pop()`.

---

## 14. Bloque 7. Mostrar el historial completo del jugador

Aunque una pila normalmente opera solo sobre la cima, en esta práctica conviene visualizar todo el historial para observar claramente el comportamiento LIFO.

### Código para agregar dentro de `HistorialAcciones`

```cpp
void mostrarHistorial() const {
    if (estaVacia()) {
        cout << "No hay acciones registradas en el historial." << endl;
        return;
    }

    NodoHistorial* actual = top;
    int contador = 1;

    cout << "Historial de acciones del jugador:" << endl;
    cout << "(de la mas reciente a la mas antigua)" << endl;

    while (actual != nullptr) {
        cout << "Accion " << contador << ": ";
        actual->accion->mostrar();
        actual = actual->sig;
        contador++;
    }
}
```

### Explicación

* Se inicia el recorrido desde `top`.
* La primera acción mostrada será la más reciente.
* El recorrido continúa hasta llegar a `nullptr`.
* Esta visualización ayuda a que el estudiante entienda la pila como historial invertido respecto al orden de ejecución original.

---

## 15. Bloque 8. Contar acciones registradas

El siguiente método permite saber cuántas acciones existen actualmente en el historial.

### Código para agregar dentro de `HistorialAcciones`

```cpp
int contarAcciones() const {
    int contador = 0;
    NodoHistorial* actual = top;

    while (actual != nullptr) {
        contador++;
        actual = actual->sig;
    }

    return contador;
}
```

### Explicación

* Se recorre la pila nodo por nodo.
* Cada nodo incrementa el contador.
* Al finalizar se devuelve el total.

Este método es útil para mostrar el tamaño actual del historial.

---

## 16. Bloque 9. Deshacer varias acciones consecutivas

Para darle mayor sentido de sistema real, se implementará una operación adicional: deshacer varias acciones en secuencia.

### Código para agregar dentro de `HistorialAcciones`

```cpp
void deshacerVarias(int cantidad) {
    for (int i = 0; i < cantidad; i++) {
        if (!deshacerAccion()) {
            cout << "Ya no hay mas acciones por deshacer." << endl;
            break;
        }
    }
}
```

### Explicación

* El método recibe el número de acciones que se desean deshacer.
* Usa un ciclo para invocar repetidamente `deshacerAccion()`.
* Si la pila se vacía antes de completar la cantidad solicitada, el proceso se detiene.

Esta función refuerza el uso de la pila como un historial real de operaciones recientes.

---

## 17. Bloque 10. Programa principal del sistema de historial

En este bloque se integra todo el sistema mediante un menú de prueba. El estudiante podrá registrar acciones, ver la última, deshacerlas y observar el historial.

### Código completo del `main`

```cpp
int main() {
    HistorialAcciones historial;
    int opcion;

    do {
        cout << "\n=====================================" << endl;
        cout << " Mini motor de videojuego - Historial" << endl;
        cout << "=====================================" << endl;
        cout << "1) Registrar accion del jugador" << endl;
        cout << "2) Ver ultima accion realizada" << endl;
        cout << "3) Deshacer ultima accion" << endl;
        cout << "4) Mostrar historial completo" << endl;
        cout << "5) Contar acciones registradas" << endl;
        cout << "6) Deshacer varias acciones" << endl;
        cout << "0) Salir" << endl;
        cout << "Opcion: ";
        cin >> opcion;

        if (opcion == 1) {
            int id, energia;
            string tipo, descripcion;

            cout << "ID de la accion: ";
            cin >> id;
            cout << "Tipo de accion: ";
            cin >> tipo;
            cout << "Descripcion de la accion: ";
            cin >> descripcion;
            cout << "Energia consumida: ";
            cin >> energia;

            historial.registrarAccion(
                new AccionJuego(id, tipo, descripcion, energia)
            );

            cout << "Accion registrada correctamente en el historial." << endl;
        }
        else if (opcion == 2) {
            AccionJuego* accion = historial.ultimaAccion();

            if (accion != nullptr) {
                cout << "Ultima accion realizada:" << endl;
                accion->mostrar();
            } else {
                cout << "El historial esta vacio." << endl;
            }
        }
        else if (opcion == 3) {
            if (historial.deshacerAccion()) {
                cout << "Se deshizo la ultima accion correctamente." << endl;
            } else {
                cout << "No hay acciones para deshacer." << endl;
            }
        }
        else if (opcion == 4) {
            historial.mostrarHistorial();
        }
        else if (opcion == 5) {
            cout << "Total de acciones registradas: "
                 << historial.contarAcciones() << endl;
        }
        else if (opcion == 6) {
            int cantidad;
            cout << "Cuantas acciones desea deshacer?: ";
            cin >> cantidad;
            historial.deshacerVarias(cantidad);
        }

    } while (opcion != 0);

    return 0;
}
```

---

## 18. Código completo integrado

```cpp
#include <iostream>
#include <string>
using namespace std;

class AccionJuego {
private:
    int id;
    string tipo;
    string descripcion;
    int energiaConsumida;

public:
    AccionJuego(int nuevoId, string nuevoTipo, string nuevaDescripcion, int nuevaEnergia) {
        id = (nuevoId > 0) ? nuevoId : 1;
        tipo = nuevoTipo;
        descripcion = nuevaDescripcion;
        energiaConsumida = (nuevaEnergia >= 0) ? nuevaEnergia : 0;
    }

    int getId() const {
        return id;
    }

    string getTipo() const {
        return tipo;
    }

    string getDescripcion() const {
        return descripcion;
    }

    int getEnergiaConsumida() const {
        return energiaConsumida;
    }

    void mostrar() const {
        cout << "Accion | ID: " << id
             << " | Tipo: " << tipo
             << " | Descripcion: " << descripcion
             << " | Energia: " << energiaConsumida << endl;
    }
};

struct NodoHistorial {
    AccionJuego* accion;
    NodoHistorial* sig;

    NodoHistorial(AccionJuego* nuevaAccion) {
        accion = nuevaAccion;
        sig = nullptr;
    }
};

class HistorialAcciones {
private:
    NodoHistorial* top;

public:
    HistorialAcciones() {
        top = nullptr;
    }

    bool estaVacia() const {
        return top == nullptr;
    }

    void registrarAccion(AccionJuego* nuevaAccion) {
        NodoHistorial* nuevo = new NodoHistorial(nuevaAccion);
        nuevo->sig = top;
        top = nuevo;
    }

    AccionJuego* ultimaAccion() const {
        if (estaVacia()) {
            return nullptr;
        }
        return top->accion;
    }

    bool deshacerAccion() {
        if (estaVacia()) {
            return false;
        }

        NodoHistorial* borrar = top;
        top = top->sig;

        delete borrar->accion;
        delete borrar;

        return true;
    }

    void mostrarHistorial() const {
        if (estaVacia()) {
            cout << "No hay acciones registradas en el historial." << endl;
            return;
        }

        NodoHistorial* actual = top;
        int contador = 1;

        cout << "Historial de acciones del jugador:" << endl;
        cout << "(de la mas reciente a la mas antigua)" << endl;

        while (actual != nullptr) {
            cout << "Accion " << contador << ": ";
            actual->accion->mostrar();
            actual = actual->sig;
            contador++;
        }
    }

    int contarAcciones() const {
        int contador = 0;
        NodoHistorial* actual = top;

        while (actual != nullptr) {
            contador++;
            actual = actual->sig;
        }

        return contador;
    }

    void deshacerVarias(int cantidad) {
        for (int i = 0; i < cantidad; i++) {
            if (!deshacerAccion()) {
                cout << "Ya no hay mas acciones por deshacer." << endl;
                break;
            }
        }
    }

    ~HistorialAcciones() {
        while (!estaVacia()) {
            NodoHistorial* borrar = top;
            top = top->sig;
            delete borrar->accion;
            delete borrar;
        }
    }
};

int main() {
    HistorialAcciones historial;
    int opcion;

    do {
        cout << "\n=====================================" << endl;
        cout << " Mini motor de videojuego - Historial" << endl;
        cout << "=====================================" << endl;
        cout << "1) Registrar accion del jugador" << endl;
        cout << "2) Ver ultima accion realizada" << endl;
        cout << "3) Deshacer ultima accion" << endl;
        cout << "4) Mostrar historial completo" << endl;
        cout << "5) Contar acciones registradas" << endl;
        cout << "6) Deshacer varias acciones" << endl;
        cout << "0) Salir" << endl;
        cout << "Opcion: ";
        cin >> opcion;

        if (opcion == 1) {
            int id, energia;
            string tipo, descripcion;

            cout << "ID de la accion: ";
            cin >> id;
            cin.ignore();
            cout << "Tipo de accion: ";
            getline(cin, tipo);
            cout << "Descripcion de la accion: ";
            cin >> descripcion;
            getline(cin, descripcion);
            cout << "Energia consumida: ";
            cin >> energia;

            historial.registrarAccion(
                new AccionJuego(id, tipo, descripcion, energia)
            );

            cout << "Accion registrada correctamente en el historial." << endl;
        }
        else if (opcion == 2) {
            AccionJuego* accion = historial.ultimaAccion();

            if (accion != nullptr) {
                cout << "Ultima accion realizada:" << endl;
                accion->mostrar();
            } else {
                cout << "El historial esta vacio." << endl;
            }
        }
        else if (opcion == 3) {
            if (historial.deshacerAccion()) {
                cout << "Se deshizo la ultima accion correctamente." << endl;
            } else {
                cout << "No hay acciones para deshacer." << endl;
            }
        }
        else if (opcion == 4) {
            historial.mostrarHistorial();
        }
        else if (opcion == 5) {
            cout << "Total de acciones registradas: "
                 << historial.contarAcciones() << endl;
        }
        else if (opcion == 6) {
            int cantidad;
            cout << "Cuantas acciones desea deshacer?: ";
            cin >> cantidad;
            historial.deshacerVarias(cantidad);
        }

    } while (opcion != 0);

    return 0;
}
```

---

## 19. Ejercicios propuestos

Los ejercicios están diseñados para que el estudiante modifique el programa sin tener que escribirlo desde cero.

### Ejercicio 1

Agregar a la clase `AccionJuego` un nuevo atributo llamado `tiempoEjecucion`, que represente cuántos segundos tomó realizar la acción.

Recomendación: seguir el mismo patrón usado para `energiaConsumida`.

---

### Ejercicio 2

Modificar el menú para que al registrar una acción también se capture `tiempoEjecucion`.

Recomendación: ampliar el constructor y actualizar la opción 1 del menú.

---

### Ejercicio 3

Agregar un método llamado `vaciarHistorial()` que elimine todas las acciones de la pila sin esperar a que termine el programa.

Recomendación: reutilizar la lógica del destructor.

---

### Ejercicio 4

Agregar una opción al menú llamada **Reiniciar nivel**, que vacíe completamente el historial de acciones.

Recomendación: crear una nueva opción que invoque `vaciarHistorial()`.

---

### Ejercicio 5

Modificar `mostrarHistorial()` para que además muestre un encabezado final con el total de acciones listadas.

Recomendación: aprovechar el contador ya existente durante el recorrido.

---

## 20. Actividad de comprobación

El estudiante deberá explicar por escrito:

1. Qué significa el comportamiento LIFO.
2. Por qué una pila es adecuada para modelar un sistema de deshacer acciones.
3. Qué representa la variable `top` dentro del historial.
4. Qué diferencia existe entre `ultimaAccion()` y `deshacerAccion()`.
5. Qué ocurre en memoria cuando se elimina un nodo de la pila.
6. Por qué esta práctica se siente diferente a la lista enlazada, aunque ambas usen nodos.
7. Cómo contribuye este módulo al mini motor de videojuego que se ha ido construyendo en el curso.

---
