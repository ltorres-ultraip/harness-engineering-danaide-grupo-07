# Backlog · Mesa de Ayuda

Orden de implementación acordado según la [constitución](../constitution.md).
Cada casilla representa una historia completa. Todas están pendientes de
implementación; los specs de las fases posteriores se redactan al abordar cada
fase.

- [ ] **001 · MDA-1 — Crear y consultar tickets.**
  [Historia](../README.md#historias-cargadas-en-jira) ·
  [Spec](001-tickets/spec.md).
  Crear tickets con sus campos iniciales; consultar por identificador y listar
  con filtros combinables, búsqueda en título y descripción y paginación.
  Incluir autenticación y los permisos acordados para los tres roles.
- [ ] **002 · MDA-2 — Asignar y responder tickets.**
  [Historia](../README.md#historias-cargadas-en-jira).
  Asignar tickets y registrar respuestas. Definir permisos y qué respuesta cuenta
  como primera respuesta para el posterior cálculo de SLA.
- [ ] **003 · MDA-3 — Cerrar tickets.**
  [Historia](../README.md#historias-cargadas-en-jira).
  Cerrar tickets y registrar el momento de resolución. Definir permisos y
  condiciones de cierre.
- [ ] **004 · MDA-5 — Escalar de nivel (1, 2 y 3).**
  [Historia](../README.md#historias-cargadas-en-jira).
  Incorporar el escalamiento manual entre los niveles de soporte y definir sus
  permisos y el comportamiento al llegar al nivel 3.
- [ ] **005 · MDA-4 — SLA por prioridad.**
  [Historia](../README.md#historias-cargadas-en-jira).
  Calcular y determinar los vencimientos de primera respuesta y resolución,
  usando horas corridas desde la creación y los tiempos acordados de la tabla
  siguiente. Detallar los criterios en el spec de esta fase.
- [ ] **006 · MDA-6 — SLA vencido: marcar el ticket y escalarlo.**
  [Historia](../README.md#historias-cargadas-en-jira).
  Integrar los vencimientos con el marcado y el escalamiento automático.
  Definir cómo evitar efectos repetidos y qué ocurre en el nivel 3.
- [ ] **007 · MDA-7 — Reportes para operaciones.**
  [Historia](../README.md#historias-cargadas-en-jira).
  Consultar tickets abiertos por nivel y cumplimiento de SLA. Definir permisos,
  períodos de consulta y significado de las métricas.

## Reglas acordadas para MDA-4

Ambos plazos se cuentan desde la creación del ticket, en tiempo corrido, incluidos
fines de semana y feriados.

| Prioridad | Primera respuesta | Resolución |
| --- | --- | --- |
| P1 · Crítica | 15 minutos | 2 horas |
| P2 · Alta | 1 hora | 4 horas |
| P3 · Media | 4 horas | 24 horas |
| P4 · Baja | 8 horas | 72 horas |

Estos valores son requisitos de MDA-4 y se incorporarán a su spec antes de
implementarla. MDA-1 no implementa el cálculo de SLA.

## Cierre de una fase

Marcar su casilla después de cumplir los criterios del spec, pasar las pruebas
asociadas y completar la revisión humana del merge request. Revisar el backlog
antes de comenzar la siguiente fase.
