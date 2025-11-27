# Plan de Entrenamiento de IA y Manual de Uso (Robot Aloha Mini - Amalia Gamma)

## 1. Plan de Entrenamiento de IA (Britetrainer y Autonomía)

El plan de entrenamiento se basa en el concepto de **Britetrainer** (Entrenamiento en Zona Virtual) y la capacidad de Amalia Gamma para procesar datos de entrenamiento de cualquier formato.

### 1.1. Flujo de Entrenamiento Modular

El entrenamiento se realiza en un ciclo continuo de **Sim-to-Real** (Simulación a Realidad), aprovechando la integración con **NVIDIA Omniverse** (simulando Britetrainer) y el agente **SIMA 2** (el motor de RL).

| Paso | Módulo Involucrado | Descripción |
| :--- | :--- | :--- |
| **1. Ingesta Flexible** | MCCE (Módulo de Gestión de Contenido de Entrenamiento) | El usuario sube cualquier formato de entrenamiento (video, dataset, juego, archivo de instrucciones). |
| **2. Análisis Autónomo** | MCCE + Kimi K2 (CorticalLabs NPU) | Kimi K2 analiza el contenido y genera una **descripción de tarea estructurada** y una **función de recompensa** para el RL. |
| **3. Generación de Escenario** | MCCE + Omniverse API | El MCCE crea automáticamente un entorno virtual en Omniverse (Britetrainer) que replica la tarea y el entorno del robot. |
| **4. Entrenamiento RL** | SIMA 2 Agent (Hosting) | SIMA 2 entrena en el entorno virtual, optimizando la política de acción para la tarea. |
| **5. Despliegue y Validación** | API Gateway (Fase 8) | La política de acción optimizada se despliega en el *API Gateway* y se prueba en el robot real (Aloha Mini). |

### 1.2. Estrategias de Entrenamiento por Formato

| Formato de Entrada | Estrategia de Análisis Autónomo (Kimi K2) | Aplicación en RL |
| :--- | :--- | :--- |
| **Videos/Fotos** | **Visión-a-Lenguaje (V2L):** Kimi K2 describe la secuencia de acciones, los objetos y el resultado final. | Se utiliza para *Imitation Learning* (IL) inicial, seguido de *Reinforcement Learning* (RL) para refinar la destreza. |
| **Juegos/Mundos Virtuales** | **Análisis de Reglas:** Kimi K2 extrae las reglas del juego y los objetivos de alto nivel. | Entrenamiento directo en el entorno virtual (Britetrainer/Omniverse) con la función de recompensa generada. |
| **Archivos con Instrucciones** | **NLP Avanzado:** Kimi K2 descompone las instrucciones en sub-tareas secuenciales y las mapea a acciones robóticas. | Se utiliza para generar la secuencia de *prompts* de entrenamiento guiado para SIMA 2. |
| **Datasets (Tabulares/Sensores)** | **Análisis de Correlación:** Kimi K2 identifica patrones de movimiento y correlaciones entre el estado del sensor y la acción exitosa. | Se utiliza para el *fine-tuning* de la política de control de bajo nivel. |

## 2. Manual de Uso y Reentrenamiento (Minimalista)

El sistema está diseñado para ser utilizado por el usuario final sin necesidad de conocimientos de programación.

### 2.1. Modo de Uso (Conversacionalidad)

El robot utiliza la conversacionalidad para recibir órdenes y reportar su estado.

1.  **Activación:** El usuario dice la palabra clave (ej. "Amalia") o el robot es activado por un evento IoT (868 MHz).
2.  **Orden:** El usuario da una orden de alto nivel (ej. "Amalia, necesito que me traigas un vaso de agua de la cocina").
3.  **Ejecución:** El *API Gateway* envía la orden a Kimi K2 (CorticalLabs NPU), que la traduce a una política de acción de SIMA 2. El robot ejecuta la acción.
4.  **Reporte:** El robot reporta verbalmente la finalización de la tarea o cualquier problema encontrado.

### 2.2. Proceso de Reentrenamiento (Actualización Autónoma)

El sistema de Amalia está diseñado para actualizarse de forma autónoma cuando se enfrenta a una tarea que no conoce.

| Escenario | Acción del Robot | Intervención del Usuario |
| :--- | :--- | :--- |
| **Tarea Desconocida** | El robot informa: "No tengo una política de acción para 'X'. ¿Desea que aprenda esta tarea?" | El usuario confirma y proporciona el contenido de entrenamiento (video, instrucciones, etc.) a través de la interfaz web del MCCE. |
| **Actualización de Software** | El robot informa: "He detectado una nueva política de acción optimizada para 'X'. Permítame un momento para actualizar mi software de control." | **Ninguna.** El MCCE gestiona la actualización de la política de acción de forma automática y segura. |
| **Mantenimiento/Recarga** | El robot informa: "Mi nivel de batería es bajo. Me dirigiré a la base de carga." | **Ninguna.** El robot gestiona sus necesidades de forma autónoma. |

## 3. Desarrollo de Montaje Minimalista

El montaje minimalista se centra en la integración eficiente de los componentes seleccionados (Aloha Mini, Jetson Orin Nano, Módulo LoRa).

1.  **Estructura Base:** Utilizar la estructura *open-source* de Aloha Mini (impresión 3D de bajo costo).
2.  **Integración del Chip:** La **Jetson Orin Nano** se montará en la base móvil del robot, cerca de la batería, para un cableado corto y eficiente.
3.  **Módulo LoRa:** El módulo de 868 MHz se conectará a la Jetson a través de un puerto GPIO o USB, con la antena montada en la parte superior del robot para una mejor cobertura.
4.  **Cableado:** Uso de conectores JST y cables de silicona flexibles para minimizar el desorden y permitir el movimiento sin restricciones.

---
## Referencias

[1] Aloha Mini: $600 Open-Source Home Robot, Reddit.
[2] NVIDIA Omniverse (Isaac Sim) Documentation.
[3] Google DeepMind SIMA 2 Research Paper.
