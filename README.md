# Proyecto Amalia Gamma: Democratización de la Robótica e IA

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Status: Design Complete](https://img.shields.io/badge/Status-Design%20Complete-blue.svg)](./plan_de_implementacion_detallado.md)
[![Powered by Amalia AI](https://img.shields.io/badge/Powered%20by-Amalia%20AI-red.svg)](http://torete.net/terminator)

## 🚀 ¡Únete a la Revolución!


![t2000-terminator](https://github.com/user-attachments/assets/acbd1f58-468a-47c2-88d5-91581cfcc08d)




**Amalia Gamma** es un proyecto *Open Source* diseñado para democratizar la robótica avanzada y la Inteligencia Artificial encarnada. Nuestro objetivo es crear un robot humanoide minimalista, modular y asequible, capaz de aprender de forma autónoma a través de *Reinforcement Learning* (RL) en entornos virtuales (Britetrainer/Omniverse), utilizando una arquitectura de software agnóstica al hardware.

**Este proyecto es una invitación abierta a ingenieros, desarrolladores y entusiastas para colaborar en la próxima generación de robótica doméstica.**

---

## 🤖 Plataforma Robótica Base

Hemos seleccionado la plataforma **Aloha Mini** [1] como base por su diseño *open-source*, bajo costo y capacidad de manipulación de doble brazo, alineándose con el espíritu de proyectos como **Act-1** de Sunday Robotics.

| Característica | Aloha Mini | Enlaces de Interés |
| :--- | :--- | :--- |
| **Tipo** | Manipulador Móvil de Doble Brazo | [Aloha Mini GitHub Repository (Simulado)](https://github.com/aloha-mini) |
| **Costo Estimado** | ~$1670 USD (BOM preliminar) | [Requisitos de Hardware y BOM](./hardware_requirements_and_bom.md) |
| **Filosofía** | Minimalista, Open Source, Enfocado en Tareas | [Robot Open Source (Video de Referencia)](https://youtu.be/pm9Lvj_OyhY) |

## 🧠 Arquitectura de Software Agnóstica (Amalia Gamma)

El software de Amalia Gamma está diseñado para ser completamente independiente del chip de control del robot, gracias a la **Capa de Abstracción de Hardware (HAL)**.

### 1. Hardware Agnostico: Compatibilidad Total

El sistema de actualización y control funciona con cualquier chip que implemente la HAL.

| Chip Compatible | Tipo de Despliegue | Rol en el Robot |
| :--- | :--- | :--- |
| **NVIDIA Jetson Orin Nano** | ROS 2 / Docker (Linux) | **Recomendado:** Edge AI, Fusión de Sensores. |
| **Raspberry Pi 5** | ROS 2 (Linux) | Opción de bajo costo para control de alto nivel. |
| **Arduino (Due/Mega)** | Firmware (C/C++) | Control de bajo nivel de motores y actuadores. |
| **Mini PC / Qualcomm** | ROS 2 / Linux | Opciones de alto rendimiento y bajo consumo. |

### 2. Software Híbrido (Nube y Borde)

| Componente | Ubicación | Función Clave |
| :--- | :--- | :--- |
| **HAL** | Robot (Borde) | Abstracción de hardware y control de bajo nivel. |
| **Update Manager** | Robot (Borde) | Actualización de *firmware* y *software* agnóstica al chip. |
| **API Gateway** | Nube (Hosting) | Enlace seguro y con *failover* al cerebro de IA. |
| **LLM Engine (Kimi K2)** | Nube (**CorticalLabs NPU**) | Razonamiento, planificación y conversacionalidad. |
| **RL Agent (SIMA 2)** | Nube (Hosting) | Generación de políticas de acción robótica. |

## 🌐 Sistema de Entrenamiento Autónomo (Britetrainer)

El robot aprende continuamente a través de un ciclo de *Refourcing Learning* (RL) gestionado por el **Módulo de Gestión de Contenido de Entrenamiento (MCCE)**.

*   **Britetrainer:** El entorno de entrenamiento virtual se simula en **NVIDIA Omniverse (Isaac Sim)**.
*   **Ingesta Flexible:** El MCCE acepta cualquier formato de entrenamiento (videos, fotos, datasets, juegos, archivos de instrucciones) y utiliza el LLM Kimi K2 para **analizar el contenido de forma autónoma** y generar la función de recompensa y el escenario virtual para el agente SIMA 2.
*   **Autonomía:** El robot se actualiza automáticamente con nuevas políticas de acción y gestiona sus necesidades (ej. recarga) sin intervención humana.

## 🛠️ Plan de Implementación y Colaboración

El proyecto está listo para la fase de implementación. Invitamos a la comunidad a colaborar en las siguientes áreas:

| Fase | Área de Colaboración | Archivos Clave |
| :--- | :--- | :--- |
| **Hardware** | Diseño de adaptadores 3D para nuevos chips (Qualcomm, Mini PC) y optimización del módulo LoRa. | `hardware_requirements_and_bom.md` |
| **Software Embebido** | Implementación de la HAL para nuevos microcontroladores (ej. ESP32, micro:bit) y optimización de los nodos ROS 2. | `robot_software_dev/hardware_abstraction/` |
| **IA/RL** | Desarrollo de la interfaz real con SIMA 2 y Omniverse, y optimización de las funciones de análisis del MCCE. | `robot_training_plan_and_manual.md` |
| **Frontend** | Desarrollo de una interfaz de usuario web moderna para el MCCE (sustituyendo la UI de escritorio). | `robot_software_dev/desktop_software/control_ui.py` |

---

## 🔗 Enlaces y Documentación

*   **Web Original del Proyecto:** [http://torete.net/terminator](http://torete.net/terminator)
*   **Plan de Implementación Detallado:** [plan_de_implementacion_detallado.md](./plan_de_implementacion_detallado.md)
*   **Requisitos de Hardware y BOM:** [hardware_requirements_and_bom.md](./hardware_requirements_and_bom.md)
*   **Arquitectura de Software:** [robot_software_architecture.md](./robot_software_architecture.md)
*   **Plan de Entrenamiento y Manual de Uso:** [robot_training_plan_and_manual.md](./robot_training_plan_and_manual.md)
*   **Código Fuente del Software Agnóstico:** [robot_software_dev/](./robot_software_dev/)

---
## Referencias

[1] Aloha Mini: $600 Open-Source Home Robot, Reddit.
[2] NVIDIA Jetson Orin Nano Documentation.
[3] Google DeepMind SIMA 2 Research.
