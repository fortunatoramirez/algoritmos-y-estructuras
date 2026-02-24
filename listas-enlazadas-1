# Lista simplemente enlazada de objetos en C++ para manejo de entidades de videojuego

---

## 1. Objetivo de la práctica

Que el estudiante implemente una **lista simplemente enlazada** en C++ para almacenar y administrar **objetos** (entidades de videojuego) utilizando Programación Orientada a Objetos. Al finalizar, el estudiante será capaz de:

* Comprender la motivación de las estructuras dinámicas frente a arreglos de tamaño fijo.
* Implementar un nodo y una lista enlazada simple.
* Insertar elementos al inicio y al final.
* Recorrer e imprimir la lista.
* Buscar un elemento por ID.
* Eliminar un elemento por ID.
* Integrar objetos con herencia y polimorfismo dentro de una estructura dinámica.

---

## 2. Introducción teórica

En videojuegos y sistemas reales, el número de entidades puede cambiar continuamente: aparecen enemigos, se crean proyectiles, se recogen ítems, se eliminan objetos, etc. En este tipo de escenarios, utilizar arreglos de tamaño fijo presenta limitaciones importantes:

* El tamaño debe definirse por adelantado.
* Si se necesita crecer, no es posible sin copiar datos a un arreglo mayor.
* Eliminar elementos en medio requiere desplazar múltiples posiciones.

Las **listas enlazadas** resuelven este problema al almacenar elementos en **nodos** conectados mediante apuntadores. Cada nodo contiene:

* Un dato (en esta práctica, un puntero a un objeto de tipo `Entidad`).
* Un apuntador al siguiente nodo.

En una lista simplemente enlazada:

* Cada nodo conoce únicamente al siguiente.
* El último nodo apunta a `nullptr`.
* La lista se recorre desde el primer nodo (cabeza o `head`) hasta el final.

Esta estructura permite:

* Insertar y eliminar elementos sin mover grandes bloques de memoria.
* Crecer dinámicamente conforme el juego agrega entidades.

---

## 3. Material y software requerido

* Visual Studio Code.
* Compilador `g++`.
* Archivo de trabajo: `practica5_lista_objetos.cpp`.

Compilación:

```bash
g++ practica5_lista_objetos.cpp -o practica5
```

Ejecución:

* Windows:

```bash
practica5
```

* Linux / macOS:

```bash
./practica5
```

---

## 4. Diseño general del sistema

Se utilizarán clases con herencia y polimorfismo para representar entidades.

* Clase base: `Entidad`
* Clases derivadas:

  * `Jugador`
  * `Enemigo`
  * `Item`

La lista no almacenará objetos “copiados” sino **apuntadores** a objetos dinámicos, de forma que el polimorfismo funcione correctamente.

---

## 5. Bloque 1. Clases de entidades para el videojuego

Este bloque define las clases que se guardarán dentro de la lista. Se utiliza un método virtual `mostrar()` para polimorfismo.

### Código

```cpp
#include <iostream>
#include <string>
using namespace std;

class Entidad {
protected:
    int id;
    string nombre;

public:
    Entidad(int nuevoId, string nuevoNombre) {
        id = (nuevoId > 0) ? nuevoId : 1;
        nombre = nuevoNombre;
    }

    int getId() const { return id; }

    virtual void mostrar() const {
        cout << "Entidad | ID: " << id << " | Nombre: " << nombre << endl;
    }

    virtual ~Entidad() {}
};

class Jugador : public Entidad {
private:
    int puntaje;

public:
    Jugador(int nuevoId, string nuevoNombre, int p)
        : Entidad(nuevoId, nuevoNombre) {
        puntaje = (p >= 0) ? p : 0;
    }

    void mostrar() const override {
        cout << "Jugador | ID: " << id
             << " | Nombre: " << nombre
             << " | Puntaje: " << puntaje << endl;
    }
};

class Enemigo : public Entidad {
private:
    int vida;

public:
    Enemigo(int nuevoId, string nuevoNombre, int v)
        : Entidad(nuevoId, nuevoNombre) {
        vida = (v >= 0) ? v : 0;
    }

    void mostrar() const override {
        cout << "Enemigo | ID: " << id
             << " | Nombre: " << nombre
             << " | Vida: " << vida << endl;
    }
};

class Item : public Entidad {
private:
    int precio;

public:
    Item(int nuevoId, string nuevoNombre, int pr)
        : Entidad(nuevoId, nuevoNombre) {
        precio = (pr >= 0) ? pr : 0;
    }

    void mostrar() const override {
        cout << "Item | ID: " << id
             << " | Nombre: " << nombre
             << " | Precio: " << precio << endl;
    }
};
```

### Explicación

* `Entidad` es base; contiene `id` y `nombre`.
* `mostrar()` es virtual para permitir polimorfismo.
* `Jugador`, `Enemigo` e `Item` sobrescriben `mostrar()`.

---

## 6. Bloque 2. Nodo de la lista enlazada

Cada nodo guardará un apuntador a `Entidad` y un apuntador al siguiente nodo.

### Código

```cpp
struct Nodo {
    Entidad* dato;
    Nodo* sig;

    Nodo(Entidad* e) {
        dato = e;
        sig = nullptr;
    }
};
```

### Explicación

* `dato` apunta a un objeto de tipo `Entidad` o derivado.
* `sig` enlaza con el siguiente nodo.
* Se inicializa `sig` con `nullptr`.

---

## 7. Bloque 3. Clase ListaEnlazada con inserción al inicio

Se crea una clase `ListaEnlazada` con un apuntador `head`.

### Código

```cpp
class ListaEnlazada {
private:
    Nodo* head;

public:
    ListaEnlazada() {
        head = nullptr;
    }

    void insertarInicio(Entidad* e) {
        Nodo* nuevo = new Nodo(e);
        nuevo->sig = head;
        head = nuevo;
    }

    void mostrarTodo() const {
        Nodo* actual = head;
        while (actual != nullptr) {
            actual->dato->mostrar();
            actual = actual->sig;
        }
    }

    ~ListaEnlazada() {
        Nodo* actual = head;
        while (actual != nullptr) {
            Nodo* siguiente = actual->sig;
            delete actual->dato;
            delete actual;
            actual = siguiente;
        }
    }
};
```

### Explicación

* `insertarInicio` crea un nodo nuevo y lo coloca al inicio.
* `mostrarTodo` recorre la lista e imprime cada entidad.
* El destructor libera memoria, eliminando nodos y objetos.

---

## 8. Bloque 4. Inserción al final

Se agrega el método `insertarFinal`.

### Código para agregar dentro de `ListaEnlazada`

```cpp
void insertarFinal(Entidad* e) {
    Nodo* nuevo = new Nodo(e);

    if (head == nullptr) {
        head = nuevo;
        return;
    }

    Nodo* actual = head;
    while (actual->sig != nullptr) {
        actual = actual->sig;
    }
    actual->sig = nuevo;
}
```

### Explicación

* Si la lista está vacía, `head` apunta al nuevo nodo.
* Si no, se recorre hasta el último nodo y se enlaza el nuevo.

---

## 9. Bloque 5. Búsqueda por ID

Se agrega un método que busca una entidad por ID. Se retorna un apuntador a la entidad, o `nullptr` si no existe.

### Código para agregar dentro de `ListaEnlazada`

```cpp
Entidad* buscarPorId(int idBuscado) const {
    Nodo* actual = head;
    while (actual != nullptr) {
        if (actual->dato->getId() == idBuscado) {
            return actual->dato;
        }
        actual = actual->sig;
    }
    return nullptr;
}
```

---

## 10. Bloque 6. Eliminación por ID

Se implementa un método que elimina un nodo por ID y libera memoria.

### Código para agregar dentro de `ListaEnlazada`

```cpp
bool eliminarPorId(int idBuscado) {
    if (head == nullptr) return false;

    if (head->dato->getId() == idBuscado) {
        Nodo* borrar = head;
        head = head->sig;
        delete borrar->dato;
        delete borrar;
        return true;
    }

    Nodo* actual = head;
    while (actual->sig != nullptr) {
        if (actual->sig->dato->getId() == idBuscado) {
            Nodo* borrar = actual->sig;
            actual->sig = borrar->sig;
            delete borrar->dato;
            delete borrar;
            return true;
        }
        actual = actual->sig;
    }
    return false;
}
```

### Explicación

* Se maneja el caso especial cuando se elimina el primer nodo.
* Para otros casos, se busca el nodo anterior para reajustar enlaces.

---

## 11. Bloque 7. Programa principal con menú simple

En este bloque se utiliza un menú para probar inserciones, búsqueda, impresión y eliminación.

### Código completo del `main`

```cpp
int main() {
    ListaEnlazada lista;
    int opcion;

    do {
        cout << "\nMenu" << endl;
        cout << "1) Insertar Jugador al inicio" << endl;
        cout << "2) Insertar Enemigo al final" << endl;
        cout << "3) Insertar Item al final" << endl;
        cout << "4) Mostrar todas las entidades" << endl;
        cout << "5) Buscar entidad por ID" << endl;
        cout << "6) Eliminar entidad por ID" << endl;
        cout << "0) Salir" << endl;
        cout << "Opcion: ";
        cin >> opcion;

        if (opcion == 1) {
            int id, p;
            string nombre;
            cout << "ID: "; cin >> id;
            cout << "Nombre: "; cin >> nombre;
            cout << "Puntaje: "; cin >> p;
            lista.insertarInicio(new Jugador(id, nombre, p));
        }
        else if (opcion == 2) {
            int id, v;
            string nombre;
            cout << "ID: "; cin >> id;
            cout << "Nombre: "; cin >> nombre;
            cout << "Vida: "; cin >> v;
            lista.insertarFinal(new Enemigo(id, nombre, v));
        }
        else if (opcion == 3) {
            int id, pr;
            string nombre;
            cout << "ID: "; cin >> id;
            cout << "Nombre: "; cin >> nombre;
            cout << "Precio: "; cin >> pr;
            lista.insertarFinal(new Item(id, nombre, pr));
        }
        else if (opcion == 4) {
            cout << "\nEntidades en lista:" << endl;
            lista.mostrarTodo();
        }
        else if (opcion == 5) {
            int idBuscado;
            cout << "ID a buscar: ";
            cin >> idBuscado;

            Entidad* e = lista.buscarPorId(idBuscado);
            if (e != nullptr) {
                cout << "Entidad encontrada:" << endl;
                e->mostrar();
            } else {
                cout << "No encontrada." << endl;
            }
        }
        else if (opcion == 6) {
            int idBorrar;
            cout << "ID a eliminar: ";
            cin >> idBorrar;

            if (lista.eliminarPorId(idBorrar)) {
                cout << "Eliminada correctamente." << endl;
            } else {
                cout << "No se encontro el ID." << endl;
            }
        }

    } while (opcion != 0);

    return 0;
}
```

---

## 12. Ejercicios propuestos

Los ejercicios están diseñados para que el estudiante realice cambios intuitivos sin escribir todo desde cero.

### Ejercicio 1

Agregar una nueva clase derivada `Proyectil` que herede de `Entidad` y tenga el atributo `danio`. Debe sobrescribir `mostrar()`.

Recomendación: copiar la clase `Item` y modificar campos y salida.

---

### Ejercicio 2

Agregar una opción al menú para insertar `Proyectil` al inicio de la lista.

Recomendación: copiar el bloque de inserción de `Jugador` y modificar.

---

### Ejercicio 3

Modificar el método `mostrarTodo()` para que además muestre un contador:

* “Entidad 1”, “Entidad 2”, etc.

Recomendación: usar una variable contador dentro del recorrido.

---

### Ejercicio 4

Agregar un método `contar()` que regrese el número total de nodos de la lista y mostrarlo en el menú.

Recomendación: recorrer la lista y contar nodos.
