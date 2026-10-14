# Cafetería Universitaria: patrones de diseño

## 1. Nombre y problema que resuelve
**Nombre:** Cafetería Universitaria.  
**Problema:** registrar pedidos de bebidas, permitir agregar ingredientes opcionales y comunicar los cambios de estado del pedido de forma organizada.

## 2. Patrones seleccionados

| Grupo | Patrón | Aplicación |
|---|---|---|
| Creacional | Factory Method | `CreadorCafe` y `CreadorTe` crean objetos que comparten la interfaz `Bebida`. |
| Estructural | Decorator | `Leche` y `Chocolate` envuelven una bebida y añaden descripción y costo sin modificar las clases `Cafe` o `Te`. |
| Comportamiento | Observer | `Pedido` notifica a `PantallaCocina` y `Notificador` cuando cambia el estado. |

## 3. Relación e integración de los patrones
El flujo principal inicia con Factory Method, que crea una bebida concreta y la devuelve como `Bebida`. Luego Decorator recibe ese mismo objeto y, si el usuario elige un ingrediente, lo envuelve para ampliar su descripción y precio. Finalmente, el pedido cambia de estado y Observer avisa a dos observadores. Los tres patrones participan en una sola compra.

Si se elimina Factory Method, la selección de productos debe volver a crear objetos concretos directamente desde el flujo principal. Si se elimina Decorator, los ingredientes opcionales tendrían que incorporarse a las clases originales o manejarse con condiciones adicionales. Si se elimina Observer, el pedido tendría que llamar manualmente a cada destinatario cada vez que cambia su estado.

## 4. Diagrama de clases (relaciones)
- `Bebida` es la interfaz común de `Cafe`, `Te` y `DecoradorBebida`.
- `CreadorBebida` define `crearBebida()`. `CreadorCafe` y `CreadorTe` implementan ese método.
- `DecoradorBebida` contiene una referencia a `Bebida`; `Leche` y `Chocolate` extienden el comportamiento.
- `Pedido` mantiene una colección de `Observador`. `PantallaCocina` y `Notificador` implementan la interfaz `Observador`.

Representación resumida:

```text
CreadorBebida
   |-- CreadorCafe
   |-- CreadorTe
          | crea
          v
        Bebida <----------------------+
        |  ^                          |
        |  |                          |
       Cafe Te                DecoradorBebida
                                  |-- Leche
                                  |-- Chocolate

Pedido o---- Observador
                 |-- PantallaCocina
                 |-- Notificador
```

## 5. Compilar y ejecutar
Se requiere un compilador de C++ compatible con C++11 o superior.

**Linux / macOS con g++:**
```bash
g++ main.cpp -o cafeteria
./cafeteria
```

**Windows con MinGW:**
```bash
g++ main.cpp -o cafeteria.exe
cafeteria.exe
```

También se puede copiar el código a un IDE de C++ y ejecutar el programa.

## 6. Ejemplo de salida esperada
Si se selecciona café y leche, se muestra un producto `Cafe con leche` con total `$6000`. Luego aparecen mensajes de la cocina y de la notificación para los estados `En preparacion` y `Listo para entregar`.

## 7. Integrantes
- [Escribir nombre del integrante 1]
- [Escribir nombre del integrante 2]
- [Escribir nombre del integrante 3]
- [Escribir nombre del integrante 4, si aplica]

## 8. Fuente de consulta
Refactoring.Guru. Catálogo de patrones de diseño:
https://refactoring.guru/design-patterns/catalog

Páginas consultadas:
- Factory Method: https://refactoring.guru/design-patterns/factory-method
- Decorator: https://refactoring.guru/design-patterns/decorator
- Observer: https://refactoring.guru/design-patterns/observer

