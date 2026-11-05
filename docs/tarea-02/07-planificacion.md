# Tarea 2 - Planificación

- **Responsable:** todas las tareas las hago yo (Pablo), porque el trabajo es individual.
- **Estados:** Pendiente, En curso, En revisión y Finalizada.
- **Horas:** son una estimación. Seguramente tenga que ajustarlas cuando empiece a programar.

## Hitos

| Hito | Qué quiero tener | Tarea del proyecto | Depende de |
|---|---|---|---|
| **H1** | El proyecto nuevo (Laravel + React + TypeScript + Inertia) funcionando en Docker y la tabla de sesiones creada | T3 | Validación de la Tarea 2 |
| **H2** | El recorrido principal completo: iniciar sesión, ver el programa y crear una sesión con validación y permisos | T3 | H1 |
| **H3** | El programa público en React y la API REST | T4 | H2 |
| **H4** | Las funciones de Attendize que se conservan y el cambio de requisito del profesor | T4 | H2 |
| **H5** | La migración de datos, la copia de seguridad y el despliegue en AWS | T5 | H3, H4 |
| **H6** | La entrega final con toda la documentación | T6 | H5 |

## Tareas

| Orden | Código | Tarea | Hito | Depende de | Horas | Estado | Cuándo la doy por terminada |
|---|---|---|---|---|---|---|---|
| 1 | BASE-1 | Crear el proyecto Laravel con React, TypeScript e Inertia, con Docker y MySQL. Subir los archivos de versiones (`composer.lock`, `package-lock.json`). | H1 | Validación T2 | 6 h | Pendiente | Arranca con `docker compose up -d` y se ve una página de React servida por Laravel. |
| 2 | BASE-2 | Crear la tabla `event_sessions` (migración y modelo) y un *seeder* con los datos de prueba (organizadores A y B, TechFest 2027, Meetup Borrador). | H1 | BASE-1 | 3 h | Pendiente | `php artisan migrate --seed` crea las tablas y los datos sin errores. |
| 3 | RQ-09 (a) | Iniciar sesión, panel y lista de eventos propios. Es la primera parte de RQ-09 y la necesito para todo lo demás. | H2 | BASE-2 | 5 h | Pendiente | Se cumplen CA-09.1 (la parte de acceso y lista) y CA-09.2. |
| 4 | RQ-05 | Permisos: la cuenta y el evento salen del usuario que ha iniciado sesión y no se puede acceder a lo de otras cuentas. | H2 | RQ-09 (a) | 4 h | Pendiente | Se cumplen CA-05.1 a CA-05.4, con una prueba automática de B intentando entrar en el evento de A. |
| 5 | RQ-01 | Pantalla del programa del evento: lista ordenada y mensaje si está vacía (P1 y P2). | H2 | RQ-05 | 4 h | Pendiente | Se cumplen CA-01.1 y CA-01.2 en ordenador y en móvil. |
| 6 | RQ-02 | Formulario de nueva sesión y guardarla (P3). | H2 | RQ-01 | 4 h | Pendiente | Se cumple CA-02.1 y el registro aparece en MySQL. |
| 7 | RQ-03 | Validar en el servidor el título, la sala, las fechas y el aforo. | H2 | RQ-02 | 3 h | Pendiente | Se cumplen CA-03.1 a CA-03.4, con pruebas automáticas de datos incorrectos. |
| 8 | RQ-04 | Mensajes de error y de éxito, no perder lo escrito y uso con teclado (P4). | H2 | RQ-03 | 3 h | Pendiente | Se cumplen CA-04.1 y CA-04.2 probándolo con el teclado. |
| 9 | RQ-08 | Comprobar que las sesiones siguen ahí al reiniciar los contenedores. | H2 | RQ-02 | 1 h | Pendiente | Se cumple CA-08.1 y guardo una captura. |
| 10 | RQ-07 | API REST de las sesiones publicadas, con sus errores y su documentación. | H3 | RQ-02 | 4 h | Pendiente | Se cumplen CA-07.1 a CA-07.3 y la API está explicada en el repositorio. |
| 11 | RQ-06 | Vista de React del programa público que usa la API (P5). | H3 | RQ-07 | 4 h | Pendiente | Se cumplen CA-06.1 a CA-06.3 y CA-07.4. |
| 12 | RQ-09 (b) | El resto de funciones que se conservan: abrir evento, crear y editar eventos, tipos de entrada, asistentes y página pública. | H4 | RQ-09 (a) | 10 h | Pendiente | Se cumplen CA-09.1, CA-09.3 y CA-09.4. |
| 13 | CAMBIO-T4 | Analizar y hacer el cambio de requisito que diga el profesor. | H4 | H2, H3 | 4 h (a revisar) | Pendiente | El análisis está documentado y el código, los datos y las pruebas están actualizados. |
| 14 | RQ-10 | Script de migración desde la base de datos de Attendize, con copia antes y una lista de los registros que den problemas. | H5 | BASE-2, RQ-09 (b) | 8 h | Pendiente | Se cumplen CA-10.1 a CA-10.3 y hay una tabla comparando los recuentos. |
| 15 | DESPLIEGUE | Desplegar Laravel y MySQL en AWS Learner Lab sin subir secretos a GitHub. | H5 | RQ-10 | 8 h | Pendiente | La aplicación funciona en AWS y con los pasos apuntados se puede repetir el despliegue. |
| 16 | PRUEBAS | Pasar todas las pruebas y criterios de aceptación con la versión final. | H6 | Todas | 3 h | Pendiente | Se cumplen todos los criterios obligatorios y los resultados están documentados. |
| 17 | ENTREGA | Manuales, memoria, README y etiqueta de la entrega final. | H6 | PRUEBAS | 5 h | Pendiente | Otra persona puede arrancar la versión final siguiendo el README. |

**En total salen unas 80 horas** para lo obligatorio. Lo deseable (RQ-11 y RQ-12) serían unas 6 horas más, pero solo lo haré si he terminado H2 y H3.

He separado RQ-09 en dos partes porque el inicio de sesión y la lista de eventos los necesito desde el principio para poder probar los permisos y el programa. El resto de funciones que se conservan pueden esperar a T4.

## Seguimiento

- Cuando termine una tarea, cambiaré su estado en esta tabla en el mismo commit.
- Si una tarea me lleva bastante más de lo previsto, apuntaré por qué y revisaré las siguientes. Si peligra una entrega, quitaré primero lo deseable.
- Los riesgos de tiempo y organización (RS-05 y RS-06) están en [05-riesgos.md](05-riesgos.md).
