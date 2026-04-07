# Práctica: Juego en línea por consola con Programación Orientada a Objetos, biblioteca personal y estructuras de datos

## Objetivo general

Que el estudiante desarrolle un juego en línea por consola entre dos computadoras, utilizando Programación Orientada a Objetos, una biblioteca personal para la comunicación en red en Windows, y estructuras de datos como pila y lista enlazada, integrando todos estos elementos en un proyecto modular organizado en múltiples archivos.

## Objetivos específicos

Al finalizar la práctica, el estudiante será capaz de:

* organizar un proyecto en carpetas y archivos `.h` y `.cpp`
* construir una biblioteca personal para comunicación básica usando Winsock
* modelar un juego por turnos mediante clases y objetos
* implementar una pila para almacenar historial de acciones
* implementar una lista enlazada para administrar un inventario dinámico
* aplicar búsqueda secuencial dentro de una estructura dinámica
* integrar la comunicación en red con la lógica del juego
* compilar y ejecutar el sistema completo en dos computadoras distintas

---

# Introducción teórica

En el desarrollo de software real, un programa no se construye como un solo bloque monolítico, sino como varios módulos con responsabilidades específicas. Esta organización facilita el mantenimiento, la comprensión del código, la reutilización de componentes y el trabajo colaborativo entre varias personas.

En esta práctica se desarrollará un juego en línea por consola entre dos jugadores. Aunque el juego será sencillo, permitirá integrar varios conceptos fundamentales del curso:

* **Programación Orientada a Objetos**, para modelar jugadores, acciones y la partida.
* **Estructuras dinámicas**, para administrar el historial y el inventario.
* **Algoritmos básicos**, como búsqueda secuencial.
* **Comunicación en red**, para intercambiar acciones entre dos computadoras.
* **Modularidad**, separando el proyecto en bibliotecas y clases.

La comunicación se realizará en Windows usando **Winsock**, que es la interfaz de programación de sockets del sistema operativo. Sin embargo, el estudiante no trabajará directamente con toda la complejidad de Winsock dentro del programa principal, sino mediante una **biblioteca personal** creada durante la práctica. Esa biblioteca encapsulará las operaciones básicas de conexión, envío, recepción y cierre.

El juego propuesto será un **duelo táctico por turnos**. Cada jugador contará con atributos como vida, energía, defensa e inventario. Durante la partida, cada jugador podrá atacar, defenderse, cargar energía o usar objetos. Cada acción se enviará como texto por red hacia la computadora del rival.

Esta práctica servirá como base para futuras versiones con interfaz gráfica, audio, varios jugadores o mecánicas más avanzadas.

---

# Descripción general del juego

Se desarrollará un juego en línea por consola entre dos jugadores conectados dentro de la misma red.

Cada jugador tendrá:

* nombre
* vida
* energía
* estado de defensa
* inventario dinámico
* historial de acciones

Las acciones disponibles serán:

* **Atacar**: causa daño al rival
* **Defender**: reduce el daño del siguiente ataque recibido
* **Cargar energía**: incrementa la energía disponible
* **Usar poción**: recupera vida
* **Usar batería**: recupera energía
* **Ver historial**
* **Ver inventario**

La comunicación será de tipo **cliente-servidor**:

* una computadora actuará como **servidor**
* la otra como **cliente**

La partida será estrictamente **por turnos**, para que el flujo sea claro y no se requieran hilos ni sincronización compleja.

---

# Reglas de la primera versión

Cada jugador iniciará con:

* vida: `100`
* energía: `0`
* defensa: desactivada
* inventario inicial:

  * `Pocion` con valor `20`
  * `Pocion` con valor `20`
  * `Bateria` con valor `10`

## Acciones

### 1. Atacar

* daño base: `15`

### 2. Defender

* reduce a la mitad el próximo daño recibido

### 3. Cargar energía

* incrementa la energía en `10`

### 4. Usar Poción

* recupera `20` puntos de vida

### 5. Usar Batería

* recupera `10` puntos de energía

## Final de la partida

La partida termina cuando la vida de uno de los jugadores llega a `0`.

---

# Estructura general del proyecto

La carpeta del proyecto deberá organizarse de la siguiente manera:

```text
JuegoEnLinea/
│
├── main.cpp
│
├── juego/
│   ├── Jugador.h
│   ├── Jugador.cpp
│   ├── Accion.h
│   ├── Accion.cpp
│   ├── Partida.h
│   ├── Partida.cpp
│
├── estructuras/
│   ├── NodoInventario.h
│   ├── ListaInventario.h
│   ├── ListaInventario.cpp
│   ├── NodoPila.h
│   ├── PilaHistorial.h
│   ├── PilaHistorial.cpp
│
├── red/
│   ├── ConexionJuego.h
│   ├── ConexionJuego.cpp
│
└── README.txt
```

---

# Bloque 1. Estructura base del proyecto

## Propósito del bloque

Organizar el proyecto desde el inicio, separando la lógica en carpetas y archivos independientes. Este bloque permite que el alumno se acostumbre a una estructura profesional de trabajo y evite programas construidos en un solo archivo.

## Paso 1. Crear la carpeta principal

Crear una carpeta llamada:

```text
JuegoEnLinea
```

Dentro de ella crear las subcarpetas:

* `juego`
* `estructuras`
* `red`

## Paso 2. Crear los archivos del proyecto

### En la carpeta principal

* `main.cpp`

### En la carpeta `juego`

* `Jugador.h`
* `Jugador.cpp`
* `Accion.h`
* `Accion.cpp`
* `Partida.h`
* `Partida.cpp`

### En la carpeta `estructuras`

* `NodoInventario.h`
* `ListaInventario.h`
* `ListaInventario.cpp`
* `NodoPila.h`
* `PilaHistorial.h`
* `PilaHistorial.cpp`

### En la carpeta `red`

* `ConexionJuego.h`
* `ConexionJuego.cpp`

---

## Paso 3. Primera compilación del proyecto

Se recomienda comenzar con una versión simple para comprobar que la estructura y el proceso de compilación funcionan correctamente.

### Archivo `juego/Jugador.h`

```cpp
#ifndef JUGADOR_H
#define JUGADOR_H

#include <string>

class Jugador {
private:
    std::string nombre;
    int vida;
    int energia;
    bool defendiendo;

public:
    Jugador();
    Jugador(std::string nombre);

    std::string getNombre() const;
    int getVida() const;
    int getEnergia() const;
    bool estaDefendiendo() const;

    void setDefensa(bool estado);
    void recibirDanio(int cantidad);
    void curar(int cantidad);
    void cargarEnergia(int cantidad);
    bool gastarEnergia(int cantidad);

    bool estaVivo() const;
    void mostrarEstado() const;
};

#endif
```

### Archivo `juego/Jugador.cpp`

```cpp
#include <iostream>
#include "Jugador.h"

Jugador::Jugador() {
    nombre = "SinNombre";
    vida = 100;
    energia = 0;
    defendiendo = false;
}

Jugador::Jugador(std::string nombre) {
    this->nombre = nombre;
    vida = 100;
    energia = 0;
    defendiendo = false;
}

std::string Jugador::getNombre() const {
    return nombre;
}

int Jugador::getVida() const {
    return vida;
}

int Jugador::getEnergia() const {
    return energia;
}

bool Jugador::estaDefendiendo() const {
    return defendiendo;
}

void Jugador::setDefensa(bool estado) {
    defendiendo = estado;
}

void Jugador::recibirDanio(int cantidad) {
    if (defendiendo) {
        cantidad = cantidad / 2;
        defendiendo = false;
    }

    vida -= cantidad;

    if (vida < 0) {
        vida = 0;
    }
}

void Jugador::curar(int cantidad) {
    vida += cantidad;

    if (vida > 100) {
        vida = 100;
    }
}

void Jugador::cargarEnergia(int cantidad) {
    energia += cantidad;
}

bool Jugador::gastarEnergia(int cantidad) {
    if (energia >= cantidad) {
        energia -= cantidad;
        return true;
    }
    return false;
}

bool Jugador::estaVivo() const {
    return vida > 0;
}

void Jugador::mostrarEstado() const {
    std::cout << "Jugador: " << nombre << std::endl;
    std::cout << "Vida: " << vida << std::endl;
    std::cout << "Energia: " << energia << std::endl;
    std::cout << "Defendiendo: " << (defendiendo ? "Si" : "No") << std::endl;
}
```

### Archivo `main.cpp`

```cpp
#include <iostream>
#include "juego/Jugador.h"

int main() {
    Jugador jugador1("JugadorPrueba");
    jugador1.mostrarEstado();
    return 0;
}
```

## Compilación

```bash
g++ main.cpp juego/Jugador.cpp -o prueba.exe
```

## Resultado esperado

El programa deberá compilar y mostrar en pantalla el estado inicial de un jugador.

---

# Bloque 2. Biblioteca de conexión

## Propósito del bloque

Construir una biblioteca personal para manejar la comunicación en red usando Winsock. Esta biblioteca se utilizará después dentro del juego, de modo que la lógica principal no dependa directamente de funciones de socket distribuidas en todo el programa.

## Introducción breve

Winsock es la interfaz de Windows para programación de sockets. En esta práctica se usará una comunicación TCP sencilla entre dos computadoras.

La biblioteca creada permitirá:

* inicializar Winsock
* crear socket
* iniciar servidor
* aceptar cliente
* conectar cliente a servidor
* enviar texto
* recibir texto
* cerrar conexión

---

## Archivo `red/ConexionJuego.h`

```cpp
#ifndef CONEXIONJUEGO_H
#define CONEXIONJUEGO_H

#include <string>
#include <winsock2.h>

class ConexionJuego {
private:
    WSADATA wsaData;
    SOCKET socketEscucha;
    SOCKET socketConexion;
    bool modoServidor;
    bool winsockInicializado;

public:
    ConexionJuego();
    ~ConexionJuego();

    bool inicializar();
    bool iniciarServidor(int puerto);
    bool esperarCliente();
    bool conectarAServidor(const std::string& ip, int puerto);

    bool enviar(const std::string& mensaje);
    std::string recibir();

    void cerrar();
};

#endif
```

## Archivo `red/ConexionJuego.cpp`

```cpp
#include "ConexionJuego.h"
#include <iostream>
#include <string>
#include <winsock2.h>
#include <ws2tcpip.h>

ConexionJuego::ConexionJuego() {
    socketEscucha = INVALID_SOCKET;
    socketConexion = INVALID_SOCKET;
    modoServidor = false;
    winsockInicializado = false;
}

ConexionJuego::~ConexionJuego() {
    cerrar();
}

bool ConexionJuego::inicializar() {
    if (WSAStartup(MAKEWORD(2, 2), &wsaData) != 0) {
        std::cout << "Error al inicializar Winsock." << std::endl;
        return false;
    }

    winsockInicializado = true;
    return true;
}

bool ConexionJuego::iniciarServidor(int puerto) {
    modoServidor = true;

    socketEscucha = socket(AF_INET, SOCK_STREAM, 0);
    if (socketEscucha == INVALID_SOCKET) {
        std::cout << "Error al crear socket de escucha." << std::endl;
        return false;
    }

    sockaddr_in servidor;
    servidor.sin_family = AF_INET;
    servidor.sin_addr.s_addr = INADDR_ANY;
    servidor.sin_port = htons(puerto);

    if (bind(socketEscucha, (sockaddr*)&servidor, sizeof(servidor)) == SOCKET_ERROR) {
        std::cout << "Error en bind." << std::endl;
        return false;
    }

    if (listen(socketEscucha, 1) == SOCKET_ERROR) {
        std::cout << "Error en listen." << std::endl;
        return false;
    }

    return true;
}

bool ConexionJuego::esperarCliente() {
    if (!modoServidor) {
        return false;
    }

    std::cout << "Esperando cliente..." << std::endl;

    socketConexion = accept(socketEscucha, nullptr, nullptr);
    if (socketConexion == INVALID_SOCKET) {
        std::cout << "Error al aceptar cliente." << std::endl;
        return false;
    }

    std::cout << "Cliente conectado correctamente." << std::endl;
    return true;
}

bool ConexionJuego::conectarAServidor(const std::string& ip, int puerto) {
    modoServidor = false;

    socketConexion = socket(AF_INET, SOCK_STREAM, 0);
    if (socketConexion == INVALID_SOCKET) {
        std::cout << "Error al crear socket de cliente." << std::endl;
        return false;
    }

    sockaddr_in servidor;
    servidor.sin_family = AF_INET;
    servidor.sin_port = htons(puerto);
    servidor.sin_addr.s_addr = inet_addr(ip.c_str());

    if (connect(socketConexion, (sockaddr*)&servidor, sizeof(servidor)) == SOCKET_ERROR) {
        std::cout << "No fue posible conectar al servidor." << std::endl;
        return false;
    }

    std::cout << "Conexion realizada correctamente." << std::endl;
    return true;
}

bool ConexionJuego::enviar(const std::string& mensaje) {
    if (socketConexion == INVALID_SOCKET) {
        return false;
    }

    int enviados = send(socketConexion, mensaje.c_str(), (int)mensaje.length(), 0);
    if (enviados == SOCKET_ERROR) {
        std::cout << "Error al enviar mensaje." << std::endl;
        return false;
    }

    return true;
}

std::string ConexionJuego::recibir() {
    if (socketConexion == INVALID_SOCKET) {
        return "";
    }

    char buffer[512];
    int bytesRecibidos = recv(socketConexion, buffer, sizeof(buffer) - 1, 0);

    if (bytesRecibidos <= 0) {
        return "";
    }

    buffer[bytesRecibidos] = '\0';
    return std::string(buffer);
}

void ConexionJuego::cerrar() {
    if (socketConexion != INVALID_SOCKET) {
        closesocket(socketConexion);
        socketConexion = INVALID_SOCKET;
    }

    if (socketEscucha != INVALID_SOCKET) {
        closesocket(socketEscucha);
        socketEscucha = INVALID_SOCKET;
    }

    if (winsockInicializado) {
        WSACleanup();
        winsockInicializado = false;
    }
}
```

---

## Prueba rápida de conexión

### Versión servidor en `main.cpp`

```cpp
#include <iostream>
#include "red/ConexionJuego.h"

int main() {
    ConexionJuego conexion;

    if (!conexion.inicializar()) return 1;
    if (!conexion.iniciarServidor(5000)) return 1;
    if (!conexion.esperarCliente()) return 1;

    std::string mensaje = conexion.recibir();
    std::cout << "Mensaje recibido: " << mensaje << std::endl;

    conexion.enviar("Hola cliente, mensaje recibido.");
    conexion.cerrar();

    return 0;
}
```

### Versión cliente en `main.cpp`

```cpp
#include <iostream>
#include "red/ConexionJuego.h"

int main() {
    ConexionJuego conexion;

    if (!conexion.inicializar()) return 1;
    if (!conexion.conectarAServidor("192.168.1.10", 5000)) return 1;

    conexion.enviar("Hola servidor.");
    std::string respuesta = conexion.recibir();

    std::cout << "Respuesta del servidor: " << respuesta << std::endl;

    conexion.cerrar();
    return 0;
}
```

Se debe cambiar la IP por la real del servidor.

## Compilación

```bash
g++ main.cpp red/ConexionJuego.cpp -o redtest.exe -lws2_32
```

## Resultado esperado

Dos computadoras deben poder conectarse y enviarse mensajes de texto.

---

# Bloque 3. Modelo orientado a objetos del juego

## Propósito del bloque

Definir las clases principales del sistema y separar sus responsabilidades.

---

## Clase `Accion`

### Archivo `juego/Accion.h`

```cpp
#ifndef ACCION_H
#define ACCION_H

#include <string>

class Accion {
private:
    std::string tipo;
    int valor;
    std::string descripcion;

public:
    Accion();
    Accion(std::string tipo, int valor, std::string descripcion);

    std::string getTipo() const;
    int getValor() const;
    std::string getDescripcion() const;
};

#endif
```

### Archivo `juego/Accion.cpp`

```cpp
#include "Accion.h"

Accion::Accion() {
    tipo = "NINGUNA";
    valor = 0;
    descripcion = "";
}

Accion::Accion(std::string tipo, int valor, std::string descripcion) {
    this->tipo = tipo;
    this->valor = valor;
    this->descripcion = descripcion;
}

std::string Accion::getTipo() const {
    return tipo;
}

int Accion::getValor() const {
    return valor;
}

std::string Accion::getDescripcion() const {
    return descripcion;
}
```

---

## Clase `Partida`

### Archivo `juego/Partida.h`

```cpp
#ifndef PARTIDA_H
#define PARTIDA_H

#include "Jugador.h"

class Partida {
private:
    Jugador jugadorLocal;
    Jugador jugadorRemoto;
    bool turnoLocal;

public:
    Partida();
    Partida(Jugador local, Jugador remoto, bool turnoInicialLocal);

    Jugador& getJugadorLocal();
    Jugador& getJugadorRemoto();

    bool esTurnoLocal() const;
    void cambiarTurno();

    bool partidaTerminada() const;
    void mostrarEstadoGeneral() const;
};

#endif
```

### Archivo `juego/Partida.cpp`

```cpp
#include <iostream>
#include "Partida.h"

Partida::Partida() {
    turnoLocal = true;
}

Partida::Partida(Jugador local, Jugador remoto, bool turnoInicialLocal) {
    jugadorLocal = local;
    jugadorRemoto = remoto;
    turnoLocal = turnoInicialLocal;
}

Jugador& Partida::getJugadorLocal() {
    return jugadorLocal;
}

Jugador& Partida::getJugadorRemoto() {
    return jugadorRemoto;
}

bool Partida::esTurnoLocal() const {
    return turnoLocal;
}

void Partida::cambiarTurno() {
    turnoLocal = !turnoLocal;
}

bool Partida::partidaTerminada() const {
    return !jugadorLocal.estaVivo() || !jugadorRemoto.estaVivo();
}

void Partida::mostrarEstadoGeneral() const {
    std::cout << "\n===== ESTADO DE LA PARTIDA =====" << std::endl;
    std::cout << "\n--- JUGADOR LOCAL ---" << std::endl;
    jugadorLocal.mostrarEstado();

    std::cout << "\n--- JUGADOR REMOTO ---" << std::endl;
    jugadorRemoto.mostrarEstado();

    std::cout << "\nTurno actual: " << (turnoLocal ? "Jugador local" : "Jugador remoto") << std::endl;
}
```

---

# Bloque 4. Estructuras de datos

## Propósito del bloque

Integrar estructuras dinámicas reales dentro del juego para que el proyecto no sea solamente un intercambio de mensajes, sino una práctica completa de algoritmos y estructuras.

---

## Parte A. Pila para historial de acciones

### Archivo `estructuras/NodoPila.h`

```cpp
#ifndef NODOPILA_H
#define NODOPILA_H

#include <string>

struct NodoPila {
    std::string dato;
    NodoPila* siguiente;
};

#endif
```

### Archivo `estructuras/PilaHistorial.h`

```cpp
#ifndef PILAHISTORIAL_H
#define PILAHISTORIAL_H

#include <string>
#include "NodoPila.h"

class PilaHistorial {
private:
    NodoPila* tope;

public:
    PilaHistorial();
    ~PilaHistorial();

    void push(const std::string& accion);
    void pop();
    std::string peek() const;
    bool estaVacia() const;
    void mostrar() const;
};

#endif
```

### Archivo `estructuras/PilaHistorial.cpp`

```cpp
#include <iostream>
#include "PilaHistorial.h"

PilaHistorial::PilaHistorial() {
    tope = nullptr;
}

PilaHistorial::~PilaHistorial() {
    while (!estaVacia()) {
        pop();
    }
}

void PilaHistorial::push(const std::string& accion) {
    NodoPila* nuevo = new NodoPila;
    nuevo->dato = accion;
    nuevo->siguiente = tope;
    tope = nuevo;
}

void PilaHistorial::pop() {
    if (tope != nullptr) {
        NodoPila* temp = tope;
        tope = tope->siguiente;
        delete temp;
    }
}

std::string PilaHistorial::peek() const {
    if (tope != nullptr) {
        return tope->dato;
    }
    return "Pila vacia";
}

bool PilaHistorial::estaVacia() const {
    return tope == nullptr;
}

void PilaHistorial::mostrar() const {
    NodoPila* actual = tope;
    std::cout << "\nHistorial de acciones:" << std::endl;

    while (actual != nullptr) {
        std::cout << "- " << actual->dato << std::endl;
        actual = actual->siguiente;
    }
}
```

---

## Parte B. Lista enlazada para inventario

### Archivo `estructuras/NodoInventario.h`

```cpp
#ifndef NODOINVENTARIO_H
#define NODOINVENTARIO_H

#include <string>

struct NodoInventario {
    std::string nombre;
    int valor;
    NodoInventario* siguiente;
};

#endif
```

### Archivo `estructuras/ListaInventario.h`

```cpp
#ifndef LISTAINVENTARIO_H
#define LISTAINVENTARIO_H

#include <string>
#include "NodoInventario.h"

class ListaInventario {
private:
    NodoInventario* cabeza;

public:
    ListaInventario();
    ~ListaInventario();

    void agregarObjeto(const std::string& nombre, int valor);
    void mostrarInventario() const;
    bool buscarObjeto(const std::string& nombre) const;
    bool usarObjeto(const std::string& nombre, int& valorRecuperado);
};

#endif
```

### Archivo `estructuras/ListaInventario.cpp`

```cpp
#include <iostream>
#include "ListaInventario.h"

ListaInventario::ListaInventario() {
    cabeza = nullptr;
}

ListaInventario::~ListaInventario() {
    NodoInventario* actual = cabeza;
    while (actual != nullptr) {
        NodoInventario* temp = actual;
        actual = actual->siguiente;
        delete temp;
    }
}

void ListaInventario::agregarObjeto(const std::string& nombre, int valor) {
    NodoInventario* nuevo = new NodoInventario;
    nuevo->nombre = nombre;
    nuevo->valor = valor;
    nuevo->siguiente = nullptr;

    if (cabeza == nullptr) {
        cabeza = nuevo;
    } else {
        NodoInventario* actual = cabeza;
        while (actual->siguiente != nullptr) {
            actual = actual->siguiente;
        }
        actual->siguiente = nuevo;
    }
}

void ListaInventario::mostrarInventario() const {
    NodoInventario* actual = cabeza;
    std::cout << "\nInventario:" << std::endl;

    while (actual != nullptr) {
        std::cout << "- " << actual->nombre << " (valor: " << actual->valor << ")" << std::endl;
        actual = actual->siguiente;
    }
}

bool ListaInventario::buscarObjeto(const std::string& nombre) const {
    NodoInventario* actual = cabeza;

    while (actual != nullptr) {
        if (actual->nombre == nombre) {
            return true;
        }
        actual = actual->siguiente;
    }

    return false;
}

bool ListaInventario::usarObjeto(const std::string& nombre, int& valorRecuperado) {
    NodoInventario* actual = cabeza;
    NodoInventario* anterior = nullptr;

    while (actual != nullptr) {
        if (actual->nombre == nombre) {
            valorRecuperado = actual->valor;

            if (anterior == nullptr) {
                cabeza = actual->siguiente;
            } else {
                anterior->siguiente = actual->siguiente;
            }

            delete actual;
            return true;
        }

        anterior = actual;
        actual = actual->siguiente;
    }

    return false;
}
```

---

# Bloque 5. Integración del juego en línea

## Propósito del bloque

Unir la lógica del juego, la comunicación en red y las estructuras de datos dentro de una sola aplicación.

Para esta primera versión se recomienda que la lógica principal esté en `main.cpp`, mientras las clases y estructuras quedan en sus archivos propios.

---

## Formato de mensajes de red

Las acciones viajarán como texto plano.

Ejemplos:

```text
ATACAR|15
DEFENDER|0
CARGAR|10
USAR|Pocion
USAR|Bateria
```

---

## Funciones auxiliares recomendadas para `main.cpp`

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include "juego/Jugador.h"
#include "juego/Accion.h"
#include "juego/Partida.h"
#include "estructuras/PilaHistorial.h"
#include "estructuras/ListaInventario.h"
#include "red/ConexionJuego.h"

void mostrarMenu() {
    std::cout << "\n===== MENU DEL JUGADOR =====" << std::endl;
    std::cout << "1. Atacar" << std::endl;
    std::cout << "2. Defender" << std::endl;
    std::cout << "3. Cargar energia" << std::endl;
    std::cout << "4. Usar Pocion" << std::endl;
    std::cout << "5. Usar Bateria" << std::endl;
    std::cout << "6. Ver historial" << std::endl;
    std::cout << "7. Ver inventario" << std::endl;
    std::cout << "Seleccione una opcion: ";
}

void aplicarMensajeRecibido(const std::string& mensaje, Jugador& jugadorLocal, PilaHistorial& historial) {
    std::stringstream ss(mensaje);
    std::string tipo, dato;

    getline(ss, tipo, '|');
    getline(ss, dato, '|');

    if (tipo == "ATACAR") {
        int danio = std::stoi(dato);
        jugadorLocal.recibirDanio(danio);
        historial.push("El rival ataco con " + std::to_string(danio));
    }
    else if (tipo == "DEFENDER") {
        historial.push("El rival se defendio");
    }
    else if (tipo == "CARGAR") {
        historial.push("El rival cargo energia");
    }
    else if (tipo == "USAR") {
        historial.push("El rival uso " + dato);
    }
}
```

---

## Programa principal completo sugerido para la primera versión

### Archivo `main.cpp`

```cpp
#include <iostream>
#include <string>
#include <sstream>
#include "juego/Jugador.h"
#include "juego/Partida.h"
#include "estructuras/PilaHistorial.h"
#include "estructuras/ListaInventario.h"
#include "red/ConexionJuego.h"

void mostrarMenu() {
    std::cout << "\n===== MENU DEL JUGADOR =====" << std::endl;
    std::cout << "1. Atacar" << std::endl;
    std::cout << "2. Defender" << std::endl;
    std::cout << "3. Cargar energia" << std::endl;
    std::cout << "4. Usar Pocion" << std::endl;
    std::cout << "5. Usar Bateria" << std::endl;
    std::cout << "6. Ver historial" << std::endl;
    std::cout << "7. Ver inventario" << std::endl;
    std::cout << "Seleccione una opcion: ";
}

void aplicarMensajeRecibido(const std::string& mensaje, Jugador& jugadorLocal, PilaHistorial& historial) {
    std::stringstream ss(mensaje);
    std::string tipo, dato;

    getline(ss, tipo, '|');
    getline(ss, dato, '|');

    if (tipo == "ATACAR") {
        int danio = std::stoi(dato);
        jugadorLocal.recibirDanio(danio);
        historial.push("El rival ataco con " + std::to_string(danio));
        std::cout << "\nEl rival te ataco." << std::endl;
    }
    else if (tipo == "DEFENDER") {
        historial.push("El rival se defendio");
        std::cout << "\nEl rival se defendio." << std::endl;
    }
    else if (tipo == "CARGAR") {
        historial.push("El rival cargo energia");
        std::cout << "\nEl rival cargo energia." << std::endl;
    }
    else if (tipo == "USAR") {
        historial.push("El rival uso " + dato);
        std::cout << "\nEl rival uso " << dato << "." << std::endl;
    }
}

int main() {
    ConexionJuego conexion;
    std::string nombreLocal, nombreRemoto;
    int modo;
    const int PUERTO = 5000;

    std::cout << "===== JUEGO EN LINEA =====" << std::endl;
    std::cout << "Ingrese su nombre: ";
    getline(std::cin, nombreLocal);

    std::cout << "Seleccione modo:" << std::endl;
    std::cout << "1. Servidor" << std::endl;
    std::cout << "2. Cliente" << std::endl;
    std::cout << "Opcion: ";
    std::cin >> modo;
    std::cin.ignore();

    if (!conexion.inicializar()) return 1;

    bool turnoInicialLocal = false;

    if (modo == 1) {
        if (!conexion.iniciarServidor(PUERTO)) return 1;
        if (!conexion.esperarCliente()) return 1;
        turnoInicialLocal = true;
    } else {
        std::string ipServidor;
        std::cout << "Ingrese IP del servidor: ";
        getline(std::cin, ipServidor);

        if (!conexion.conectarAServidor(ipServidor, PUERTO)) return 1;
        turnoInicialLocal = false;
    }

    conexion.enviar(nombreLocal);
    nombreRemoto = conexion.recibir();

    Jugador jugadorLocal(nombreLocal);
    Jugador jugadorRemoto(nombreRemoto);

    Partida partida(jugadorLocal, jugadorRemoto, turnoInicialLocal);

    PilaHistorial historial;
    ListaInventario inventario;

    inventario.agregarObjeto("Pocion", 20);
    inventario.agregarObjeto("Pocion", 20);
    inventario.agregarObjeto("Bateria", 10);

    while (!partida.partidaTerminada()) {
        partida.mostrarEstadoGeneral();

        if (partida.esTurnoLocal()) {
            int opcion;
            bool accionValida = false;

            while (!accionValida) {
                mostrarMenu();
                std::cin >> opcion;
                std::cin.ignore();

                switch (opcion) {
                    case 1:
                        conexion.enviar("ATACAR|15");
                        partida.getJugadorRemoto().recibirDanio(15);
                        historial.push("Atacaste con 15");
                        accionValida = true;
                        break;

                    case 2:
                        partida.getJugadorLocal().setDefensa(true);
                        conexion.enviar("DEFENDER|0");
                        historial.push("Te defendiste");
                        accionValida = true;
                        break;

                    case 3:
                        partida.getJugadorLocal().cargarEnergia(10);
                        conexion.enviar("CARGAR|10");
                        historial.push("Cargaste energia");
                        accionValida = true;
                        break;

                    case 4: {
                        int valor;
                        if (inventario.usarObjeto("Pocion", valor)) {
                            partida.getJugadorLocal().curar(valor);
                            conexion.enviar("USAR|Pocion");
                            historial.push("Usaste Pocion");
                            accionValida = true;
                        } else {
                            std::cout << "No tienes Pocion disponible." << std::endl;
                        }
                        break;
                    }

                    case 5: {
                        int valor;
                        if (inventario.usarObjeto("Bateria", valor)) {
                            partida.getJugadorLocal().cargarEnergia(valor);
                            conexion.enviar("USAR|Bateria");
                            historial.push("Usaste Bateria");
                            accionValida = true;
                        } else {
                            std::cout << "No tienes Bateria disponible." << std::endl;
                        }
                        break;
                    }

                    case 6:
                        historial.mostrar();
                        break;

                    case 7:
                        inventario.mostrarInventario();
                        break;

                    default:
                        std::cout << "Opcion invalida." << std::endl;
                }
            }

            partida.cambiarTurno();
        }
        else {
            std::cout << "\nEsperando accion del rival..." << std::endl;
            std::string mensaje = conexion.recibir();

            if (mensaje.empty()) {
                std::cout << "Conexion perdida." << std::endl;
                break;
            }

            aplicarMensajeRecibido(mensaje, partida.getJugadorLocal(), historial);
            partida.cambiarTurno();
        }
    }

    std::cout << "\n===== FIN DE LA PARTIDA =====" << std::endl;

    if (partida.getJugadorLocal().estaVivo() && !partida.getJugadorRemoto().estaVivo()) {
        std::cout << "Ganaste la partida." << std::endl;
    } else if (!partida.getJugadorLocal().estaVivo() && partida.getJugadorRemoto().estaVivo()) {
        std::cout << "Perdiste la partida." << std::endl;
    } else {
        std::cout << "La partida termino." << std::endl;
    }

    conexion.cerrar();
    return 0;
}
```

---

## Compilación completa

```bash
g++ main.cpp juego/Jugador.cpp juego/Accion.cpp juego/Partida.cpp estructuras/ListaInventario.cpp estructuras/PilaHistorial.cpp red/ConexionJuego.cpp -o JuegoEnLinea.exe -lws2_32
```

---

# Bloque 6. Pruebas entre compañeros

## Propósito del bloque

Comprobar que el sistema completo funciona entre dos computadoras reales dentro de la misma red.

## Prueba 1. Conexión

* una computadora inicia en modo servidor
* otra en modo cliente
* ambas intercambian nombre

## Prueba 2. Turnos

* un jugador realiza una acción
* el otro jugador la recibe
* el turno cambia correctamente

## Prueba 3. Historial

* realizar varias acciones
* verificar que se almacenan en la pila
* consultar historial desde el menú

## Prueba 4. Inventario

* usar una poción
* verificar que ya no aparezca si se agotó
* usar una batería
* comprobar que desaparezca al consumirse

## Prueba 5. Partida completa

* jugar hasta que uno de los jugadores llegue a vida cero

---

# Anexo A

## Generar el ejecutable `.exe`

Una vez que el sistema funcione, compilar todos los archivos:

```bash
g++ main.cpp juego/Jugador.cpp juego/Accion.cpp juego/Partida.cpp estructuras/ListaInventario.cpp estructuras/PilaHistorial.cpp red/ConexionJuego.cpp -o JuegoEnLinea.exe -lws2_32
```

## Organizar la carpeta final

Se recomienda una carpeta de entrega como esta:

```text
Entrega_JuegoEnLinea/
│
├── src/
│   ├── main.cpp
│   ├── juego/
│   ├── estructuras/
│   └── red/
│
├── JuegoEnLinea.exe
├── README.txt
└── capturas/
```

---

# Actividad de comprobación

Cada equipo deberá explicar y demostrar:

1. dónde está implementada la biblioteca personal de red
2. qué clases forman el modelo del juego
3. dónde se utiliza la pila
4. dónde se utiliza la lista enlazada
5. cómo se realiza la búsqueda de objetos
6. qué mensajes se envían por red
7. cómo se actualiza el estado local después de recibir una acción
8. cómo se determina el final de la partida

---
