# Trabajo: Comparativa de Tecnologías y Soluciones en Sistemas Embebidos

## 1. Comparativa entre lenguaje de programación y lenguaje de descripción de hardware

| Característica | Lenguaje de Programación (Ej. C/C++, Python) | Lenguaje de Descripción de Hardware (Ej. VHDL, Verilog) |
|----------------|----------------------------------------------|----------------------------------------------------------|
| Propósito | Describir algoritmos secuenciales que ejecuta un procesador. | Describir el **comportamiento y estructura física** del hardware digital. |
| Nivel de abstracción | Alto nivel, centrado en la lógica de control y datos. | Bajo nivel, enfocado en señales, temporización y arquitectura. |
| Ejecución | Se compila y ejecuta sobre una CPU o microcontrolador. | Se sintetiza para implementarse físicamente en una FPGA o ASIC. |
| Paralelismo | Secuencial, instrucciones una tras otra. | Altamente paralelo, múltiples procesos simultáneos. |
| Ejemplo de uso | Control de sensores, comunicación serial, IoT. | Diseño de sistemas digitales, procesadores, controladores. |

**Conclusión:**  
Los lenguajes de programación controlan sistemas existentes (software sobre hardware), mientras que los lenguajes de descripción de hardware crean el hardware mismo a nivel lógico.

---

## 2. Comparación entre Microcontrolador, Microprocesador, FPGA y PLD

| Dispositivo | Descripción | Ventajas | Desventajas | Ejemplo |
|--------------|-------------|-----------|--------------|----------|
| **Microcontrolador (MCU)** | Circuito integrado con CPU, memoria y periféricos en un solo chip. | Bajo costo, consumo reducido, ideal para control en tiempo real. | Menor potencia de procesamiento y flexibilidad. | ESP32, Arduino, PIC. |
| **Microprocesador (CPU)** | Unidad central de procesamiento, requiere periféricos externos. | Alta capacidad de cómputo, multitarea. | Mayor costo y consumo. | Intel i7, ARM Cortex-A. |
| **FPGA** | Matriz de compuertas programables que permite crear circuitos digitales personalizados. | Reconfigurable, paralelismo total, excelente rendimiento en tareas específicas. | Programación compleja, mayor consumo y costo. | Basys 3, Xilinx Spartan. |
| **PLD** | Dispositivo lógico programable básico, usado para implementar circuitos simples. | Reconfigurable, rápido de prototipar. | Menor capacidad y flexibilidad que una FPGA. | GAL, CPLD. |

---

## 3. Lenguajes más usados en la programación de hardware

| Tipo de lenguaje | Ejemplos | Aplicación principal |
|------------------|-----------|----------------------|
| **Descripción de hardware (HDL)** | VHDL, Verilog, SystemVerilog | Diseño de circuitos digitales, FPGA, ASIC. |
| **Lenguajes de alto nivel para síntesis** | C/C++, OpenCL, SystemC | Modelado de sistemas y generación automática de hardware. |
| **Lenguajes de programación embebida** | C, C++, MicroPython, Rust | Programación de microcontroladores (ESP32, STM32, etc.). |

La tendencia actual es combinar HDL (para hardware reconfigurable) con lenguajes de alto nivel (para lógica de control), integrando lo mejor de ambos mundos en sistemas embebidos heterogéneos.

---

## 4. Propuesta de Solución Tecnológica Embebida

### a. Proceso Microcontrolado (Ejemplo con ESP32)

**Descripción:**  
Se propone un sistema de monitoreo ambiental que mida temperatura y humedad con un sensor DHT11, mostrando los datos en el monitor serial.

Código en C++ (Arduino IDE):

```cpp
#include <DHT.h>

#define DHTPIN 4
#define DHTTYPE DHT11
DHT dht(DHTPIN, DHTTYPE);

void setup() {
  Serial.begin(9600);
  dht.begin();
}

void loop() {
  float temperatura = dht.readTemperature();
  float humedad = dht.readHumidity();
  Serial.print("Temperatura: ");
  Serial.print(temperatura);
  Serial.print(" °C  Humedad: ");
  Serial.print(humedad);
  Serial.println(" %");
  delay(2000);
}
```

---

### b. Programación de Hardware 

Para este ejemplo, se ha decidido trabajar con Basys 3
Descripción:
Diseño de un contador binario de 4 bits que se visualiza mediante los LEDs de la placa Basys 3.

Código en VHDL:

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity contador_4bits is
    Port ( clk : in STD_LOGIC;
           leds : out STD_LOGIC_VECTOR(3 downto 0));
end contador_4bits;

architecture Behavioral of contador_4bits is
signal cuenta : STD_LOGIC_VECTOR(3 downto 0) := "0000";
begin
    process(clk)
    begin
        if rising_edge(clk) then
            cuenta <= cuenta + 1;
        end if;
    end process;
    leds <= cuenta;
end Behavioral;
```


---

## 5. Ventajas y Desventajas de Cada Sistema

| Aspecto | ESP32 (Microcontrolado) | Basys 3 (FPGA) |
|----------|-------------------------|----------------|
| **Velocidad de procesamiento** | Media (hasta 240 MHz). | Alta, lógica paralela. |
| **Facilidad de desarrollo** | Alta (IDE amigable, librerías). | Media-baja (requiere HDL y síntesis). |
| **Flexibilidad** | Limitada al hardware interno. | Total, hardware reconfigurable. |
| **Costo** | Bajo. | Medio-alto. |
| **Consumo de energía** | Bajo. | Moderado-alto. |
| **Aplicaciones ideales** | IoT, sensores, automatización. | Procesamiento digital, control avanzado, sistemas de alta velocidad. |

**Conclusión general:**  
El ESP32 es ideal para proyectos prácticos, económicos y de rápida implementación, mientras que la Basys 3 (FPGA) ofrece un control total del hardware y un desempeño superior, aunque requiere mayor conocimiento técnico y tiempo de desarrollo.

