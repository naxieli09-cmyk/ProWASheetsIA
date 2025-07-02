# Plan de Trabajo: Chatbot Dinámico con Google Sheets y IA (Groq)

## Versión 2.0 (Actualizado al Final del Proyecto)

Este documento describe la arquitectura y el plan de trabajo implementado para transformar un chatbot de WhatsApp en una plataforma dinámica, gestionada por Google Sheets y potenciada por inteligencia artificial.

---

### Arquitectura Final Implementada

El sistema se compone de cuatro módulos principales que trabajan en conjunto:

1.  **`src/app.js` (El Orquestador):**
    *   Es el corazón del bot. Utiliza `EVENTS.WELCOME` para capturar **todos** los mensajes entrantes.
    *   Su lógica principal es simple: primero intenta encontrar una respuesta predefinida en Google Sheets.
    *   Si no encuentra una coincidencia, delega la responsabilidad de responder al módulo de IA.
    *   Integra el sistema de historial para todas las conversaciones (tanto flujos predefinidos como respuestas de IA).

2.  **`src/sheets.js` (El Gestor de Datos):**
    *   Se conecta de forma segura a la API de Google Sheets usando las credenciales del archivo `.env`.
    *   Implementa un **sistema de caché de 5 minutos** para leer los datos de forma eficiente, garantizando respuestas rápidas y fiabilidad.
    *   Provee dos funciones: una para leer la hoja `Flujos` y otra para leer la hoja `IA_Prompts`.

3.  **`src/ai-chat.js` (El Cerebro de IA):**
    *   Se conecta a la API de Groq usando la API Key del archivo `.env`.
    *   Lee la configuración (el prompt del sistema y los parámetros como `temperature`) desde la hoja `IA_Prompts`.
    *   Integra el historial de conversaciones para proporcionar contexto a la IA y generar respuestas más coherentes.
    *   Construye y envía la petición a la IA para generar una respuesta inteligente cuando es requerido por el orquestador.

4.  **`src/chat-history.js` (El Gestor de Memoria):**
    *   Almacena automáticamente todas las conversaciones en archivos JSON individuales por contacto.
    *   Proporciona contexto histórico a la IA para respuestas más coherentes y personalizadas.
    *   Implementa limpieza automática de historiales antiguos para optimizar el rendimiento.
    *   Genera estadísticas de uso y gestiona la memoria del sistema de forma eficiente.

---

### Configuración del Entorno

*   **Gestor de Paquetes:** El proyecto utiliza **`pnpm`**. Todas las dependencias deben instalarse con `pnpm install`.
*   **Variables de Entorno (`.env`):**
    *   `GOOGLE_APPLICATION_CREDENTIALS_JSON`: Contiene el JSON de credenciales de Google en **una sola línea**.
    *   `GROQ_API_KEY`: La clave de API para el servicio de Groq.
    *   `SHEET_ID`: El ID de la hoja de cálculo de Google.

---

### Estructura de Google Sheets

#### Hoja 1: `Flujos`

Define las respuestas automáticas basadas en palabras clave.

*   **Columna A (`addKeyword`):** La palabra o frase que activa la respuesta.
*   **Columna B (`addAnswer`):** El texto que responderá el bot.
*   **Columna C (`media`):** (Opcional) La URL de un archivo de imagen o video para adjuntar.

#### Hoja 2: `IA_Prompts`

Configura la personalidad y el comportamiento del motor de inteligencia artificial.

*   **Columna A (`Prompt`):** En la celda `A2` se coloca el **`system_prompt`** principal, que define quién es el bot y cómo debe comportarse.
*   **Columna B (`Parámetro`):** El nombre del parámetro técnico a configurar (ej. `temperature`, `max_tokens`).
*   **Columna C (`Valor`):** El valor para dicho parámetro.

---

### Fases del Proyecto (Implementadas)

1.  **Fase 0: Configuración Inicial:**
    *   Se instalaron las dependencias (`googleapis`, `groq-sdk`, `dotenv`) usando `pnpm`.
    *   Se configuró el archivo `.env` para un manejo seguro de las credenciales.

2.  **Fase 1: Integración con Google Sheets:**
    *   Se implementó la lectura dinámica de la hoja `Flujos`.
    *   Se desarrolló y depuró la lógica para buscar palabras clave y responder con texto y/o media.
    *   Se estableció la arquitectura de caché para optimizar el rendimiento.

3.  **Fase 2: Integración de IA con Groq:**
    *   Se implementó la lectura de la hoja `IA_Prompts` para una configuración flexible.
    *   Se creó el módulo `ai-chat.js` para comunicarse con la API de Groq.
    *   Se integró la lógica en `app.js` para derivar a la IA cuando no se encuentra una palabra clave.

4.  **Fase 3: Sistema de Historial de Chat:**
    *   Se implementó un sistema completo de historial de conversaciones en formato JSON.
    *   Se creó el módulo `src/chat-history.js` para gestionar el almacenamiento y recuperación del historial.
    *   Se integró el historial en el motor de IA para proporcionar contexto de conversaciones anteriores.
    *   Se añadió limpieza automática de historiales antiguos para optimizar el rendimiento.
    *   Se implementaron estadísticas de uso y gestión de memoria.

5.  **Fase 4: Optimización y Documentación:**
    *   Se refactorizó y limpió el código en todos los módulos.
    *   Se añadió documentación detallada en `app.js`, `sheets.js`, `ai-chat.js` y `chat-history.js` para explicar su funcionamiento.
    *   Se creó documentación específica del sistema de historial en `HISTORIAL_CHAT.md`.
    *   Se actualizó este mismo `PLAN_DE_TRABAJO.md` para que sirva como la documentación final y fiel del proyecto.