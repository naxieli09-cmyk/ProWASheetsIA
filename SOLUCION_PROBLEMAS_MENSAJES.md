# Solución de Problemas - Mensajes Programados

## Problema Identificado

El sistema de mensajes programados no está enviando mensajes al destinatario.

## Diagnóstico Realizado

### 1. Problema con Credenciales (RESUELTO)
- **Problema**: Las credenciales de Google Sheets estaban en formato multilínea
- **Solución**: Se convirtieron a formato JSON de una sola línea en el archivo `.env`

### 2. Problema con Filtrado de Estados (RESUELTO)
- **Problema**: Los mensajes con estado `null` no se consideraban "pendientes"
- **Solución**: Se actualizó la lógica para considerar pendientes los mensajes con estado `null`, `undefined`, vacío o "pendiente"

### 3. Problema con Formatos de Fecha (RESUELTO)
- **Problema**: El sistema solo manejaba formato `YYYY-MM-DD` pero Google Sheets tenía `DD/MM/YYYY`
- **Solución**: Se implementó normalización automática de fechas para ambos formatos

### 4. Problema con Tolerancia de Tiempo (RESUELTO)
- **Problema**: Requería coincidencia exacta al minuto
- **Solución**: Se implementó tolerancia de ±2 minutos

### 5. Error en Método sendMessage (RESUELTO)
- **Problema**: `TypeError: Cannot read properties of undefined (reading 'options')`
- **Causa**: El provider Baileys espera siempre un objeto options como tercer parámetro
- **Solución**: Se modificó para pasar siempre `{ media: mediaUrl || null }`

## Estado Actual del Sistema

### Mensajes en Google Sheets:
1. **Mensaje 1**: Fecha `2025-07-29`, Hora `12:12` - No se envía (fecha pasada)
2. **Mensaje 2**: Fecha `29/07/2025`, Hora `12:05` - No se envía (fecha futura)
3. **Mensaje 3**: Fecha `30/07/2025`, Hora `null` - No se envía (hora inválida)

### Funcionalidades Implementadas:
- ✅ Conexión a Google Sheets
- ✅ Filtrado de mensajes pendientes
- ✅ Normalización de formatos de fecha
- ✅ Tolerancia de tiempo (±2 minutos)
- ✅ Formateo de números de teléfono
- ✅ Manejo de errores
- ✅ Logging detallado
- ✅ API endpoints para monitoreo

## Cómo Probar el Sistema

### Opción 1: Mensaje para Hoy
1. Ejecutar: `node test-message-today.js`
2. Seguir las instrucciones para agregar un mensaje en Google Sheets
3. Ejecutar el bot y esperar

### Opción 2: Cambiar Fecha de Mensaje Existente
1. Ir a Google Sheets
2. Cambiar la fecha del mensaje 2 a: `30/07/2025` (hoy)
3. Cambiar la hora a una hora cercana (ej: `07:10`)
4. Ejecutar el bot

### Opción 3: Usar Formato YYYY-MM-DD
1. Cambiar fecha a: `2025-07-30`
2. Establecer hora cercana
3. Ejecutar el bot

## Comandos de Depuración

```bash
# Ver datos de Google Sheets
node debug-scheduled.js

# Probar sistema completo
node test-scheduled-system.js

# Generar mensaje de prueba
node test-message-today.js

# Ejecutar bot normal
npm start
```

## Logs Esperados

Cuando un mensaje se envía correctamente:
```
✅ Mensaje programado para envío: {
  fecha: '30/07/2025',
  hora: '07:10',
  phone: '573017474717',
  diferencia_minutos: 1
}
📱 Enviando mensaje programado a 573017474717
✅ Mensaje enviado exitosamente
📝 Estado actualizado a 'Enviado'
```

## Verificaciones Adicionales

1. **Verificar que el bot esté conectado a WhatsApp**
2. **Verificar que el número de teléfono sea válido**
3. **Verificar que Google Sheets tenga permisos de escritura**
4. **Verificar que la zona horaria sea correcta**

## Próximos Pasos

1. Crear un mensaje de prueba para hoy
2. Verificar que el bot esté ejecutándose
3. Monitorear los logs en tiempo real
4. Confirmar que el mensaje se envíe y el estado se actualice

## Contacto

Si el problema persiste después de seguir estos pasos, revisar:
- Logs del sistema
- Conexión a Google Sheets
- Estado de WhatsApp
- Configuración de zona horaria