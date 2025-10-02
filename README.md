# Tarea1EDAgrupo06
tarea semana 07 de Estructuras avanzadas grupo 06


#include <iostream>
#include <string>
#include <list>
using namespace std;

const int TABLE_SIZE = 10; // tamaño de la tabla hash

// Estructura para almacenar un producto
struct Producto {
    string nombre;
    int cantidad;
};

// Clase HashTable
class HashTable {
private:
    list<Producto> tabla[TABLE_SIZE];

    // Función hash (simple: suma de caracteres módulo TABLE_SIZE)
    int funcionHash(const string &key) {
        int hash = 0;
        for (char c : key) {
            hash += c;
        }
        return hash % TABLE_SIZE;
    }

public:
    // Agregar producto
    void agregarProducto(string nombre, int cantidad) {
        int indice = funcionHash(nombre);
        for (auto &p : tabla[indice]) {
            if (p.nombre == nombre) {
                p.cantidad += cantidad; // si ya existe, acumula
                cout << "Producto actualizado correctamente.\n";
                return;
            }
        }
        tabla[indice].push_back({nombre, cantidad});
        cout << "Producto agregado correctamente.\n";
    }

    // Buscar producto
    void buscarProducto(string nombre) {
        int indice = funcionHash(nombre);
        for (auto &p : tabla[indice]) {
            if (p.nombre == nombre) {
                cout << "Producto encontrado: " << p.nombre 
                     << " - Cantidad: " << p.cantidad << endl;
                return;
            }
        }
        cout << "Producto no encontrado.\n";
    }

    // Eliminar producto
    void eliminarProducto(string nombre) {
        int indice = funcionHash(nombre);
        for (auto it = tabla[indice].begin(); it != tabla[indice].end(); it++) {
            if (it->nombre == nombre) {
                tabla[indice].erase(it);
                cout << "Producto eliminado.\n";
                return;
            }
        }
        cout << "Producto no encontrado.\n";
    }

    // Mostrar inventario completo
    void mostrarInventario() {
        cout << "\n--- Inventario ---\n";
        for (int i = 0; i < TABLE_SIZE; i++) {
            for (auto &p : tabla[i]) {
                cout << "Producto: " << p.nombre 
                     << " | Cantidad: " << p.cantidad << endl;
            }
        }
        cout << "------------------\n";
    }
};

// Función principal con menú
int main() {
    HashTable inventario;
    int opcion;
    string nombre;
    int cantidad;

    do {
        cout << "\n--- Menu de Inventario ---\n";
        cout << "1. Agregar producto\n";
        cout << "2. Buscar cantidad de producto\n";
        cout << "3. Eliminar producto\n";
        cout << "4. Mostrar inventario\n";
        cout << "5. Salir\n";
        cout << "Seleccione una opcion: ";
        cin >> opcion;

        switch(opcion) {
            case 1:
                cout << "Ingrese nombre del producto: ";
                cin >> nombre;
                cout << "Ingrese cantidad: ";
                cin >> cantidad;
                inventario.agregarProducto(nombre, cantidad);
                break;

            case 2:
                cout << "Ingrese nombre del producto a buscar: ";
                cin >> nombre;
                inventario.buscarProducto(nombre);
                break;

            case 3:
                cout << "Ingrese nombre del producto a eliminar: ";
                cin >> nombre;
                inventario.eliminarProducto(nombre);
                break;

            case 4:
                inventario.mostrarInventario();
                break;

            case 5:
                cout << "Saliendo del sistema...\n";
                break;

            default:
                cout << "Opcion no valida.\n";
        }
    } while (opcion != 5);

    return 0;
}
