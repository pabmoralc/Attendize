# Tarea 2 - Propuesta y alcance

**Proyecto:** Modernización de Attendize v2.8.0 (caso TechFest)\
**Rama:** `tarea-02-planificacion` (sale de `tarea-01-entrega`)\
**Trabajo individual**

---

## 1. Descripción del problema

### Cuál es la necesidad

TechFest organiza eventos de tecnología en los que hay varias charlas y talleres. Para cada evento necesitan un programa de sesiones con el título, la hora, la sala y el aforo de cada una. El organizador tendría que poder gestionarlo desde el panel y el público consultarlo en la web.

Attendize sirve para gestionar eventos, entradas, pedidos y asistentes, pero no tiene nada para las sesiones. Ahora mismo lo único que se puede hacer es escribir el programa a mano en la descripción del evento, y eso da varios problemas:

- no se comprueba si las horas o el aforo están bien;
- las sesiones no se pueden ordenar por hora;
- no se puede publicar o esconder una sesión concreta;
- otra aplicación no puede leer el programa.

### A quién afecta

- **Al organizador**, porque cada vez que cambia una hora o una sala tiene que reescribir el texto.
- **A los visitantes y asistentes**, que no tienen un programa claro para consultar.
- **A quien mantiene la aplicación**, porque trabaja con versiones que ya no tienen soporte.

### Hechos comprobados

| N.º | Hecho | Cómo lo he comprobado |
|---|---|---|
| H1 | Attendize v2.8.0 no tiene ninguna pantalla ni tabla para las sesiones del programa. | Revisando el panel del organizador y con `SHOW TABLES;` en MySQL. |
| H2 | El programa solo se puede poner en la descripción del evento, como texto libre. | En el formulario de crear y editar evento. |
| H3 | La aplicación usa PHP 7.3, Laravel 6.5, MySQL 5.7 y plantillas Blade con jQuery. Son versiones sin soporte oficial. | `composer.json`, `Dockerfile` y `docker-compose.yml` (Tarea 1). |
| H4 | Para desplegarla en local tuve que solucionar dos problemas: el puerto 3306 ocupado y un error con `APP_KEY` en `/install`. | README de la Tarea 1. |
| H5 | En mi entorno local (Docker en Windows) cada carga de página del panel tarda unos 22 segundos. | DevTools > Network, valor *Load* sin caché, media de 3 cargas (04/11/2026). |

### Suposiciones (habría que confirmarlas con el cliente)

| N.º | Suposición |
|---|---|
| S1 | En TechFest hay sesiones a la misma hora en salas distintas. |
| S2 | El aforo de la sesión es solo informativo, no hace falta reservar plaza en cada sesión. |
| S3 | La mayoría del público mirará el programa desde el móvil. |
| S4 | Las sesiones tienen que estar dentro de las fechas del evento. |
| S5 | Ahora mismo llevan el programa fuera de Attendize (en un documento o una hoja de cálculo). |

### Dudas para el cliente

1. ¿Puede haber dos sesiones a la vez en la misma sala?
2. ¿El aforo de una sesión tiene algún límite (el del evento, el de la sala…)?
3. ¿Se puede crear una sesión fuera de las fechas del evento?
4. ¿Se publica cada sesión por separado o todo el programa a la vez?
5. ¿Hace falta guardar los ponentes? De momento lo he puesto como opcional.

---

## 2. Perfiles de usuario

| Perfil | Qué quiere conseguir | Qué hace | Qué datos consulta o cambia | Problema actual |
|---|---|---|---|---|
| **Organizador** (con sesión iniciada) | Tener al día el programa de sus eventos | Iniciar sesión, entrar en un evento suyo, ver las sesiones y crear sesiones nuevas | Consulta sus eventos y sesiones. Cambia las sesiones | No hay gestión de sesiones, tiene que escribir el programa en la descripción |
| **Visitante** | Saber qué charlas hay, a qué hora y dónde | Ver la página de un evento publicado y su programa | Solo consulta el evento y las sesiones publicadas | El programa no está ordenado ni es fácil de leer, sobre todo en el móvil (S3) |
| **Asistente** | Conseguir su entrada y decidir a qué sesiones va | Hacer la inscripción (de prueba) y consultar el programa | Consulta el evento, las entradas y las sesiones. Crea su pedido (datos ficticios) | Tiene que buscar el programa dentro de la descripción |

El personal de acceso (el que hace el registro de entrada) no lo he incluido en el núcleo. Esa parte se queda como está en Attendize.

---

## 3. Alcance

### Recorrido principal

El recorrido que voy a modernizar es: **el organizador crea una sesión en el programa de uno de sus eventos.**

1. Inicia sesión y entra en el panel.
2. Abre uno de sus eventos.
3. Entra en el programa, que puede estar vacío o tener sesiones ordenadas por hora.
4. Rellena el formulario de nueva sesión: título, inicio, fin, sala, aforo y si se publica o no.
5. Laravel comprueba que el evento es suyo y que los datos son correctos, y guarda la sesión en MySQL.
6. Sale un mensaje de confirmación y la sesión aparece en la lista.
7. Si está publicada, también sale en el programa público y en la API.

Con este recorrido se pasa por todas las partes: la interfaz (React + TypeScript), Inertia.js, Laravel y MySQL.

### Qué entra y qué no

| Prioridad | Función | Requisito |
|---|---|---|
| Obligatoria | Ver las sesiones de un evento propio | RQ-01 |
| Obligatoria | Crear una sesión | RQ-02 |
| Obligatoria | Validar los datos en el servidor | RQ-03 |
| Obligatoria | Mensajes claros, no perder lo escrito y uso con teclado | RQ-04 |
| Obligatoria | Permisos: cada uno solo ve y cambia lo de su cuenta | RQ-05 |
| Obligatoria | Programa público en React (móvil, ordenador y teclado) | RQ-06 |
| Obligatoria | API REST de las sesiones publicadas, usada desde React | RQ-07 |
| Obligatoria | Que los datos se guarden aunque se reinicie el entorno | RQ-08 |
| Obligatoria | Mantener lo que ya funciona (acceso, eventos, entradas, asistentes, página pública) | RQ-09 |
| Obligatoria | Migrar los datos acordados sin duplicarlos | RQ-10 |
| Obligatoria (Fase 2) | Desplegar Laravel y MySQL en AWS Learner Lab | Se planifica en T5 |
| Obligatoria (T4) | El cambio de requisito que diga el profesor | Se verá en T4 |
| Deseable | Editar una sesión y cambiar si está publicada | RQ-11 |
| Deseable | Avisar si dos sesiones coinciden en la misma sala | RQ-12 |
| Opcional | Filtrar el programa público por día y sala | — |
| Opcional | Ponentes de cada sesión | — |
| Opcional | Exportar el programa al calendario (`.ics`) | — |
| Fuera de alcance | Migrar Attendize entero con todas sus funciones | — |
| Fuera de alcance | Pagos reales, devoluciones y facturas | — |
| Fuera de alcance | Integraciones con servicios externos de correo o pago | — |
| Fuera de alcance | Streaming, recomendaciones y app móvil | — |
| Fuera de alcance | Reservar plaza en cada sesión | — |

Lo deseable y lo opcional solo lo haré si me da tiempo después de terminar y probar lo obligatorio.

---

## 4. Requisitos

| Código | Usuario | Necesidad | Prioridad | Datos afectados | Origen |
|---|---|---|---|---|---|
| **RQ-01** | Organizador | Ver las sesiones de un evento suyo, ordenadas por fecha y hora, con título, horario, sala, aforo y si están publicadas. Si no hay ninguna, que salga un mensaje avisando. | Obligatoria | Sesiones, evento | Anexo A (núcleo) |
| **RQ-02** | Organizador | Crear una sesión en un evento suyo con título, inicio, fin, sala, aforo y si se publica. | Obligatoria | Sesiones | Anexo A (núcleo) |
| **RQ-03** | Organizador | Que el servidor no deje guardar datos incorrectos: título y sala obligatorios (máximo 150 caracteres), inicio y fin obligatorios, el fin después del inicio y el aforo un número entero mayor que 0. | Obligatoria | Sesiones | Anexo A (núcleo), enunciado R3 |
| **RQ-04** | Organizador | Que los mensajes de error y de éxito se entiendan y salgan al lado de cada campo, que no se borre lo escrito si hay un error y que el formulario se pueda usar solo con el teclado. | Obligatoria | Ninguno (es de la interfaz) | Enunciado R4 y R5 |
| **RQ-05** | Organizador | Que nadie pueda ver ni cambiar eventos o sesiones de otra cuenta, ni desde la web ni enviando peticiones a mano. La cuenta y el evento los decide el servidor según el usuario que ha iniciado sesión, no según lo que envíe el navegador. | Obligatoria | Cuentas, eventos, sesiones | Anexo A (núcleo), enunciado R2, A4 |
| **RQ-06** | Visitante | Ver el programa de un evento publicado en una vista hecha con React, solo con las sesiones publicadas, ordenado por hora y que se vea bien en el móvil, en el ordenador y con el teclado. | Obligatoria | Eventos y sesiones (solo lectura) | Anexo A (núcleo), S3 |
| **RQ-07** | Visitante / otra aplicación | Poder consultar en JSON, con una API REST, las sesiones publicadas de un evento publicado. Solo con los datos necesarios, con los errores documentados y usada desde una vista React. | Obligatoria | Sesiones (solo lectura) | Anexo A (núcleo), enunciado R6 |
| **RQ-08** | Organizador | Que las sesiones se guarden en MySQL y sigan ahí después de parar y volver a arrancar los contenedores. | Obligatoria | Sesiones | Anexo A (núcleo) |
| **RQ-09** | Organizador / visitante | Poder seguir usando lo que ya hace Attendize: iniciar sesión, el panel, listar y abrir sus eventos, crear y editar eventos de forma básica, los tipos de entrada, los asistentes y la página pública. | Obligatoria | Cuentas, eventos, entradas, pedidos, asistentes | Anexo A (funciones que se conservan), enunciado R1 |
| **RQ-10** | Cliente | Que al migrar no se pierdan las cuentas, usuarios, organizadores, eventos, entradas, pedidos ni asistentes, ni sus relaciones; que si se repite la migración no se dupliquen datos, y que haya una copia de seguridad que se pueda restaurar. | Obligatoria | Todos los que se conservan | Anexo A (datos y migración), A7 |
| **RQ-11** | Organizador | Editar una sesión y cambiar si está publicada o no, con las mismas validaciones que al crearla. | Deseable | Sesiones | Idea propia |
| **RQ-12** | Organizador | Que avise si una sesión coincide en hora con otra de la misma sala en el mismo evento. | Deseable | Sesiones | S1 y duda 1 |

**Reglas sin decidir:** si el aforo tiene un límite y si la sesión tiene que estar dentro de las fechas del evento depende de las dudas 2 y 3. Si el cliente dice que sí, lo añadiré a RQ-03.
