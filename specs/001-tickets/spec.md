# 001 · Crear y consultar tickets

Historia de origen: [MDA-1 — Crear y consultar tickets](../../README.md#historias-cargadas-en-jira).

Acuerdos aplicables: [constitución](../../constitution.md).
Orden de trabajo: [primera fase pendiente del backlog](../backlog.md).
Estado: especificada; pendiente de implementación y revisión humana.

## Contexto

El equipo de soporte necesita registrar solicitudes y encontrar tickets para
atenderlos. Esta fase establece la creación y consulta de tickets de la API
interna, con autenticación y permisos desde el inicio. Todos los datos son
sintéticos y no se permite ingresar información confidencial.

Los niveles 1, 2 y 3 representan niveles de soporte. Son independientes de la
prioridad: un ticket crítico también comienza en el nivel de soporte 1.

### Roles y permisos

| Rol autenticado | Crear | Consultar por identificador, listar y buscar |
| --- | --- | --- |
| Soporte y producto | Sí | Sí, sobre todos los tickets |
| Operaciones | No | Sí, sobre todos los tickets |
| Ingeniería | Sí | Sí, sobre todos los tickets |

### Campos del ticket

| Campo | Regla en esta fase |
| --- | --- |
| Cliente | Obligatorio; identificador sintético del cliente. |
| Categoría | Obligatoria; texto libre. |
| Prioridad | Obligatoria; uno de los valores `crítica`, `alta`, `media`, `baja`. Corresponden a P1, P2, P3 y P4, respectivamente. |
| Título | Obligatorio; texto no vacío de hasta 200 caracteres. |
| Descripción | Obligatoria; texto no vacío. |
| Identificador | Único; generado al crear el ticket y utilizado para consultarlo. |
| Fecha de creación | Registrada por el sistema al crear el ticket. |
| Estado | Inicialmente abierto. |
| Nivel de soporte | Inicialmente 1, cualquiera sea la prioridad. |
| Asignación | Inicialmente sin asignar. |

Los campos de entrada obligatorios no pueden omitirse ni contener valores
vacíos. Para los campos de texto, un valor compuesto solo por espacios se
considera vacío. Identificador, fecha de creación y valores iniciales de estado,
nivel y asignación los establece el sistema.

### Consulta y búsqueda

Se puede consultar un ticket por su identificador o listar tickets. El listado
admite filtros combinables por cliente, categoría, prioridad y rango de fecha de
creación, además de búsqueda por texto en título o descripción.

Las condiciones de distintos filtros se combinan con **Y**. La búsqueda textual
encuentra coincidencias parciales, sin distinguir mayúsculas y minúsculas, en el
título **o** en la descripción. Cuando se combina con filtros, debe cumplir
también todos esos filtros.

El listado se ordena del más reciente al más antiguo. Tiene 20 tickets por página
por defecto, permite un máximo de 100 e informa el total de resultados que
cumplen la consulta antes de paginar. Estas reglas pueden revisarse actualizando
primero este spec.

## Historias de usuario

- **H1 · Crear:** como integrante de soporte y producto o de ingeniería, quiero
  crear un ticket con los datos obligatorios para registrar una solicitud.
- **H2 · Consultar:** como usuario de cualquiera de los tres roles, quiero
  consultar un ticket por su identificador para conocer sus datos y situación
  inicial.
- **H3 · Buscar:** como usuario de cualquiera de los tres roles, quiero combinar
  filtros y búsqueda textual y recorrer resultados paginados para localizar
  tickets.
- **H4 · Controlar el acceso:** como responsable del servicio, quiero que las
  operaciones exijan autenticación y respeten los permisos acordados.

Todas estas historias desarrollan el alcance de MDA-1.

## Criterios de aceptación en EARS

Cada criterio `R<n>` debe contar con al menos un test, escrito antes de su
implementación, según la constitución.

### Acceso

- **R1 — H4:** Cuando una solicitud de creación o consulta carezca de una
  autenticación válida, el sistema deberá rechazarla con un error claro de
  autenticación, sin crear tickets ni revelar sus datos.
- **R2 — H4:** Cuando un usuario autenticado no tenga permiso para la operación
  solicitada según la matriz de roles, el sistema deberá rechazarla con un error
  claro de permisos, sin ejecutarla ni revelar datos del ticket.
- **R3 — H4:** Cuando un usuario de soporte y producto o de ingeniería solicite
  crear un ticket con datos válidos, el sistema deberá permitir la creación.
- **R4 — H2, H3, H4:** Cuando un usuario de cualquiera de los tres roles solicite
  consultar, listar o buscar tickets, el sistema deberá permitir la consulta
  sobre todos los tickets que cumplan sus condiciones de búsqueda.

### Creación y validación

- **R5 — H1:** Cuando un usuario autorizado solicite crear un ticket con cliente,
  categoría, prioridad, título y descripción válidos, el sistema deberá
  registrar el ticket y devolver sus campos de entrada y los campos generados.
- **R6 — H1:** Cuando se cree un ticket, el sistema deberá asignarle un
  identificador único y registrar su fecha de creación.
- **R7 — H1:** Cuando se cree un ticket, el sistema deberá establecerlo como
  abierto, en el nivel de soporte 1 y sin asignar, independientemente de su
  prioridad.
- **R8 — H1:** Si falta un campo obligatorio, un campo obligatorio está vacío o
  un texto obligatorio contiene solo espacios, el sistema deberá rechazar la
  creación, indicar el campo inválido y no registrar el ticket.
- **R9 — H1:** Si el título supera 200 caracteres, el sistema deberá rechazar la
  creación, indicar el límite incumplido y no registrar el ticket.
- **R10 — H1:** Si la prioridad no pertenece a `crítica`, `alta`, `media` o
  `baja`, el sistema deberá rechazar la creación, indicar los valores admitidos y
  no registrar el ticket.

### Consulta por identificador

- **R11 — H2:** Cuando un usuario autorizado consulte el identificador de un
  ticket existente, el sistema deberá devolver sus campos registrados,
  incluidos identificador, fecha de creación, estado, nivel y asignación.
- **R12 — H2:** Cuando un usuario autorizado consulte un identificador
  inexistente, el sistema deberá devolver un error claro que indique que el
  ticket no existe.

### Listado, filtros y búsqueda

- **R13 — H3:** Cuando un usuario autorizado liste tickets sin filtros ni texto
  de búsqueda, el sistema deberá devolver los tickets según el orden y la
  paginación establecidos.
- **R14 — H3:** Cuando un usuario autorizado liste tickets con un filtro por
  cliente, categoría o prioridad, el sistema deberá devolver únicamente tickets
  cuyo campo cumpla el filtro solicitado.
- **R15 — H3:** Cuando un usuario autorizado liste tickets con un rango de fecha
  de creación válido, el sistema deberá devolver únicamente tickets cuya fecha
  de creación pertenezca al rango solicitado.
- **R16 — H3:** Cuando un usuario autorizado busque texto, el sistema deberá
  devolver tickets cuyo título o descripción contenga ese texto como
  coincidencia parcial, sin distinguir mayúsculas y minúsculas.
- **R17 — H3:** Cuando un usuario autorizado combine filtros y búsqueda
  textual, el sistema deberá devolver únicamente tickets que cumplan todas las
  condiciones solicitadas, conservando la alternativa entre título y
  descripción para la coincidencia textual.
- **R18 — H3:** Cuando se devuelva un listado, el sistema deberá ordenar sus
  resultados por fecha de creación del más reciente al más antiguo.
- **R19 — H3:** Cuando se solicite un listado sin indicar tamaño de página, el
  sistema deberá devolver como máximo 20 tickets en la página.
- **R20 — H3:** Cuando se solicite un tamaño de página válido entre 1 y 100, el
  sistema deberá respetar ese tamaño como máximo de tickets por página.
- **R21 — H3:** Cuando se devuelva una página de resultados, el sistema deberá
  informar el total de tickets que cumplen la consulta antes de paginar.
- **R22 — H3:** Cuando una consulta válida no tenga coincidencias, el sistema
  deberá devolver una lista vacía y un total de cero resultados.
- **R23 — H3:** Si una consulta incluye una prioridad no admitida, un rango de
  fechas inválido o un tamaño de página fuera de 1 a 100, el sistema deberá
  rechazar la consulta con un error claro que identifique el parámetro inválido.

### Confidencialidad de los logs

- **R24 — H4:** Cuando el sistema registre en logs una operación de creación o
  consulta, el sistema deberá omitir credenciales y descripciones de tickets.

## Fuera de alcance

- Asignar responsables y registrar respuestas: MDA-2.
- Cerrar tickets: MDA-3.
- Escalar manualmente a los niveles 2 y 3: MDA-5.
- Calcular SLA, determinar sus vencimientos o marcar incumplimientos: MDA-4.
- Escalar automáticamente por SLA vencido: MDA-6.
- Reportes de abiertos por nivel y cumplimiento de SLA: MDA-7.
- Editar o eliminar tickets y administrar catálogos de clientes o categorías.
- Administrar usuarios o roles mediante funcionalidades propias de esta fase.
- Detectar y rechazar automáticamente información confidencial en los textos.

## Preguntas abiertas

Antes de implementar los comportamientos afectados, acordar y actualizar este
spec con las respuestas. Las decisiones técnicas se documentarán por separado.

- ¿Qué longitudes máximas tendrán el identificador de cliente, la categoría y
  la descripción?
- ¿Los filtros por cliente y categoría exigirán coincidencia exacta y
  distinguirán mayúsculas y minúsculas?
- ¿Qué zona horaria y precisión se usarán en la fecha de creación y en los
  filtros? ¿Los extremos del rango serán inclusivos? ¿Se permitirán rangos con
  un solo extremo?
- ¿Cómo se desempatará el orden entre tickets con la misma fecha de creación
  para que la paginación sea estable?
- ¿Cómo se identificará la página solicitada y qué respuesta se dará cuando se
  pida una página posterior a la última?
- ¿Cómo se proveerán las identidades sintéticas y sus roles, y qué vigencia o
  revocación tendrán sus credenciales?
