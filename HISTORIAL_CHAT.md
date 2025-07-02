# 📚 Sistema de Historial de Chat

Este documento explica cómo funciona el sistema de historial de chat implementado en el bot de WhatsApp.

## 🎯 Características Principales

### ✅ Funcionalidades Implementadas

- **Guardado Automático**: Todos los mensajes se guardan automáticamente en archivos JSON
- **Contexto para IA**: La IA tiene acceso al historial de conversaciones para respuestas coherentes
- **Archivos Individuales**: Un archivo JSON por contacto para mejor rendimiento
- **Limpieza Automática**: Eliminación automática de historiales antiguos (30 días)
- **Estadísticas**: Métricas sobre el uso del historial
- **Gestión de Memoria**: Limitación de mensajes por contacto (100 máximo)

## 📁 Estructura de Archivos

```
chat_history/
├── 573017474717.json    # Historial del contacto +57 301 747 4717
├── 573505489828.json    # Historial del contacto +57 350 548 9828
└── ...
```

## 📋 Formato del Historial

Cada archivo JSON contiene:

```json
{
  "phoneNumber": "573017474717",
  "name": "Juan Pérez",
  "firstContact": "2024-01-15T08:00:00Z",
  "lastActivity": "2024-01-15T14:30:00Z",
  "messages": [
    {
      "timestamp": "2024-01-15T14:25:00Z",
      "role": "user",
      "content": "Hola, necesito ayuda con mi pedido"
    },
    {
      "timestamp": "2024-01-15T14:25:30Z",
      "role": "assistant",
      "content": "¡Hola Juan! Te ayudo con tu pedido. ¿Cuál es el número de orden?"
    }
  ],
  "context": {
    "preferences": [],
    "topics": ["soporte", "pedidos"],
    "summary": null
  }
}
```

## 🔧 Configuración

### Parámetros Configurables (en `chat-history.js`):

- **maxMessages**: 100 - Máximo de mensajes por contacto
- **retentionDays**: 30 - Días de retención del historial
- **contextMessages**: 10 - Mensajes incluidos en el contexto para la IA

## 🚀 Cómo Funciona

### 1. Guardado de Mensajes

- **Flujos Predefinidos**: Se guardan tanto el mensaje del usuario como la respuesta del bot
- **Respuestas de IA**: Se guarda el mensaje del usuario antes de consultar la IA y la respuesta después
- **Formato Automático**: Los mensajes se formatean automáticamente con timestamp y rol

### 2. Contexto para la IA

- La IA recibe los últimos 10 mensajes como contexto
- Esto permite respuestas coherentes basadas en la conversación previa
- El contexto se incluye automáticamente en cada consulta a la IA

### 3. Limpieza Automática

- Cada 24 horas se ejecuta una limpieza automática
- Se eliminan archivos de historial más antiguos que 30 días
- Se muestran estadísticas en la consola

## 📊 Estadísticas Disponibles

El sistema proporciona las siguientes métricas:

- **Total de contactos**: Número de archivos de historial
- **Contactos activos**: Contactos con actividad en las últimas 24 horas
- **Total de mensajes**: Suma de todos los mensajes guardados
- **Promedio de mensajes por contacto**: Métrica de engagement

## 🛠️ Métodos Principales

### ChatHistoryService

```javascript
// Guardar un mensaje
await chatHistoryService.saveMessage(phoneNumber, 'user', 'Hola')

// Obtener contexto para IA
const context = await chatHistoryService.getContextForAI(phoneNumber)

// Obtener resumen del historial
const summary = await chatHistoryService.getHistorySummary(phoneNumber)

// Obtener estadísticas
const stats = await chatHistoryService.getStats()

// Limpiar historiales antiguos
const deleted = await chatHistoryService.cleanOldHistories()
```

## 🔍 Logs y Monitoreo

El sistema genera logs informativos:

```
💬 Mensaje guardado para 573017474717: user
💬 Mensaje guardado para 573017474717: assistant
🧠 Contexto cargado para 573017474717: 5 mensajes
📊 Estadísticas del historial: { totalContacts: 15, activeContacts: 8, ... }
🧹 Iniciando limpieza automática del historial...
🧹 Limpieza completada. Archivos eliminados: 2
```

## 🔒 Consideraciones de Privacidad

- Los historiales se almacenan localmente en el servidor
- No se envían a servicios externos (excepto el contexto necesario para la IA)
- Los archivos antiguos se eliminan automáticamente
- Los números de teléfono se limpian para usar como nombres de archivo

## 🚨 Solución de Problemas

### Problemas Comunes:

1. **Error al guardar historial**: Verificar permisos de escritura en la carpeta `chat_history/`
2. **Contexto no se carga**: Verificar que el archivo JSON del contacto no esté corrupto
3. **Limpieza no funciona**: Verificar que el proceso tenga permisos para eliminar archivos

### Logs de Error:

```
❌ Error al guardar historial: [detalle del error]
❌ Error al limpiar historiales: [detalle del error]
❌ Error al obtener estadísticas: [detalle del error]
```

## 🔄 Mantenimiento

### Tareas Automáticas:
- ✅ Limpieza de archivos antiguos (cada 24 horas)
- ✅ Limitación de mensajes por archivo (automática)
- ✅ Creación de directorio si no existe (al iniciar)

### Tareas Manuales:
- 📋 Revisar estadísticas periódicamente
- 🔍 Monitorear logs de errores
- 💾 Hacer backup de la carpeta `chat_history/` si es necesario

## 📈 Beneficios del Sistema

1. **Respuestas Coherentes**: La IA mantiene el contexto de conversaciones anteriores
2. **Mejor Experiencia**: Los usuarios no necesitan repetir información
3. **Análisis de Uso**: Estadísticas para entender el comportamiento de los usuarios
4. **Rendimiento Optimizado**: Archivos individuales evitan bloqueos
5. **Gestión Automática**: Limpieza y mantenimiento sin intervención manual

---

*Sistema implementado el [fecha] - Versión 1.0*