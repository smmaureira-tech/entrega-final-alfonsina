# Entrega Final — Ecosistema de Automatización IA
**Caso de uso:** Triage y clasificación automática de consultas de huéspedes (Cabañas La Alfonsina) — mail de un huésped → clasificación por IA (Reclamo / Estándar / Spam) → aprobación humana (mail a Gmail) para Reclamos → respuesta al huésped (Gmail).

**Stack:** n8n (orquestación) + Notion (memoria/base de datos: 2 bases) + Claude/Anthropic (motor de razonamiento) + Gmail (entrada, aprobación humana y canal de salida). Versión deliberadamente simple: un solo trigger, un solo canal, sin Slack.

## Archivos de este repo
1. `01_Diagrama_Arquitectura.pdf` → Entregable 1 (Mapa de arquitectura, 20%).
2. `02_Documentacion_Tecnica.pdf` → Entregable 2 (estructuras de datos), 3 (optimización de costos) y 4 (seguridad y resiliencia) — 20% c/u.
3. `flujo_n8n.json` → Blueprint técnico de respaldo del flujo (JSON exportado desde n8n) — ya importado y probado.
4. `Plan_Test_Estres.md` → las 5 corridas del test de estrés (camino feliz x4 + camino infeliz), con resultado real de cada una.
5. `screenshots/` → Evidencia de ejecución (ver `screenshots/README.md` para la correspondencia con cada corrida).
6. Este `README.md` con los links del Entregable 5 (Dashboard) y de la base en modo lectura.

## Checklist de armado (orden recomendado — priorizado para terminar rápido)
- [x] **Notion (2 bases, no 4):** creadas **Consultas** y **Errores** según el esquema de `02_Documentacion_Tecnica.pdf` (sección 2.1), con la relación `Consulta` en Errores apuntando a Consultas.
- [x] **Dashboard:** vista de Consultas publicada como **Shared View pública** (Notion → Share → Publish to web) → link abajo.
- [x] **n8n:** credenciales reales conectadas (Anthropic → Header Auth con `x-api-key`; Notion; Gmail OAuth2) y flujo activo en la instancia real.
- [x] **Test de estrés:** corrido en vivo (Reclamo+Aprobado, Reclamo+Rechazado, Estándar, Spam, y fallo forzado de API → Error Trigger). Ver `Plan_Test_Estres.md` para el detalle de cada corrida — incluye un bug conocido y no corregido en el logging a Notion "Errores" (el Error Trigger y la alerta por Gmail sí funcionan).
- [x] **Screenshots:** guardadas en `screenshots/` con la correspondencia detallada en `screenshots/README.md`. La captura de la corrida #5 muestra el error real del nodo "Notion - Error de registro" en vez de un registro exitoso, ya que ese logging tiene un bug conocido (ver nota en `Plan_Test_Estres.md`).
- [x] **Video demo (3 min):** link abajo.
- [ ] **Subir todo a GitHub** (repo público o con acceso de lectura) y pegar el link de este repo en la entrega de Coderhouse.

## Links de la entrega
- Link a la base Notion en modo lectura: https://bold-hydrangea-449.notion.site/3d1551c50c5880f8b16eee64e2e04183?v=3d2551c50c5880b39d61000c478ef807&source=copy_link
- Link al Dashboard/Shared View de KPIs: misma página (la vista Tablero agrupada por Estado ya funciona como panel de KPIs).
- Link al video demo (3 min): https://drive.google.com/file/d/1aVRDqRUIgHNQmEZZ1ho-NsddzjveDMyn/view?usp=sharing

## Check de seguridad (ya contemplado en el diseño, ver PDF sección 4)
1. Filtro anti-bucle infinito: Gmail Trigger en modo "From now on" (no reprocesa historial). ✅
2. Comparaciones de tipo correctas en filtros (number vs number, ej. campo "urgencia"). ✅
3. Prompt de IA dinámico con variables del sistema ({{remitente}}, {{mensaje}}). ✅

---
*Cabañas La Alfonsina · Entrega Final, curso AI Automation (Coderhouse)*
