# Constitución · Mesa de Ayuda

## Misión y audiencia

Construir una API interna para gestionar tickets de soporte con SLA por prioridad y
escalamiento en tres niveles de soporte.

La usan soporte y producto (Martín) y operaciones (Sofía). Ingeniería (Lucía) la
mantiene. Los acuerdos funcionales se construyen con el equipo a partir del
[README](README.md) y de las conversaciones de requisitos; para este proyecto no
se consulta Jira.

## Stack

- Python 3.12 y FastAPI.
- PostgreSQL; SQLite para los tests.
- pytest.
- Docker Compose para ejecutar el proyecto de forma reproducible.

Las decisiones técnicas se documentan fuera de los specs funcionales.

## Calidad

- Primero el test: cada criterio de aceptación `R<n>` del spec debe tener al menos
  un test que verifique su comportamiento.
- Validar las entradas y devolver errores claros y consistentes.
- Todo cambio entra por merge request con revisión humana.
- Una fase se termina cuando satisface sus criterios, pasan sus pruebas y se
  completa la revisión humana.

## Seguridad y datos

- Solo datos sintéticos: no usar información real de personas, clientes o sistemas.
- Está prohibido ingresar información confidencial, como claves, contraseñas o
  tokens, y está prohibido incluirla en logs. La detección automática de ese
  contenido queda fuera del MVP.
- No registrar descripciones de tickets ni credenciales en logs.
- Ningún secreto en el repositorio: la configuración se proporciona mediante
  variables de entorno.
- El MVP requiere autenticación y permisos desde la primera fase. Toda operación
  sobre tickets exige autenticación.
- Existen tres roles: **soporte y producto** (un único rol), **operaciones** e
  **ingeniería**.

Permisos acordados para MDA-1:

| Rol | Crear tickets | Consultar y buscar todos los tickets |
| --- | --- | --- |
| Soporte y producto | Sí | Sí |
| Operaciones | No | Sí |
| Ingeniería | Sí | Sí |

Los permisos de asignación, respuesta, cierre y escalamiento se definen en los
specs de sus respectivas fases. El nivel de soporte (1, 2 o 3) y la prioridad del
ticket son conceptos independientes.

## Forma de trabajo

- El orden de trabajo vive en [specs/backlog.md](specs/backlog.md): fases chicas,
  una por historia `MDA-n`. Se revisa entre fases.
- Se trabaja una fase por vez, tomando la primera casilla sin marcar.
- Cada fase tiene un spec en `specs/NNN-<slug>/spec.md`, enlazado a su historia en
  el README. La numeración de carpetas corresponde al orden de implementación,
  no al número de la historia.
- El spec se redacta y revisa antes de escribir código. Expresa el comportamiento
  esperado y sus criterios de aceptación, sin decisiones de stack.
- Si el comportamiento cambia, cambia primero el spec.
- La casilla de una fase se marca únicamente cuando la fase está terminada;
  redactar su spec no equivale a implementarla.
- Los cambios a esta constitución requieren acuerdo del equipo.

Las reglas funcionales concretas, incluidos los tiempos de SLA y la paginación,
viven en la fase correspondiente y pueden cambiar actualizando primero su spec.
