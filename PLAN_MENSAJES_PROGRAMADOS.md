# Plan de Trabajo: Sistema de Mensajes Programados

## Análisis del Sistema Actual

### Funcionamiento Actual
El sistema actual funciona de la siguiente manera:
1. **Flujo Principal**: Se activa con `addKeyword(EVENTS.WELCOME)` que escucha todos los mensajes entrantes
2. **Google Sheets**: Lee desde la hoja "Flujos" con columnas: `addKeyword`, `addAnswer`, `media`
3. **Lógica de Activación**: Busca coincidencias de palabras clave en el mensaje del usuario
4. **Respuesta**: Envía `addAnswer` y opcionalmente `media` si hay URL
5. **Fallback**: Si no hay coincidencia, deriva a la IA (Groq)

### Estructura de Archivos
- `app.js`: Lógica principal del bot y flujos
- `sheets.js`: Servicio para conectar con Google Sheets
- `ai-chat.js`: Servicio de IA con Groq
- `chat-history.js`: Manejo del historial de conversaciones

## Objetivo del Nuevo Sistema

Crear un **sistema de mensajes programados** que:
- Se active por **fecha y hora** en lugar de palabras clave
- Tenga campos adicionales: `Fecha`, `Hora`, `phone`, `addAnswer`, `media`, `Estado`
- Envíe mensajes automáticamente cuando coincida la fecha/hora programada
- Funcione de forma independiente al flujo actual

## Plan de Implementación

### Fase 1: Extensión del Servicio de Google Sheets

#### 1.1 Modificar `sheets.js`
- Agregar método `getScheduledMessages()` para leer nueva hoja "Mensajes_Programados"
- Estructura de la nueva hoja:
  ```
  Fecha | Hora | phone | addAnswer | media | Estado
  ```
- Implementar caché similar al sistema actual
- Agregar método `updateMessageStatus()` para marcar mensajes como enviados

#### 1.2 Validaciones
- Validar formato de fecha (YYYY-MM-DD)
- Validar formato de hora (HH:MM)
- Validar formato de teléfono
- Validar estados permitidos: "Pendiente", "Enviado", "Error"

### Fase 2: Servicio de Mensajes Programados

#### 2.1 Crear `scheduled-messages.js`
- Clase `ScheduledMessageService`
- Método `checkPendingMessages()`: Verificar mensajes pendientes
- Método `sendScheduledMessage()`: Enviar mensaje programado
- Método `updateStatus()`: Actualizar estado en Google Sheets
- Manejo de errores y reintentos

#### 2.2 Lógica de Programación
- Verificar cada minuto los mensajes pendientes
- Comparar fecha/hora actual con mensajes programados
- Filtrar solo mensajes con estado "Pendiente"
- Enviar mensajes que coincidan con la fecha/hora actual

### Fase 3: Integración con el Bot Principal

#### 3.1 Modificar `app.js`
- Importar el nuevo servicio de mensajes programados
- Configurar intervalo de verificación (cada minuto)
- Integrar con el sistema de envío existente
- Mantener compatibilidad con el flujo actual

#### 3.2 API Endpoints
- Mantener endpoints existentes
- Considerar agregar endpoint para programar mensajes vía API

### Fase 4: Manejo de Estados y Logs

#### 4.1 Sistema de Estados
- "Pendiente": Mensaje programado, esperando envío
- "Enviado": Mensaje enviado exitosamente
- "Error": Error en el envío

#### 4.2 Logging
- Registrar intentos de envío
- Registrar errores y razones de fallo
- Estadísticas de mensajes programados

## Estructura de Archivos Resultante

```
src/
├── app.js                    # Bot principal (modificado)
├── sheets.js                 # Servicio Google Sheets (extendido)
├── scheduled-messages.js     # Nuevo servicio de mensajes programados
├── ai-chat.js               # Servicio IA (sin cambios)
└── chat-history.js          # Historial (sin cambios)
```

## Configuración de Google Sheets

### Nueva Hoja: "Mensajes_Programados"
```
A: Fecha (YYYY-MM-DD)
B: Hora (HH:MM)
C: phone (número con código país)
D: addAnswer (mensaje a enviar)
E: media (URL opcional)
F: Estado (Pendiente/Enviado/Error)
```

### Ejemplo de Datos
```
2024-01-15 | 09:00 | 5491234567890 | ¡Buenos días! Recordatorio de tu cita | https://example.com/image.jpg | Pendiente
2024-01-15 | 14:30 | 5491234567890 | Hora del almuerzo 🍽️ | | Pendiente
```

## Consideraciones Técnicas

### 1. Zona Horaria
- Usar zona horaria local del servidor
- Considerar configuración de zona horaria en variables de entorno

### 2. Rendimiento
- Verificación cada minuto (configurable)
- Caché de mensajes programados
- Límite de mensajes por verificación

### 3. Manejo de Errores
- Reintentos automáticos
- Logging detallado
- Notificaciones de errores críticos

### 4. Escalabilidad
- Paginación para grandes volúmenes
- Índices en Google Sheets para búsquedas eficientes

## Cronograma de Implementación

1. **Día 1**: Extensión de `sheets.js` y configuración de nueva hoja
2. **Día 2**: Desarrollo de `scheduled-messages.js`
3. **Día 3**: Integración con `app.js` y pruebas
4. **Día 4**: Testing y refinamiento
5. **Día 5**: Documentación y deployment

## Beneficios del Nuevo Sistema

- **Automatización**: Mensajes enviados sin intervención manual
- **Flexibilidad**: Programación precisa por fecha y hora
- **Escalabilidad**: Manejo de múltiples mensajes programados
- **Trazabilidad**: Estados y logs detallados
- **Compatibilidad**: No interfiere con el sistema actual

## Próximos Pasos

1. Crear la nueva hoja "Mensajes_Programados" en Google Sheets
2. Implementar las modificaciones según el plan
3. Realizar pruebas exhaustivas
4. Documentar el uso del nuevo sistema
5. Capacitar al equipo en el nuevo flujo de trabajo