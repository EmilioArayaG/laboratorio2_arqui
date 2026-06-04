# Laboratorio 3: El Sistema de Rutas Misticas

**Universidad Tecnica Federico Santa Maria**
**INF-245: Arquitectura y Organizacion de Computadores**

**Nombres:**  Emilio Alfonso Araya Guzman, Nicolas Andre Muñoz Lira

## Descripcion del Proyecto
Este proyecto consiste en la implementacion en hardware de una Maquina de Estados Finita (FSM) tipo Moore, disenada para resolver un problema de navegacion automatizada a traves de un grafo dirigido (mazmorra). El circuito calcula y ejecuta la ruta de escape desde cualquier nodo inicial ingresado, finalizando su recorrido de manera segura y definitiva en el Nodo F (`0101`).

## Requisitos Previos
* **Software:** Logisim-evolution (version 3.8.0 o superior recomendada, se utilizo la version 4.1.0).
* **Archivo principal:** `main.circ` (contiene el circuito secuencial y sus respectivos subcircuitos combinacionales).

## Instrucciones de Ejecucion y Pruebas

Para validar el correcto funcionamiento del circuito y la condicion de parada, siga estos pasos detallados:

### 1. Carga del Nodo Inicial (Modo Manual)
1. Abra el archivo `main.circ` en Logisim-evolution.
2. En la interfaz principal, asegurese de que el pin de control **`Cargar_Inicio`** este en valor logico **`1`**.
3. Ingrese el codigo binario de 4 bits correspondiente a la habitacion de inicio deseada a traves de los pines de entrada **A3 (MSB)** a **A0 (LSB)**.
   * *Nota:* El arreglo de LEDs validara instantaneamente la habitacion seleccionada.
4. Aplique un unico pulso de reloj (`Ctrl + T` o cambiando el pin `Reloj` manualmente) para almacenar este estado inicial en los Flip-Flops. El Display de 7 segmentos mostrara el simbolo runico de la habitacion cargada.

### 2. Avance Automatico (Lazo Cerrado)
1. Cambie el estado del pin de control **`Cargar_Inicio`** a valor logico **`0`**. Esto aisla las entradas manuales y cierra el lazo de retroalimentacion de la FSM.
2. Active la simulacion del reloj (`Simulate -> Ticks Enabled` o `Ctrl + K`), o aplique pulsos manuales (`Ctrl + T`).
3. El circuito avanzara secuencialmente a traves de las habitaciones intermedias segun la tabla de transiciones disenada, actualizando el Display en cada flanco de subida.

### 3. Verificacion de la Condicion de Parada
1. Permita que el circuito avance de forma autonoma hasta alcanzar el **Nodo F** (Codigo `0101`).
2. Una vez que el Display muestre el simbolo runico de la F, el sistema entrara en un bucle cerrado seguro.
3. Puede verificar que pulsos de reloj adicionales no alteraran el estado actual, cumpliendo con la detencion automatica exigida.

## Supuestos del Diseno
Durante el desarrollo y la implementacion de esta Maquina de Estados, se consideraron los siguientes supuestos tecnicos:
* **Manejo de Reset:** Se asumio que el sistema no requiere un pin de "Reset" asincrono global para volver a un estado cero. El reinicio de la maquina o el cambio de ruta se gestiona estrictamente de forma sincrona, activando la senal `Cargar_Inicio = 1` y sobreescribiendo la memoria de los Flip-Flops con un nuevo flanco de reloj.
* **Entorno Ideal de Simulacion:** Al tratarse de un diseno validado exclusivamente en el emulador Logisim-evolution, se asumio un entorno logico ideal (libre de ruidos o retardos de propagacion criticos). Por lo tanto, no se incluyeron circuitos anti-rebote (debouncing) en los pines de entrada manual ni en la senal de reloj.
* **Robustez sin Estados Invalidos:** Se asumio que el circuito debia ser a prueba de fallos frente a cualquier combinacion binaria. Por ello, se mapearon exhaustivamente los 16 estados posibles (4 bits) en la logica combinacional, asumiendo que no existen estados "Don't Care" que pudieran atrapar a la FSM en un bucle indeterminado fuera de la ruta planificada.

## Contenido del Entregable
* `main.circ`: Archivo ejecutable de Logisim-evolution con el circuito principal y subcircuitos (`logica_transicion`, `decodificador_entrada`, `decodificador_display`).
* `Informe_Lab_3.pdf`: Documento tecnico detallando el analisis teorico, mapas de Karnaugh, funciones minimizadas, arquitectura del circuito y capturas de validacion empirica.
* `README.md`: Este archivo de instrucciones.
