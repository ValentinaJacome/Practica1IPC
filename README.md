# Practica1IPC
## Datos generales

- Practica: Práctica 1
- Curso: Introduccion a la Programacion y Computación 1

---

## Información General:

El programa es una funcionalidad diseñada para gestionar las reservaciones de vuelo de la aerolinea Aero-USAC. 

Alcance y Funcionalidades: Se ejecuta en consola y permite vender boletos, buscar asientos, ver el mapa del avion y generar un reporte del uso de los asientos.

El avion tiene 20 filas y 6 columnas (A B C D E F) lo que da 120 asientos en total. Las primeras 5 filas son primera clase y las demas son economica.

---

## Funcionalidades

1. Venta de boleto individual
2. Buscar boletos contiguos
3. Asignacion automatica
4. Ver mapa de la cabina
5. Reporte del vuelo
6. Salir

---

## Estructuras utilizadas

Se uso una matriz de char de 20x6 para representar la cabina del avion.

```java
static char[][] cabina = new char[20][6];
```

Los estados posibles de cada asiento son:

- L = libre
- X = ocupado
- B = bloqueado por distanciamiento VIP

No se usaron ArrayList ni ninguna coleccion dinamica.

---

## Explicacion del codigo

### Variables de estado

```java
//// Estado de como se encuentran los asientos////
static final char LIBRE = 'L';
static final char Ocupado = 'X';
static final char BLOQUEADO = 'B';

static char[][] cabina = new char[20][6];
```

Estas son las variables principales del programa. `LIBRE`, `Ocupado` y `BLOQUEADO` son constantes que representan el estado de cada asiento. La matriz `cabina` tiene 20 filas y 6 columnas, cada posicion guarda uno de esos tres caracteres.

Los corchetes `[][]` indican que es una matriz bidimensional, el primer `[]` son las filas y el segundo `[]` son las columnas.

---

### Menu principal (do-while + try/catch + switch)

```java
do {
    System.out.println("\n=== AERO-USAC: SISTEMA DE ABORDAJE ===");
    System.out.println("1. Venta de Boleto Individual");
    System.out.println("2. Buscar Boletos Contiguos");
    System.out.println("3. Asignacion Automatica");
    System.out.println("4. Mostrar Mapa de la Cabina");
    System.out.println("5. Reporte de Vuelo");
    System.out.println("6. Salir");
    System.out.print("Seleccione una opcion: ");

    try {
        opcion = Integer.parseInt(sc.nextLine().trim());
    } catch (NumberFormatException e) {
        opcion = 0;
    }

    switch (opcion) {
        case 1: ventaIndividual(); break;
        case 2: buscarContiguos(); break;
        case 3: asignacionAutomatica(); break;
        case 4: mostrarCabina(); break;
        case 5: reporteVuelo(); break;
        case 6: System.out.println("Hasta luego."); break;
    }

} while (opcion != 6);
```

Este bloque tiene tres partes importantes:

**do-while:** hace que el menu se repita hasta que el usuario elija la opcion 6. Sin esto el programa correria una sola vez y se cerraria.

**try/catch:** sirve para atrapar errores. Si el usuario escribe algo que no es numero (por ejemplo "hola"), el `parseInt` fallaria y el programa crashearia. El `catch` lo atrapa y pone `opcion = 0` para que siga funcionando normal.

**switch:** segun el numero que escribio el usuario llama al metodo que corresponde. El `break` en cada case es importante porque sin el el switch seguiria ejecutando todos los casos siguientes en cadena.

---

### Metodo para mostrar la cabina

```java
static void mostrarCabina() {
    System.out.println("\n     A    B    C       D    E    F");
    for (int f = 0; f < 20; f++) {
        if (f + 1 < 10) System.out.print(" ");
        System.out.print((f + 1) + ": ");
        for (int c = 0; c < 6; c++) {
            System.out.print("[" + cabina[f][c] + "]");
            if (c == 2) System.out.print(" || ");
        }
        System.out.println();
    }
}
```

Recorre toda la matriz con dos ciclos `for` (uno para filas y uno para columnas) e imprime el estado de cada asiento. Cuando llega a la columna 2 (C) imprime `||` que representa el pasillo central del avion.

---

### Validacion de coordenadas

```java
static int[] parsearAsiento(String codigo) {
    char letra = Character.toUpperCase(codigo.charAt(0));
    int col = letraAColumna(letra);
    if (col == -1) return null;
    int fila = Integer.parseInt(codigo.substring(1));
    if (fila < 1 || fila > 20) return null;
    return new int[]{fila - 1, col};
}
```

Convierte lo que escribe el usuario (ejemplo "C15") en indices de la matriz. La letra se convierte a numero de columna (A=0, B=1... F=5) y el numero de fila se ajusta restando 1 porque la matriz empieza en 0.

---

### Logica VIP

```java
// si el asiento esta en filas 1-5 se aplica distanciamiento
if (fila <= 4) {
    venderVIP(fila, col);
}
```

Cuando se vende un asiento en primera clase el sistema bloquea automaticamente el asiento de adelante y el de atras en la misma columna para garantizar el distanciamiento.

---

### Reporte de vuelo

```java
static void reporteVuelo() {
    int libres = 0, ocupados = 0, bloqueados = 0;
    for (int f = 0; f < 20; f++) {
        for (int c = 0; c < 6; c++) {
            if      (cabina[f][c] == LIBRE)     libres++;
            else if (cabina[f][c] == Ocupado)   ocupados++;
            else if (cabina[f][c] == BLOQUEADO) bloqueados++;
        }
    }
    System.out.println("Asientos libres:    " + libres);
    System.out.println("Asientos ocupados:  " + ocupados);
    System.out.println("Asientos bloqueados:" + bloqueados);
    System.out.println("Ingresos recaudados: $" + ingresos);
}
```

Recorre toda la matriz contando cuantos asientos hay en cada estado y muestra el resumen.

---

## Capturas de funcionamiento
https://github.com/ValentinaJacome/Practica1IPC/blob/0d1c6b4aca7007842a6231f4f9b99139f7d260c5/C%C3%B3digo%201.png


---
