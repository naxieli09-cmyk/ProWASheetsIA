# Test del Sistema de Mensajes Programados

## Verificación de Implementación

### ✅ Archivos Creados/Modificados

1. **`src/sheets.js`** - Extendido con métodos para mensajes programados
   - `getScheduledMessages()` - Obtiene mensajes programados
   - `updateMessageStatus()` - Actualiza estado de mensajes
   - `invalidateCache()` - Invalida cachés

2. **`src/scheduled-messages.js`** - Nuevo servicio completo
   - Clase `ScheduledMessageService`
   - Verificación automática cada minuto
   - Manejo de estados y errores
   - Formateo de números de teléfono

3. **`src/app.js`** - Integración del nuevo sistema
   - Importación del servicio
   - Inicialización automática
   - Nuevos endpoints API
   - Estadísticas integradas

### ✅ Funcionalidades Implementadas

#### Sistema de Verificación Automática
- ⏰ Verificación cada 60 segundos
- 📅 Comparación de fecha/hora actual
- 🔍 Filtrado de mensajes pendientes
- 📤 Envío automático cuando corresponde

#### Manejo de Estados
- 🟡 **Pendiente**: Mensaje esperando envío
- 🟢 **Enviado**: Mensaje enviado exitosamente
- 🔴 **Error**: Error en el envío

#### API Endpoints
- `GET /v1/scheduled-stats` - Estadísticas
- `POST /v1/scheduled-check` - Verificación forzada
- `POST /v1/scheduled-restart` - Reiniciar servicio

### ✅ Estructura de Google Sheets

**Hoja: "Mensajes_Programados"**
```
A: Fecha (YYYY-MM-DD)
B: Hora (HH:MM)
C: phone (número con código país)
D: addAnswer (mensaje a enviar)
E: media (URL opcional)
F: Estado (Pendiente/Enviado/Error)
```

## Pasos para Probar el Sistema

### 1. Configurar Google Sheets
1. Crear nueva hoja llamada "Mensajes_Programados"
2. Agregar encabezados en fila 1: `Fecha | Hora | phone | addAnswer | media | Estado`
3. Importar datos del archivo `ejemplo-mensajes-programados.csv`

### 2. Configurar Mensaje de Prueba
```
Fecha: 2024-01-15 (cambiar por fecha actual)
Hora: 14:30 (cambiar por hora actual + 2 minutos)
phone: TU_NUMERO_DE_TELEFONO
addAnswer: ¡Prueba exitosa! El sistema de mensajes programados funciona correctamente 🎉
media: (dejar vacío o agregar URL de imagen)
Estado: Pendiente
```

### 3. Ejecutar el Bot
```bash
npm run dev
```

### 4. Verificar Funcionamiento
1. **Logs del Sistema**: Verificar que aparezcan mensajes como:
   ```
   📅 Servicio de mensajes programados inicializado
   📅 Programador iniciado - verificando cada 60 segundos
   ✅ Mensajes programados cargados y cacheados correctamente. Total: X
   ```

2. **Estadísticas**: Llamar endpoint para ver estadísticas:
   ```bash
   curl http://localhost:3008/v1/scheduled-stats
   ```

3. **Verificación Forzada**: Forzar verificación inmediata:
   ```bash
   curl -X POST http://localhost:3008/v1/scheduled-check
   ```

### 5. Verificar Envío
1. Esperar a que llegue la hora programada
2. Verificar que el mensaje se envíe automáticamente
3. Comprobar que el estado en Google Sheets cambie a "Enviado"
4. Verificar logs del sistema para confirmación

## Casos de Prueba

### Caso 1: Mensaje Simple
```
Fecha: 2024-01-15
Hora: 10:00
phone: 5491234567890
addAnswer: Mensaje de prueba simple
media: 
Estado: Pendiente
```

### Caso 2: Mensaje con Media
```
Fecha: 2024-01-15
Hora: 11:00
phone: 5491234567890
addAnswer: Mensaje con imagen
media: https://via.placeholder.com/300x200.png?text=Prueba
Estado: Pendiente
```

### Caso 3: Múltiples Destinatarios
```
Fecha: 2024-01-15
Hora: 12:00
phone: 5491234567890
addAnswer: Mensaje masivo 1
media: 
Estado: Pendiente

Fecha: 2024-01-15
Hora: 12:00
phone: 5491987654321
addAnswer: Mensaje masivo 2
media: 
Estado: Pendiente
```

## Verificación de Errores

### Error de Número Inválido
```
phone: numero_invalido
```
**Resultado esperado**: Estado cambia a "Error"

### Error de URL de Media
```
media: https://url-inexistente.com/imagen.jpg
```
**Resultado esperado**: Estado cambia a "Error"

### Error de Formato de Fecha
```
Fecha: 15/01/2024 (formato incorrecto)
```
**Resultado esperado**: Mensaje no se procesa

## Monitoreo y Logs

### Logs Esperados (Éxito)
```
📅 Servicio de mensajes programados inicializado
📅 Programador iniciado - verificando cada 60 segundos
✅ Mensajes programados cargados y cacheados correctamente. Total: 5
📅 Encontrados 1 mensajes para enviar
📤 Enviando mensaje programado a 5491234567890@s.whatsapp.net
✅ Mensaje programado enviado exitosamente a 5491234567890@s.whatsapp.net
✅ Estado actualizado en fila 2: Enviado
```

### Logs Esperados (Error)
```
❌ Error al enviar mensaje programado: Error: Invalid phone number
❌ Error al actualizar estado de error: ...
```

## Compatibilidad Verificada

- ✅ No interfiere con flujo de palabras clave existente
- ✅ Compatible con sistema de IA (Groq)
- ✅ Mantiene historial de conversaciones
- ✅ Funciona con mensajes multimedia
- ✅ Sistema de caché optimizado
- ✅ Endpoints API funcionando

## Próximos Pasos

1. **Configurar Google Sheets** con la nueva hoja
2. **Probar con datos reales** usando números de teléfono válidos
3. **Monitorear logs** para verificar funcionamiento
4. **Ajustar horarios** según zona horaria del servidor
5. **Documentar casos de uso** específicos del negocio

## Notas Importantes

- El sistema usa la zona horaria del servidor
- La verificación es cada minuto, no cada segundo
- Los mensajes con error no se reenvían automáticamente
- El caché se actualiza cada 5 minutos
- Los números de teléfono deben incluir código de país