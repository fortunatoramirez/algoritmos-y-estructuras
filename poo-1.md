# Práctica 4

## Programación Orientada a Objetos en C++ aplicada a entidades de videojuego

---

## 1. Objetivo de la práctica

Que el estudiante comprenda y aplique los fundamentos de la Programación Orientada a Objetos (POO) en C++, implementando clases y objetos que representen entidades de un videojuego. Al finalizar, el estudiante será capaz de crear clases con atributos y métodos, utilizar constructores, aplicar encapsulamiento mediante modificadores de acceso, y reconocer los cuatro pilares de la POO con ejemplos básicos.

---

## 2. Introducción teórica

En las prácticas anteriores se trabajó con datos en arreglos y con estructuras (`struct`) para representar entidades sencillas, como jugadores con campos `id`, `puntaje` y `nivel`. Sin embargo, al desarrollar software más realista, especialmente videojuegos, no es suficiente guardar datos. Se necesita que las entidades tengan un comportamiento y que el código sea fácil de mantener y extender.

La Programación Orientada a Objetos permite organizar un sistema en torno a “cosas” (entidades) del mundo del problema. En videojuegos, estas cosas suelen ser: jugador, enemigo, ítem, proyectil, evento, misión, etc. Cada una tiene datos (atributos) y comportamientos (métodos).

En POO se manejan principalmente los siguientes conceptos:

### 2.1 Clase y objeto

* Una clase es un molde o plantilla que define qué datos y qué acciones tiene una entidad.
* Un objeto es una instancia real creada a partir de una clase.

Ejemplo conceptual:

* La clase `Jugador` define que un jugador tiene `id`, `puntaje` y `nivel`.
* Un objeto `j1` es un jugador concreto con valores específicos, como `id = 10`, `puntaje = 150`, `nivel = 3`.

### 2.2 Atributos y métodos

* Los atributos son los datos internos del objeto.
* Los métodos son funciones asociadas al objeto y representan acciones.

En un videojuego:

* Atributos: vida, puntaje, nivel, velocidad, posición.
* Métodos: recibir daño, ganar puntos, subir nivel, mover, atacar, mostrar información.

### 2.3 Encapsulamiento

El encapsulamiento consiste en proteger el estado interno del objeto. Esto se logra definiendo atributos como `private` y permitiendo el acceso controlado mediante métodos `public`.

Ventajas del encapsulamiento:

* Evita que el programa modifique valores de forma incorrecta.
* Permite validar datos antes de guardarlos.
* Facilita cambios futuros sin afectar todo el sistema.

Ejemplo: si el nivel no puede ser negativo, entonces el método `setNivel()` puede impedir valores inválidos.

### 2.4 Abstracción

La abstracción consiste en modelar solo lo importante para el problema. Por ejemplo, si el objetivo es simular un ranking, quizá solo se necesita `id`, `puntaje` y `nivel`, sin modelar la posición en pantalla o animaciones.

### 2.5 Herencia

La herencia permite crear una clase nueva a partir de otra, reutilizando atributos y métodos. Se usa cuando existen entidades similares con diferencias específicas.

Ejemplo:

* `Entidad` como clase base con `id` y `nombre`.
* `Jugador` hereda de `Entidad` y agrega `puntaje` y `nivel`.

### 2.6 Polimorfismo

El polimorfismo permite que diferentes clases respondan de forma distinta a un mismo método, generalmente usando métodos `virtual` y `override`.

Ejemplo:

* `mostrar()` existe en `Entidad`.
* `Jugador` y `Enemigo` implementan `mostrar()` de manera distinta.
* Un programa puede llamar `mostrar()` sin saber si es jugador o enemigo, y cada uno se mostrará correctamente.

En esta práctica se trabajará con estos conceptos de forma progresiva y con código guía para que el estudiante solo requiera modificaciones intuitivas.

---

## 3. Material y software requerido

* Visual Studio Code.
* Compilador `g++` instalado.
* Archivo de trabajo: `practica4_poo.cpp`.

Compilación:

```bash
g++ practica4_poo.cpp -o practica4
```

Ejecución:

* Windows:

```bash
practica4
```

* Linux / macOS:

```bash
./practica4
```

---

## 4. Bloque 1. Primera clase y primer objeto

En este bloque se define una clase muy simple llamada `Jugador` y se crea un objeto. Se inicia con atributos públicos para comprender la sintaxis, y después se mejorará con encapsulamiento.

### Código

```cpp
#include <iostream>
using namespace std;

class Jugador {
public:
    int id;
    int puntaje;
    int nivel;
};

int main() {
    Jugador j1;
    j1.id = 10;
    j1.puntaje = 150;
    j1.nivel = 3;

    cout << "Jugador 1" << endl;
    cout << "ID: " << j1.id << endl;
    cout << "Puntaje: " << j1.puntaje << endl;
    cout << "Nivel: " << j1.nivel << endl;

    return 0;
}
```

### Explicación

* La palabra `class` define una clase.
* Dentro de `public` se colocan miembros accesibles desde fuera.
* `Jugador j1;` crea un objeto.
* `j1.id = 10;` asigna valores a sus atributos.

Este enfoque funciona, pero es poco seguro porque permite asignar cualquier valor sin control.

---

## 5. Bloque 2. Encapsulamiento con atributos privados y métodos públicos

En este bloque se convierten los atributos en privados y se agregan métodos para asignar y obtener valores.

### Código

```cpp
#include <iostream>
using namespace std;

class Jugador {
private:
    int id;
    int puntaje;
    int nivel;

public:
    void setId(int nuevoId) {
        if (nuevoId > 0) id = nuevoId;
    }

    void setPuntaje(int nuevoPuntaje) {
        if (nuevoPuntaje >= 0) puntaje = nuevoPuntaje;
    }

    void setNivel(int nuevoNivel) {
        if (nuevoNivel >= 0) nivel = nuevoNivel;
    }

    int getId() { return id; }
    int getPuntaje() { return puntaje; }
    int getNivel() { return nivel; }
};

int main() {
    Jugador j1;

    j1.setId(10);
    j1.setPuntaje(150);
    j1.setNivel(3);

    cout << "Jugador 1" << endl;
    cout << "ID: " << j1.getId() << endl;
    cout << "Puntaje: " << j1.getPuntaje() << endl;
    cout << "Nivel: " << j1.getNivel() << endl;

    return 0;
}
```

### Explicación

* `private` protege los atributos.
* `set...()` asigna valores solo si son válidos.
* `get...()` permite leer valores.

Este es el primer ejemplo real de encapsulamiento.

---

## 6. Bloque 3. Constructores e inicialización correcta

En este bloque se agrega un constructor para asegurar que todo objeto se cree con valores iniciales válidos.

### Código

```cpp
#include <iostream>
using namespace std;

class Jugador {
private:
    int id;
    int puntaje;
    int nivel;

public:
    Jugador() {
        id = 1;
        puntaje = 0;
        nivel = 0;
    }

    Jugador(int nuevoId, int nuevoPuntaje, int nuevoNivel) {
        id = (nuevoId > 0) ? nuevoId : 1;
        puntaje = (nuevoPuntaje >= 0) ? nuevoPuntaje : 0;
        nivel = (nuevoNivel >= 0) ? nuevoNivel : 0;
    }

    int getId() { return id; }
    int getPuntaje() { return puntaje; }
    int getNivel() { return nivel; }

    void mostrar() {
        cout << "ID: " << id
             << " | Puntaje: " << puntaje
             << " | Nivel: " << nivel << endl;
    }
};

int main() {
    Jugador j1;
    Jugador j2(10, 150, 3);

    cout << "Jugador 1 (constructor por defecto)" << endl;
    j1.mostrar();

    cout << "Jugador 2 (constructor con parametros)" << endl;
    j2.mostrar();

    return 0;
}
```

### Explicación

* El constructor por defecto inicializa el objeto con valores seguros.
* El constructor con parámetros permite crear objetos con valores desde el inicio.
* El método `mostrar()` encapsula la impresión de datos.

---

## 7. Bloque 4. Abstracción mediante métodos de acción

En este bloque se agregan métodos que representan acciones del jugador.

### Código

```cpp
#include <iostream>
using namespace std;

class Jugador {
private:
    int id;
    int puntaje;
    int nivel;

public:
    Jugador(int nuevoId) {
        id = (nuevoId > 0) ? nuevoId : 1;
        puntaje = 0;
        nivel = 0;
    }

    void ganarPuntos(int p) {
        if (p > 0) puntaje += p;
    }

    void subirNivel() {
        nivel++;
    }

    void mostrar() {
        cout << "ID: " << id
             << " | Puntaje: " << puntaje
             << " | Nivel: " << nivel << endl;
    }
};

int main() {
    Jugador j1(7);

    j1.ganarPuntos(50);
    j1.subirNivel();
    j1.ganarPuntos(25);

    cout << "Estado final del jugador:" << endl;
    j1.mostrar();

    return 0;
}
```

### Explicación

* `ganarPuntos()` y `subirNivel()` son acciones, no solo manipulación de variables.
* Esta idea es central en POO: los objetos representan comportamiento.

---

## 8. Bloque 5. Herencia básica: Entidad y Jugador

En este bloque se crea una clase base `Entidad` y se deriva `Jugador`.

### Código

```cpp
#include <iostream>
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

    void mostrarBase() {
        cout << "ID: " << id << " | Nombre: " << nombre;
    }
};

class Jugador : public Entidad {
private:
    int puntaje;
    int nivel;

public:
    Jugador(int nuevoId, string nuevoNombre)
        : Entidad(nuevoId, nuevoNombre) {
        puntaje = 0;
        nivel = 0;
    }

    void ganarPuntos(int p) {
        if (p > 0) puntaje += p;
    }

    void subirNivel() {
        nivel++;
    }

    void mostrar() {
        mostrarBase();
        cout << " | Puntaje: " << puntaje
             << " | Nivel: " << nivel << endl;
    }
};

int main() {
    Jugador j1(10, "Alex");

    j1.ganarPuntos(100);
    j1.subirNivel();

    j1.mostrar();

    return 0;
}
```

### Explicación

* `Entidad` contiene atributos comunes.
* `Jugador` hereda y agrega atributos propios.
* `protected` permite acceso a clases hijas sin exponerlo a todo el programa.

---

## 9. Bloque 6. Polimorfismo básico con virtual y override

En este bloque se permite que diferentes clases se muestren de forma distinta usando el mismo método `mostrar()`.

### Código

```cpp
#include <iostream>
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

    virtual void mostrar() {
        cout << "Entidad | ID: " << id << " | Nombre: " << nombre << endl;
    }

    virtual ~Entidad() {}
};

class Jugador : public Entidad {
private:
    int puntaje;

public:
    Jugador(int nuevoId, string nuevoNombre)
        : Entidad(nuevoId, nuevoNombre) {
        puntaje = 0;
    }

    void ganarPuntos(int p) {
        if (p > 0) puntaje += p;
    }

    void mostrar() override {
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

    void mostrar() override {
        cout << "Enemigo | ID: " << id
             << " | Nombre: " << nombre
             << " | Vida: " << vida << endl;
    }
};

int main() {
    Entidad* lista[3];

    Jugador* j1 = new Jugador(1, "Alex");
    j1->ganarPuntos(120);

    Enemigo* e1 = new Enemigo(2, "Slime", 30);
    Enemigo* e2 = new Enemigo(3, "Goblin", 50);

    lista[0] = j1;
    lista[1] = e1;
    lista[2] = e2;

    cout << "Mostrando entidades con polimorfismo:" << endl;
    for (int i = 0; i < 3; i++) {
        lista[i]->mostrar();
    }

    delete j1;
    delete e1;
    delete e2;

    return 0;
}
```

### Explicación

* `virtual` permite que el método correcto se elija según el tipo real del objeto.
* Un arreglo de `Entidad*` puede almacenar diferentes tipos.
* Cada clase define su propia versión de `mostrar()`.

Este bloque introduce la base para guardar objetos en estructuras (colas, listas, pilas) sin perder su comportamiento.

---

## 10. Ejercicios propuestos

En los ejercicios el estudiante no debe escribir el programa desde cero. Solo debe modificar y agregar código en lugares intuitivos.

### Ejercicio 1

En el Bloque 4, agregar un método `perderPuntos(int p)` que reduzca el puntaje sin permitir que sea negativo.

---

### Ejercicio 2

En el Bloque 5, agregar el atributo `energia` al jugador, inicializado en 100, y mostrarlo dentro del método `mostrar()`.

---

### Ejercicio 3

En el Bloque 6, crear una nueva clase `Item` que herede de `Entidad` y tenga el atributo `precio`. Debe sobrescribir `mostrar()` para incluir el precio.

---

### Ejercicio 4

En el Bloque 6, agregar una cuarta entidad al arreglo `lista` y mostrarla con el mismo ciclo.

---

## 11. Actividad de comprobación

El estudiante deberá explicar por escrito:

1. Qué diferencia existe entre clase y objeto.
2. Qué significa encapsulamiento y cuál fue el ejemplo aplicado.
3. Cómo se aplica la abstracción en el diseño de la clase Jugador.
4. Qué es herencia y por qué se creó la clase base Entidad.
5. Qué es polimorfismo y por qué se requiere `virtual`.

