# Diseño de la Arquitectura de Software Modular (Amalia Gamma Robot)

## 1. Principios de Diseño

La arquitectura de software para el robot (basado en Aloha Mini) debe ser modular, escalable y cumplir con los requisitos de autonomía, actualización y entrenamiento flexible. Se utilizará **ROS 2** como *middleware* principal para la comunicación entre los componentes del robot y el *API Gateway* de Amalia Gamma.

## 2. Estructura Modular de Software

La arquitectura se divide en tres capas principales, alineadas con el concepto MindOn (Percepción, Cognición, Actuación) y extendidas para la modularidad del entrenamiento.

### 2.1. Capa de Borde (Robot - Jetson Orin Nano)

Esta capa se ejecuta directamente en el robot y se encarga de la interacción física y la comunicación de bajo nivel.

| Módulo | Función | Tecnología Clave |
| :--- | :--- | :--- |
| **ROS 2 Core** | *Middleware* de comunicación. | ROS 2 (DDS) |
| **Sensor Fusion Node** | Recopila datos de cámara, LiDAR (si se añade), *encoders* y micrófono. | ROS 2, OpenCV |
| **Conversational Node** | *Wake Word Detection* y STT ligero (Edge AI). | PyTorch Mobile / ONNX Runtime |
| **Control Node** | Control de bajo nivel de los actuadores (brazos, base móvil). | SDK de Aloha Mini, ROS 2 Control |
| **IoT 868 MHz Node** | Gestión de la comunicación LoRa (modo reposo/activación). | Python/C++ con librería LoRaWAN |
| **Health & Power Node** | Monitoreo de batería y ejecución de la secuencia de recarga autónoma. | ROS 2, Python |

### 2.2. Capa de Nube (Hosting del Usuario - CorticalLabs NPU)

Esta capa es el "cerebro" de Amalia Gamma, donde reside la inteligencia de alto nivel.

| Módulo | Función | Tecnología Clave |
| :--- | :--- | :--- |
| **API Gateway (Fase 8)** | Punto de entrada seguro. Enrutamiento de comandos y gestión de *failover*. | FastAPI, Python |
| **LLM Engine (Kimi K2)** | Razonamiento, planificación de tareas de alto nivel y generación de respuestas conversacionales. | CorticalLabs NPU |
| **RL Agent (SIMA 2)** | Generación de políticas de acción robótica (Act-1, Tareas Domésticas). | SIMA 2 (Simulado), Python |
| **Memory Manager (Mem0)** | Memoria a largo plazo y gestión de contexto. | Mem0 |

## 3. Sistema de Entrenamiento Modular y Autónomo

El requisito de que el entrenamiento sea flexible (datasets, juegos, mundos virtuales, videos, fotos, archivos) y que el robot se actualice de forma autónoma requiere un **Módulo de Gestión de Contenido de Entrenamiento (MCCE)**.

### 3.1. Módulo de Gestión de Contenido de Entrenamiento (MCCE)

Este módulo, que se ejecutará en el *hosting* del usuario, gestiona la ingesta y el pre-procesamiento de cualquier formato de entrenamiento.

| Sub-Módulo | Función | Tecnología Clave |
| :--- | :--- | :--- |
| **Ingesta Flexible** | Acepta cualquier formato (video, imagen, texto, archivos de instrucciones, mundos virtuales). | Python (FFmpeg, OpenCV, PDFMiner) |
| **Análisis Autónomo** | **Define cómo analiza el contenido:** Utiliza el LLM Kimi K2 para analizar el contenido y generar *prompts* de entrenamiento estructurados para el agente RL (SIMA 2). | Kimi K2 (CorticalLabs NPU) |
| **Generación de Escenarios** | Convierte el contenido analizado en escenarios de entrenamiento virtual para **Britetrainer** (simulado en Omniverse). | Python, API de Omniverse (USD) |
| **Actualización Autónoma** | Tras recibir nuevas órdenes que no sabe ejecutar, el MCCE busca contenido relevante, genera un nuevo escenario de entrenamiento y programa un ciclo de RL. | Python, Cron Job (para programación) |

### 3.2. Ciclo de Entrenamiento (RL)

1.  **Input:** Nuevo contenido o tarea no resuelta.
2.  **MCCE:** Analiza el contenido y genera un escenario virtual en **Britetrainer (Omniverse)**.
3.  **RL:** El agente **SIMA 2** entrena en el entorno virtual.
4.  **Output:** Una nueva política de acción optimizada.
5.  **Despliegue:** La nueva política se integra automáticamente en el *API Gateway* para su uso en el robot real.

## 4. Requisitos de Autonomía y Necesidades

El robot debe ser autónomo en sus necesidades (recarga) y capacidad de reposo/actuación.

| Necesidad | Módulo de Control | Lógica de Funcionamiento |
| :--- | :--- | :--- |
| **Recarga** | Health & Power Node (Borde) | Si la batería < 20%, el nodo envía una señal al LLM (Kimi K2) para que genere una secuencia de navegación hacia la base de carga. |
| **Reposo** | IoT 868 MHz Node (Borde) | El robot entra en modo de bajo consumo (solo el módulo LoRa y el Health Node están activos). |
| **Actuación** | IoT 868 MHz Node (Borde) | Una señal de 868 MHz (ej. un sensor de movimiento) o un comando del *API Gateway* despierta al robot para la acción. |

---
## Referencias

[1] Sunday Robotics ACT-1: A Robot Foundation Model Trained on Zero Robot Data, Sunday.ai.
[2] ROS 2 (Robot Operating System), Open Robotics.
[3] LoRaWAN Specification, LoRa Alliance.
