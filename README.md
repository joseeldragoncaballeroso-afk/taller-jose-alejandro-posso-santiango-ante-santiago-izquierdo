
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// Interfaz comun de las bebidas
class Bebida {
public:
    virtual string descripcion() = 0;
    virtual double precio() = 0;
    virtual ~Bebida() {}
};

// Productos concretos
class Cafe : public Bebida {
public:
    string descripcion() override {
        return "Cafe";
    }
    double precio() override {
        return 5000;
    }
};

class Te : public Bebida {
public:
    string descripcion() override {
        return "Te";
    }
    double precio() override {
        return 4000;
    }
};

// 1. CREACIONAL: FACTORY METHOD
class CreadorBebida {
public:
    virtual Bebida* crearBebida() = 0;
    virtual ~CreadorBebida() {}
};

class CreadorCafe : public CreadorBebida {
public:
    Bebida* crearBebida() override {
        return new Cafe();
    }
};

class CreadorTe : public CreadorBebida {
public:
    Bebida* crearBebida() override {
        return new Te();
    }
};

// 2. ESTRUCTURAL: DECORATOR
class DecoradorBebida : public Bebida {
protected:
    Bebida* bebida;
public:
    DecoradorBebida(Bebida* b) : bebida(b) {}

    ~DecoradorBebida() override {
        delete bebida;
    }
};

class Leche : public DecoradorBebida {
public:
    Leche(Bebida* b) : DecoradorBebida(b) {}

    string descripcion() override {
        return bebida->descripcion() + " con leche";
    }

    double precio() override {
        return bebida->precio() + 1000;
    }
};

class Chocolate : public DecoradorBebida {
public:
    Chocolate(Bebida* b) : DecoradorBebida(b) {}

    string descripcion() override {
        return bebida->descripcion() + " con chocolate";
    }

    double precio() override {
        return bebida->precio() + 1500;
    }
};

// 3. COMPORTAMIENTO: OBSERVER
class Observador {
public:
    virtual void actualizar(string estado) = 0;
    virtual ~Observador() {}
};

class PantallaCocina : public Observador {
public:
    void actualizar(string estado) override {
        cout << "[Cocina] Estado del pedido: "
             << estado << endl;
    }
};

class Notificador : public Observador {
public:
    void actualizar(string estado) override {
        cout << "[Notificacion] El pedido esta "
             << estado << endl;
    }
};

class Pedido {
private:
    vector<Observador*> observadores;
    string estado;

public:
    Pedido() {
        estado = "Recibido";
    }

    void agregarObservador(Observador* o) {
        observadores.push_back(o);
    }

    void cambiarEstado(string nuevoEstado) {
        estado = nuevoEstado;

        for (Observador* o : observadores) {
            o->actualizar(estado);
        }
    }
};

int main() {
    int opcion, extra;
    Bebida* bebida = nullptr;
    CreadorBebida* creador = nullptr;

    cout << "=== CAFETERIA UNIVERSITARIA ===\n";
    cout << "Seleccione su bebida:\n";
    cout << "1. Cafe ($5000)\n";
    cout << "2. Te ($4000)\n";
    cout << "Opcion: ";
    cin >> opcion;

    if (opcion == 1) {
        creador = new CreadorCafe();
    } else if (opcion == 2) {
        creador = new CreadorTe();
    } else {
        cout << "Opcion no valida.\n";
        return 0;
    }

    // Factory Method crea la bebida
    bebida = creador->crearBebida();
    delete creador;

    cout << "\nDesea agregar un ingrediente?\n";
    cout << "1. Leche (+$1000)\n";
    cout << "2. Chocolate (+$1500)\n";
    cout << "3. Ninguno\n";
    cout << "Opcion: ";
    cin >> extra;

    // Decorator agrega opciones sin modificar
    // las clases Cafe o Te
    if (extra == 1) {
        bebida = new Leche(bebida);
    } else if (extra == 2) {
        bebida = new Chocolate(bebida);
    } else if (extra != 3) {
        cout << "Opcion no valida; se conserva "
             << "la bebida original.\n";
    }

    cout << "\n=== RESUMEN DEL PEDIDO ===\n";
    cout << "Producto: " << bebida->descripcion()
         << endl;
    cout << "Total: $" << bebida->precio() << endl;

    // Observer registra los interesados
    // en los cambios del pedido
    Pedido pedido;
    PantallaCocina cocina;
    Notificador notificacion;

    pedido.agregarObservador(&cocina);
    pedido.agregarObservador(&notificacion);

    cout << "\nProcesando pedido...\n";
    pedido.cambiarEstado("En preparacion");
    pedido.cambiarEstado("Listo para entregar");

    delete bebida;

    cout << "\nGracias por su compra.\n";
    return 0;
}
