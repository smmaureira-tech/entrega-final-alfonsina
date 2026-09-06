# Screenshots — evidencia del test de estrés

Ver `../Plan_Test_Estres.md` para el detalle de cada corrida. Correspondencia:

| Archivo | Corrida (Plan_Test_Estres.md) | Qué muestra |
|---|---|---|
| `06_gmail_respuesta.png` | #3 — Estándar | Mail de respuesta automática enviada al huésped, mapeando el hilo original (Estado final: Enviado). |
| `05_gmail_aprobacion.png` | #1 y #2 — Reclamo | Mail de aprobación humana (HITL) con links "Aprobar" / "Rechazar" antes de contactar al huésped. |
| `04_notion_errores.png` | #5 — Fallo forzado de API | Panel de Ejecuciones de n8n mostrando el error real en el nodo "Notion - Error de registro" (bug conocido, ver nota en `Plan_Test_Estres.md`). |
| `03_notion_consultas.png` | General | Base Notion "Consultas" con registros reales de las 5 corridas (distintas categorías y estados). |
| `02_error_capturado_1.png` | #5 — Fallo forzado de API | Mail de alerta automática al dueño avisando la falla — este paso del manejo de errores sí funcionó correctamente. |
| `01_ejecucion_n8n_ok_1.png` | General | Editor de n8n con el flujo completo y el mensaje "Flujo de trabajo ejecutado correctamente". |
