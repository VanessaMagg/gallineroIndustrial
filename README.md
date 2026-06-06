# 🐔 SmartFarm — Gallinero Inteligente

> \*\*Proyecto académico de automatización IoT\*\* desarrollado bajo metodología de prototipado ágil con dinámica cliente-desarrollador.  
> Cooperativa Avícola Local · GallineroSmart v1.0

\---

## 📋 Tabla de Contenidos

1. [Descripción del Proyecto](#descripción-del-proyecto)
2. [Contexto y Problemática](#contexto-y-problemática)
3. [Módulos del Sistema](#módulos-del-sistema)
4. [Arquitectura General](#arquitectura-general)
5. [Estructura del Repositorio](#estructura-del-repositorio)
6. [Entregables por Etapa](#entregables-por-etapa)
7. [Interfaces Desarrolladas](#interfaces-desarrolladas)
8. [Requerimientos del Sistema](#requerimientos-del-sistema)
9. [Stack Tecnológico](#stack-tecnológico)
10. [Cómo Ejecutar el Proyecto](#cómo-ejecutar-el-proyecto)
11. [Dinámica de Trabajo (Espejo Cruzado)](#dinámica-de-trabajo-espejo-cruzado)
12. [Equipo](#equipo)

\---

## 📖 Descripción del Proyecto

**SmartFarm** es un sistema de automatización IoT para gallineros de pequeña escala, diseñado para una cooperativa de agricultores locales. El sistema controla y monitorea en tiempo real las condiciones del gallinero, toma decisiones automáticas sobre iluminación, temperatura y alimentación, y envía alertas al celular del granjero cuando algo requiere intervención humana.

El proyecto fue construido siguiendo una metodología de **prototipado por etapas** con roles diferenciados de cliente y desarrollador, simulando un entorno profesional real de levantamiento de requerimientos, diseño y construcción de MVP.

\---

## 🌾 Contexto y Problemática

Una cooperativa de pequeños agricultores locales necesita modernizar la crianza de aves de corral para:

* Mejorar la **producción de huevos orgánicos**
* Garantizar el **bienestar animal** mediante control de condiciones ambientales
* Reducir la supervisión manual, dado que los granjeros **pasan el día en el campo**
* Contar con **alertas automáticas** al celular ante situaciones críticas
* Operar de forma **autónoma y local** incluso sin conexión a internet (alertas offline)

\---

## ⚙️ Módulos del Sistema

### 💡 Módulo 1 — Control de Iluminación

Simula el ciclo solar para maximizar la postura de huevos.

* Enciende LEDs automáticamente para garantizar **14 horas de luz diarias**
* Visualización del ciclo día/noche con barra de progreso
* Control manual de override desde la interfaz web
* Estado en tiempo real: `Ciclo Solar Activo` / `Noche`

### 🌡️ Módulo 2 — Monitoreo Térmico y Ventilación

Control reactivo de temperatura ambiente.

|Condición|Acción Automática|
|-|-|
|Temperatura > 26 °C|Activar extractores de aire|
|Temperatura < 10 °C|Activar sistema de calefacción|
|10 °C – 26 °C|Sin acción — rango óptimo|

* Slider interactivo para simular variaciones de temperatura (demo/prototipo)
* Indicadores visuales de estado para cada actuador
* Integración con sistema de alarmas (semáforo verde/amarillo/rojo)

### 🌾 Módulo 3 — Alimentación y Agua

Monitoreo de insumos críticos.

* **Sensor ultrasónico** simulado para medir nivel de alimento en silos (Silo A y Silo B)
* **Sensor de flujo/presión** para sistema de agua por goteo
* Alertas preventivas cuando el silo baja del 20%
* Estadísticas de consumo diario y proyección de reposición

### 🔔 Módulo 4 — Sistema de Alarmas (Semáforo)

Sistema de notificaciones con tres niveles de prioridad.

|Color|Significado|Acción|
|-|-|-|
|🟢 Verde|Todo OK — operación normal|Sin intervención|
|🟡 Amarillo|Aviso preventivo|Atención próxima|
|🔴 Rojo|Falla crítica|Revisión urgente|

* Simulación de notificación push al celular del granjero
* Log de eventos con timestamp
* Integrado con todos los demás módulos

\---

## 🏗️ Arquitectura General

```
┌─────────────────────────────────────────────────────┐
│                   SENSORES (Entrada)                │
│  Temp/Humedad · Ultrasónico · Flujo · Luminosidad  │
└────────────────────┬────────────────────────────────┘
                     │ señal analógica/digital
                     ▼
┌─────────────────────────────────────────────────────┐
│             MICROCONTROLADOR (ESP32)                │
│         Lógica de control · Umbrales · MQTT         │
└────────────────────┬────────────────────────────────┘
                     │ WiFi / Serial
          ┌──────────┴──────────┐
          ▼                     ▼
┌──────────────────┐   ┌─────────────────────┐
│  INTERFAZ WEB    │   │  NOTIFICACIONES     │
│  Dashboard HTML  │   │  Push al celular    │
│  Mockup Digital  │   │  Alertas offline    │
└──────────────────┘   └─────────────────────┘
          │
          ▼
┌──────────────────────────────────────────────────────┐
│                   ACTUADORES (Salida)               │
│  LEDs · Extractores · Calefacción · Válvulas agua   │
└──────────────────────────────────────────────────────┘
```

**Flujo de datos (DFD resumido):**  
`Sensor → ESP32 → Evaluación de umbral → Actuador + Log → Interfaz web + Alerta`

\---

## 📁 Estructura del Repositorio

```
smartfarm-gallinero/
│
├── 📄 README.md
│
├── 📂 etapa1-requisitos/
│   ├── PRD\_GallineroSmart\_v1.0.pdf     # Documento de Requerimientos firmado
│   └── acta\_reunion\_cliente.md         # Notas de la reunión de alineación
│
├── 📂 etapa2-diseno/
│   ├── gallinero\_smart\_mockup.html     # Mockup digital interactivo (interfaz agrícola)
│   ├── gallinero\_industrial.html       # Mockup vista industrial / SCADA
│   ├── DFD\_sensor\_actuador.png         # Diagrama de flujo de datos
│   └── aprobacion\_cliente.png          # Captura de firma digital del cliente
│
├── 📂 etapa3-prototipo/
│   ├── firmware/
│   │   ├── main.ino                    # Código fuente ESP32/Arduino
│   │   └── config.h                    # Configuración de pines y umbrales
│   ├── simulacion/
│   │   └── wokwi\_link.txt              # Enlace al circuito en Wokwi
│   └── frontend/
│       ├── index.html                  # Interfaz web del MVP
│       ├── style.css
│       └── app.js
│
└── 📂 docs/
    ├── arquitectura.md
    └── manual\_usuario.md
```

\---

## 📦 Entregables por Etapa

### Etapa 1 — Investigación y Requisitos ✅

> \*"El Contrato"\*

La reunión de alineación cliente-desarrollador permitió definir el alcance del sistema. El **Grupo Cliente** (cooperativa) explicó la operativa diaria de la granja; el **Grupo Desarrollador** tomó notas, planteó preguntas sobre límites del sistema y propuso el alcance final.

**Entregable:** Documento de Requerimientos (PRD) de 2 páginas, firmado por ambos grupos.

|#|Requerimiento Funcional|Prioridad|
|-|-|-|
|RF-01|El sistema debe encender/apagar luces LED automáticamente para garantizar 14 horas de luz diaria|Alta|
|RF-02|El sistema debe activar extractores cuando la temperatura supere 26 °C y calefacción cuando baje de 10 °C|Alta|
|RF-03|El sistema debe medir el nivel de alimento en el silo y alertar cuando quede menos del 20%|Alta|
|RF-04|El sistema debe reiniciarse automáticamente ante una falla de energía|Media|
|RF-05|El sistema debe considerar el pronóstico meteorológico para ajustar ventilación preventiva|Baja|

|#|Requerimiento No Funcional|Descripción|
|-|-|-|
|RNF-01|Resistencia de materiales|Todos los componentes expuestos deben ser IP54 o superior (polvo y humedad)|
|RNF-02|Alertas offline|El sistema debe poder enviar notificaciones SMS cuando no haya conexión WiFi|

\---

### Etapa 2 — Diseño Rápido e Interfaz ✅

> \*"El Plano"\*

Antes de escribir una línea de código del MVP, el equipo diseñó las pantallas y el flujo de datos. El cliente revisó y **firmó digitalmente** la aprobación del diseño.

**Entregables:**

* **Mockup Digital Interactivo** — `gallinero\_smart\_mockup.html`  
Interfaz web con temática agrícola (fondo crema, tipografía DM Sans), contiene:

  * Dashboard principal con métricas en tiempo real
  * Pantalla de alarma semáforo (verde/amarillo/rojo) con simulación de push
  * Control de temperatura con slider interactivo y actuadores visuales
  * Módulo de iluminación con ciclo solar
  * Módulo de alimentos y agua con barras de nivel
  * Estadísticas y gráficos de producción
  * Pantalla de aprobación con **firma digital del cliente** (canvas)
* **Vista Industrial / SCADA** — `gallinero\_industrial.html`  
Interfaz de monitoreo estilo panel de control (fondo oscuro, fuente IBM Plex Mono), con canvas de nave 2D animada, HUD superpuesto y sidebar de parámetros en tiempo real.
* **DFD Sensor → Actuador** — Diagrama validado por el cliente.

**Pantallas aprobadas:**

|Pantalla|Req. vinculado|Estado|
|-|-|-|
|Dashboard principal|RF-01|✅ Aprobado|
|Alarma semáforo|RF-02|✅ Aprobado|
|Temperatura / Ventilación|RF-09|✅ Aprobado|
|Iluminación ciclo solar|RF-08|✅ Aprobado|
|Alimentos y Agua|RF-07|✅ Aprobado|
|Estadísticas|RF-03|✅ Aprobado|
|DFD Sensor→Actuador|Arquitectura|✅ Aprobado|
|Pronóstico meteorológico|RF-05|✅ Aprobado|
|Reinicio automático|RF-04|✅ Aprobado|

\---

### Etapa 3 — Construcción del Prototipo 🚧

> \*"El Código"\*

**MVP funcional** que demuestra la reactividad del sistema: al modificar una variable simulada (ej. subir la temperatura), el actuador correspondiente (LED/extractor) reacciona en tiempo real.

* Simulación en **Wokwi** con ESP32
* Frontend web conectado por WebSocket o Serial
* Ver enlace en `etapa3-prototipo/simulacion/wokwi\_link.txt`

\---

## 🖥️ Interfaces Desarrolladas

### Interfaz Agrícola (Mockup Principal)

Acceso: abrir `gallinero\_smart\_mockup.html` en cualquier navegador moderno.

```
Credenciales de demo:
  Usuario: admin
  Contraseña: 1234
```

Navegación por pestañas:

* **Dashboard** — Vista general del sistema
* **Alarma** — Semáforo de estado con simulación push
* **Temperatura** — Control térmico con slider de prueba
* **Iluminación** — Ciclo solar y control LED
* **Alimentos** — Niveles de silo y agua
* **Estadísticas** — Producción de huevos y consumo
* **Aprobación** — Firma digital del cliente (Etapa 2)

### Interfaz Industrial / SCADA

Acceso: abrir `gallinero\_industrial.html` en cualquier navegador moderno.

* Canvas 2D animado de la nave del gallinero
* HUD con chips de estado superpuestos
* Sidebar de control de parámetros en tiempo real
* Tema oscuro estilo GitHub/terminal

\---

## 📐 Requerimientos del Sistema

**Para ejecutar los mockups HTML:**

* Navegador web moderno (Chrome 90+, Firefox 88+, Edge 90+)
* No requiere servidor — apertura directa de archivo `.html`
* Conexión a internet solo para cargar fuentes de Google Fonts (opcional)

**Para el firmware (Etapa 3):**

* Placa ESP32 o Arduino Uno/Nano
* IDE Arduino 2.x o PlatformIO
* Librería `DHT sensor library` (temperatura/humedad)
* Librería `NewPing` (sensor ultrasónico HC-SR04)
* Acceso a Wokwi para simulación online

\---

## 🛠️ Stack Tecnológico

|Capa|Tecnología|
|-|-|
|Microcontrolador|ESP32 / Arduino|
|Simulación de circuitos|Wokwi|
|Frontend web|HTML5 · CSS3 · JavaScript vanilla|
|Tipografías|DM Sans · DM Mono · IBM Plex Sans · IBM Plex Mono · Bebas Neue|
|Iconos|Tabler Icons (CDN)|
|Gráficos|Canvas API (nave industrial)|
|Comunicación|WebSocket / MQTT (planificado)|
|Notificaciones|Push API / SMS fallback|

\---

## 🚀 Cómo Ejecutar el Proyecto

### 1\. Clonar el repositorio

```bash
git clone https://github.com/tu-usuario/smartfarm-gallinero.git
cd smartfarm-gallinero
```

### 2\. Abrir los mockups

```bash
# Mockup interfaz agrícola
open etapa2-diseno/gallinero\_smart\_mockup.html

# Mockup vista industrial
open etapa2-diseno/gallinero\_industrial.html
```

O simplemente hacer doble clic en los archivos `.html` desde el explorador de archivos.

### 3\. Simular el prototipo en Wokwi

1. Ir al enlace en `etapa3-prototipo/simulacion/wokwi\_link.txt`
2. Presionar **▶ Start Simulation**
3. Modificar el valor del sensor de temperatura en el simulador
4. Observar cómo el extractor o calefactor reacciona automáticamente

### 4\. Cargar firmware en hardware físico (opcional)

```bash
# Con Arduino IDE:
# 1. Abrir etapa3-prototipo/firmware/main.ino
# 2. Seleccionar placa: ESP32 Dev Module (o Arduino Uno)
# 3. Configurar pines en config.h según tu circuito
# 4. Upload
```

\---

## 🔄 Dinámica de Trabajo (Espejo Cruzado)

Este proyecto sigue la metodología de **Intercambio de Roles** definida en el enunciado académico:

```
CICLO 1
┌──────────────┐              ┌──────────────┐
│   GRUPO A    │   diseña →   │   GRUPO B    │
│ Desarrollador│              │   Cliente    │
│              │ ← feedback   │  (Granjero)  │
└──────────────┘              └──────────────┘

CICLO 2 (sub-módulo diferente)
┌──────────────┐              ┌──────────────┐
│   GRUPO B    │   diseña →   │   GRUPO A    │
│ Desarrollador│              │   Cliente    │
│              │ ← feedback   │  (Granjero)  │
└──────────────┘              └──────────────┘
```

**Regla Áurea:** ningún grupo puede desarrollar el módulo que ellos mismos propusieron como clientes. El objetivo es practicar escucha activa, negociación de requerimientos, documentación técnica y recepción de feedback crítico.

<div align="center">

**GallineroSmart · SmartFarm v1.0**  
Proyecto académico — Automatización IoT · Cooperativa Avícola Local

🐔 *"Un gallinero feliz es un gallinero productivo"*

</div>

