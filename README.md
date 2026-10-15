# TP6 — Cronómetro Digital + Teclado Matricial + Comunicación Serie UART

**Materia:** Electrónica Digital II — Universidad Nacional de Córdoba (UNC)  
**Comisión:** COMUN 4.2 Blasco  
**Grupo N°:** 21  
**Scrum Master:** Martina Arias  
**Desarrolladores:** 
* Elias Josué León
* Grisel Alejandra Ramos
* Ivan Andrés Choque Solares
* Emiliano Gonzalez Ricardo
* Cecilia Valentina Bianchi

---

## 📝 Descripción del Proyecto
Implementación de un sistema digital integrado sobre el microcontrolador **PIC16F887** que combina un cronómetro digital (00.00 a 99.99 segundos) multiplexado en 4 displays de 7 segmentos, carga de tiempo local mediante un **teclado matricial 4x4** y **comunicación bidireccional UART** a 9600 baudios conectada a una PC mediante la terminal **Hércules**.

El programa opera en un bucle principal continuo transmitiendo de forma automática la lectura del tiempo congelado en formato ASCII (`SS.CC\r\n`) al pausar la cuenta y permitiendo recibir caracteres ASCII numéricos desde la PC para predefinir el tiempo inicial por desplazamiento a la izquierda (Shift BCD).

---

## 💡 Conceptos e Implementación Técnica

* **Microcontrolador:** PIC16F887 con Cristal externo de 4 MHz.
* **Cronómetro (00.00 a 99.99 s):** Incremento de centésimas y segundos mediante interrupciones periódicas generadas por el **Timer0** (Prescaler 1:32, precarga .100, interrupción cada 5 ms).
* **Multiplexado de Displays:** Muestra el tiempo acumulado en 4 displays de 7 segmentos (Cátodo Común) utilizando el `PORTD` para el envío de segmentos BCD y el `PORTC` (`RC0-RC3`) para el encendido secuencial de cada dígito (refresco total cada 20 ms).
* **Controles Físicos:**
  * **Pulsador RB0 (`INT`):** Alterna entre los estados *Start* y *Stop* con filtro antirrebote por software (30 ciclos de Timer0).
  * **Pulsador RB1:** Reinicia la cuenta a `00.00` únicamente cuando el cronómetro se encuentra detenido.
  * **Teclado Matricial 4x4 (`PORTA` / `PORTB`):** Permite el ingreso de dígitos de 0 a 9 mediante barrido (*polling*) secuencial de filas (`RA0-RA3`) y lectura de columnas (`RB4-RB7`).
* **Comunicación PC (EUSART / UART a 9600 Baudios):**
  * **Configuración:** $SPBRG = 25$, $BRGH = 1$, $TXEN = 1$, $SPEN = 1$, $CREN = 1$.
  * **Transmisión (TX - Pin RC6):** Al presionar el botón de pausa (`RB0`), se activa la bandera `FLAG_ENVIAR_UART` y el bucle principal transmite los 5 caracteres del tiempo (`SS.CC`) más un salto de línea (`\r\n`) a la terminal de la PC.
  * **Recepción (RX - Pin RC7):** Detecta la recepción de bytes (`PIR1, RCIF == 1`), valida que el carácter se encuentre en el rango ASCII de dígitos (`'0'` a `'9'`), convierte el valor a numérico (`ASCII - '0'`) y aplica el desplazamiento (*Shift BCD*) en la pantalla.

---

## 📊 Diagramas del Sistema

### Diagrama de Bloques del Sistema
![Diagrama de Bloques](./Diagrama%20de%20bloques.png)

### Diagrama de Flujo Principal (MAIN)
![Diagrama de Flujo Principal](./Diagrama%20de%20flujo%20principal.png)

### Diagrama de Flujo de la ISR (Interrupción)
![Diagrama ISR](./Diagrama%20ISR.png)

---

## 🛠️ Historias de Usuario (Entrevista con el Cliente)

1. **Como usuario,** quiero ver el tiempo del cronómetro reflejado en displays de 7 segmentos sin parpadeos visuales.
2. **Como usuario,** quiero poder iniciar y pausar el conteo en cualquier instante presionando el botón `RB0`.
3. **Como usuario,** quiero que al pausar el cronómetro, el tiempo congelado se envíe de manera automática a la terminal de una PC mediante el puerto serie.
4. **Como usuario,** quiero poder ingresar un tiempo predefinido desde un teclado matricial físico cuando la cuenta esté detenida.
5. **Como usuario,** quiero poder ingresar dígitos de tiempo directamente enviándolos desde el teclado numérico de la PC (Terminal Hércules) a través de la interfaz UART.
6. **Como usuario,** quiero disponer de un botón físico de Reset (`RB1`) para reiniciar la medición a `00.00` únicamente cuando el sistema no esté contando.

---

## ✅ Definition of Done (DoD)

- [x] **Transmisión UART Validada:** Se comprueba el envío correcto de la cadena `SS.CC\r\n` a 9600 baudios en la terminal Hércules al conmutar el estado a *Stop*.
- [x] **Recepción y Conversión ASCII:** Se valida la lectura del puerto serie en `RCREG`, descartando caracteres no numéricos y cargando correctamente los dígitos de 0 a 9.
- [x] **Inhibición de Carga en Movimiento:** Tanto el teclado físico como la entrada serie de la PC quedan bloqueados si la bandera `ESTADO` indica que el cronómetro está en ejecución (`RUN`).
- [x] **Multiplexado Estable:** La visualización se mantiene fija y sin parpadeos a través del refresco a 5 ms por dígito administrado por el Timer0 dentro de la ISR.
- [x] **Código y Diagramas Sincronizados:** El flujo lógico reflejado en los diagramas representa exactamente la secuencia implementada en el código fuente Assembly.
