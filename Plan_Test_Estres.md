# Plan de Test de Estrés (mínimo 5 corridas)

Correr y registrar el resultado de cada una. Screenshot de cada corrida en n8n + el registro resultante en Notion.

| # | Escenario | Input | Resultado esperado | Verifica |
|---|-----------|-------|---------------------|----------|
| 1 | Camino feliz — Reclamo + Aprobado | Mensaje con queja/reclamo explícito sobre una reserva anterior | Categoría Reclamo, Estado "Esperando Aprobación", mail HITL por Gmail; al click en "Aprobar" se envía la respuesta al huésped y Estado pasa a "Enviado" | Clasificación + HITL (camino Aprobar) |
| 2 | Camino feliz — Reclamo + Rechazado | Igual al anterior | Al click en "Rechazar" el Estado pasa a "Descartado" y NO se envía respuesta al huésped | HITL (camino Rechazar) |
| 3 | Camino feliz — Estándar | Consulta de disponibilidad/reserva sin queja | Categoría Estándar, auto-respuesta inmediata sin pasar por Wait, Estado = "Enviado" | Router + auto-respuesta |
| 4 | Camino feliz — Spam | Mensaje publicitario o no relacionado con una reserva | Categoría Spam, Estado = "Descartado", sin envío de mail | Router + descarte |
| 5 | Camino infeliz — fallo de API IA | Forzar error (ej. API key inválida temporalmente, o desconectar credencial) | Error Trigger se activa, se loguea en Notion "Errores" con nodo/mensaje truncado, llega el mail de alerta al dueño por Gmail | Manejo de errores / resiliencia |

## Cómo forzar el "camino infeliz" (#5)
Desactivar temporalmente la credencial de Anthropic en n8n (o poner una key inválida) antes de correr, y volver a activarla después.

## Registro
Corridas ya probadas en vivo la noche del 5/9/2026 (ver `screenshots/`), confirmando que los 3 caminos de HITL, el auto-respondido y el manejo de errores funcionan de punta a punta.

| # | Fecha/hora | Resultado obtenido | ¿OK? |
|---|------------|---------------------|------|
| 1 | 05/09/2026 | Reclamo → HITL → Aprobado → respuesta enviada al huésped → Estado "Enviado" | ✅ |
| 2 | 05/09/2026 | Reclamo → HITL → Rechazado → Estado "Descartado", sin envío | ✅ |
| 3 | 05/09/2026 | Estándar → auto-respuesta directa → Estado "Enviado" | ✅ |
| 4 | 05/09/2026 | Spam → Estado "Descartado" | ✅ |
| 5 | 05/09/2026 | Fallo forzado → Error Trigger disparado → alerta por Gmail enviada → nodo "Notion - Error de registro" falló (Solicitud incorrecta: el body enviado incluía campos internos del objeto de error de n8n como `name`/`isNotionError` en vez de los campos mapeados de la base Errores) | ⚠️ parcial |

## Nota post-entrega — bug conocido (no corregido)
El nodo **Notion - Error de registro** falla en las 3 corridas registradas la noche del 5/9 (20:51, 20:56, 20:57) con: *"Solicitud incorrecta: comprueba sus parámetros — body.name no debería estar presente, en su lugar estaba "PublicApiError"; body.isNotionError no debería estar presente..."*. Causa probable: el nodo mapea el objeto de error completo (`{{ $json }}`) en vez de los campos específicos que espera la base Errores (ej. `{{ $json.message }}`). El Error Trigger global y la alerta por Gmail sí funcionan correctamente — solo el logging en Notion queda pendiente de corregir.
