# Guía de Uso: Sistema de Mensajes Programados

## Descripción General

El sistema de mensajes programados permite enviar mensajes de WhatsApp de forma automática basándose en fecha y hora programada, sin necesidad de palabras clave. Este sistema funciona de manera independiente al flujo de conversación normal del bot.

## Configuración en Google Sheets

### 1. Crear la Hoja "Mensajes_Programados"

En tu Google Sheet, crea una nueva hoja llamada exactamente: `Mensajes_Programados`

### 2. Estructura de Columnas

La hoja debe tener las siguientes columnas en la fila 1 (encabezados):

| A | B | C | D | E | F |
|---|---|---|---|---|---|
| Fecha | Hora | phone | addAnswer | media | Estado |

### 3. Descripción de Campos

- **Fecha** (Columna A): Fecha en formato YYYY-MM-DD (ej: 2024-01-15)
- **Hora** (Columna B): Hora en formato HH:MM (ej: 09:30)
- **phone** (Columna C): Número de teléfono con código de país (ej: 5491234567890)
- **addAnswer** (Columna D): Mensaje de texto a enviar
- **media** (Columna E): URL de imagen/video (opcional)
- **Estado** (Columna F): Estado del mensaje (Pendiente/Enviado/Error)

### 4. Ejemplo de Datos

```
Fecha      | Hora  | phone         | addAnswer                    | media                        | Estado
2024-01-15 | 09:00 | 5491234567890 | ¡Buenos días! Recordatorio   | https://example.com/img.jpg  | Pendiente
2024-01-15 | 14:30 | 5491234567890 | Hora del almuerzo 🍽️         |                              | Pendiente
2024-01-16 | 10:00 | 5491987654321 | Reunión en 30 minutos        |                              | Pendiente
```

## Estados de Mensajes

### Estados Disponibles:

1. **Pendiente**: Mensaje programado esperando ser enviado
2. **Enviado**: Mensaje enviado exitosamente
3. **Error**: Error en el envío del mensaje

### Notas sobre Estados:
- Los campos vacíos o con "Pendiente" se consideran mensajes pendientes
- El sistema actualiza automáticamente el estado después del envío
- Solo los mensajes "Pendiente" son procesados por el sistema

## Funcionamiento del Sistema

### 1. Verificación Automática
- El sistema verifica cada **60 segundos** si hay mensajes para enviar
- Compara la fecha/hora actual con los mensajes programados
- Envía automáticamente los mensajes que coincidan

### 2. Proceso de Envío
1. Busca mensajes con estado "Pendiente"
2. Filtra por fecha y hora actual
3. Envía el mensaje (texto + media si existe)
4. Actualiza el estado a "Enviado" o "Error"
5. Guarda el mensaje en el historial del chat

### 3. Manejo de Errores
- Si hay un error en el envío, el estado se marca como "Error"
- Los errores se registran en los logs del sistema
- Los mensajes con error no se reenvían automáticamente

## API Endpoints

El sistema incluye endpoints para monitoreo y control:

### 1. Obtener Estadísticas
```http
GET /v1/scheduled-stats
```

**Respuesta:**
```json
{
  "status": "ok",
  "stats": {
    "total": 10,
    "pendientes": 5,
    "enviados": 4,
    "errores": 1
  }
}
```

### 2. Forzar Verificación
```http
POST /v1/scheduled-check
```

**Respuesta:**
```json
{
  "status": "ok",
  "message": "Verificación forzada completada"
}
```

### 3. Reiniciar Servicio
```http
POST /v1/scheduled-restart
```

**Respuesta:**
```json
{
  "status": "ok",
  "message": "Servicio reiniciado"
}
```

## Mejores Prácticas

### 1. Formato de Números de Teléfono
- Usar código de país sin el símbolo "+"
- Ejemplo correcto: `5491234567890`
- Ejemplo incorrecto: `+54 9 11 2345-6789`

### 2. Formato de Fecha y Hora
- **Fecha**: Siempre YYYY-MM-DD
- **Hora**: Siempre HH:MM (formato 24 horas)
- Usar la zona horaria del servidor

### 3. URLs de Media
- Usar URLs públicas y accesibles
- Formatos soportados: JPG, PNG, GIF, MP4, PDF
- Verificar que la URL funcione antes de programar

### 4. Gestión de Estados
- No modificar manualmente los estados "Enviado" o "Error"
- Para reenviar un mensaje con error, cambiar el estado a "Pendiente"
- Revisar regularmente los mensajes con error

## Monitoreo y Logs

### 1. Logs del Sistema
El sistema registra automáticamente:
- Inicio y detención del programador
- Mensajes enviados exitosamente
- Errores en el envío
- Estadísticas periódicas

### 2. Verificación de Estado
- Revisar regularmente la hoja de Google Sheets
- Usar el endpoint `/v1/scheduled-stats` para monitoreo
- Verificar logs del servidor para errores

## Solución de Problemas

### 1. Mensajes No Se Envían
**Posibles causas:**
- Formato incorrecto de fecha/hora
- Número de teléfono inválido
- Estado diferente a "Pendiente"
- Error en la conexión con Google Sheets

**Solución:**
- Verificar formato de datos
- Usar endpoint `/v1/scheduled-check` para forzar verificación
- Revisar logs del servidor

### 2. Error en URLs de Media
**Posibles causas:**
- URL no accesible públicamente
- Formato de archivo no soportado
- URL expirada o inválida

**Solución:**
- Verificar que la URL funcione en el navegador
- Usar servicios de hosting confiables
- Verificar permisos de acceso

### 3. Problemas de Sincronización
**Posibles causas:**
- Caché de Google Sheets no actualizada
- Zona horaria incorrecta
- Diferencia de tiempo entre servidor y configuración

**Solución:**
- Usar endpoint `/v1/scheduled-restart` para reiniciar
- Verificar zona horaria del servidor
- Sincronizar hora del sistema

## Limitaciones

1. **Precisión de Tiempo**: Verificación cada minuto (no segundos)
2. **Zona Horaria**: Usa la zona horaria del servidor
3. **Volumen**: Recomendado máximo 100 mensajes por verificación
4. **Reintentos**: No hay reintentos automáticos para errores
5. **Historial**: Los mensajes programados se guardan en el historial normal

## Compatibilidad

El sistema de mensajes programados:
- ✅ Funciona independientemente del flujo de palabras clave
- ✅ Compatible con el sistema de IA existente
- ✅ Mantiene el historial de conversaciones
- ✅ No interfiere con mensajes manuales
- ✅ Soporta media (imágenes, videos, documentos)

## Próximas Mejoras

- [ ] Soporte para mensajes recurrentes
- [ ] Zona horaria configurable por mensaje
- [ ] Reintentos automáticos para errores
- [ ] Plantillas de mensajes
- [ ] Programación con días de la semana
- [ ] Notificaciones de errores por email/WhatsApp