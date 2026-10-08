# tp6-comun-5-blasco-grupo-21
Repositorio del **Trabajo Práctico N°6** para la materia **Electrónica Digital II** - UNC. 

Cada equipo implementa la actividad adaptando el diseño según las especificaciones y requerimientos dados por su docente de comisión.

---

## 👥 Integrantes del Equipo

- **Comisión / Docente:** COMUN 4.2 Blasco
- **Grupo N°:** 21
- **Scrum Master:** Martina Arias
- **Desarrolladores:**
  - Elias Josué León
  - Grisel Alejandra Ramos
  - Ivan Andrés Choque Solares
  - Emiliano Gonzalez Ricardo
  - Cecilia Valentina Bianchi

---

## 📝 Descripción del Proyecto
Creamos un programa que haga un bucle infinito en el cual lleva a cabo una demora de uno o dos segundos, incrementa un registro y lo envía a una PC, visualice los datos en un terminal. Solo incrementa la variable y la envía, lo hacemos por medio de hercules el cual 
permite visualizar los datos  en codificación ASCII en una pantalla.

---
## 💡 Conceptos Involucrados

-**Cronómetro (00.00 a 99.99 s):** Incrementa centésimas y segundos mediante interrupciones de tiempo (Timer0).
-**Multiplexado de Displays:** Muestra el tiempo en 4 displays de 7 segmentos cathode común usando PORTD (segmentos) y PORTC (activación de dígitos).

-**Controles Físicos:**
-**Botón RB0:** Inicia / Pausa el conteo (con antirrebote por software).
-**Botón RB1:** Reinicia el tiempo a 00.00.
-**Teclado Matricial (4×3 en PORTA/PORTB):** Permite ingresar un tiempo inicial cuando el cronómetro está pausado.

-**Comunicación PC (UART a 9600 baudios):**
-**TX (Envío):** Transmite el tiempo actual a la PC (SS.CC\r\n) cada vez que se pausa el cronómetro.
-**RX (Recepción):** Recibe dígitos numéricos en formato ASCII desde la PC para cargarlos en el cronómetro por desplazamiento a la izquierda.

---

## 📐 Documentación Técnica

### 1. Diagrama de Bloques
<img width="1600" height="532" alt="WhatsApp Image 2026-09-01 at 7 33 58 PM" src="https://github.com/user-attachments/assets/843e0dce-999e-48ca-be7e-1921b8c193c9" />


### 1. Diagrama de Flujo
<img width="1058" height="1600" alt="WhatsApp Image 2026-09-01 at 6 11 51 PM" src="https://github.com/user-attachments/assets/10522317-eda1-4c4e-945f-e33f44853686" />

