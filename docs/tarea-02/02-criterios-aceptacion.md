# Tarea 2 - Criterios de aceptación

Formato: **Dado** [estado inicial], **cuando** [usuario y acción], **entonces** [resultado observable].

Entre corchetes pongo el tipo de caso: [Correcto], [Dato inválido], [Sin permiso] o [Vacío o límite].

## Datos de prueba

Todos los datos son ficticios:

- **Organizador A:** cuenta TechFest, con el evento TechFest 2027 (publicado) y el evento Meetup Borrador (sin publicar).
- **Organizador B:** otra cuenta (Demo) con un evento suyo.
- **TechFest 2027:** tiene 3 sesiones, 2 publicadas y 1 sin publicar.
- **Visitante:** alguien que entra sin iniciar sesión.

---

## RQ-01. Ver las sesiones de un evento propio

- **CA-01.1** [Correcto] **Dado** que el organizador A ha iniciado sesión y TechFest 2027 tiene 3 sesiones, **cuando** abre el programa del evento, **entonces** ve las 3 sesiones ordenadas por fecha y hora de inicio, con su título, horario, sala, aforo y si están publicadas.
- **CA-01.2** [Vacío o límite] **Dado** que Meetup Borrador no tiene sesiones, **cuando** A abre su programa, **entonces** sale el mensaje «Este evento todavía no tiene sesiones» y un botón para crear la primera.

## RQ-02. Crear una sesión

- **CA-02.1** [Correcto] **Dado** que A está en el programa de TechFest 2027, **cuando** crea la sesión «Introducción a Laravel» (10/03/2027, de 10:00 a 11:00, Sala 1, aforo 50, publicada), **entonces** sale el mensaje «Sesión creada correctamente», la sesión aparece en la lista y en MySQL hay un registro nuevo asociado a ese evento y a la cuenta de A.

## RQ-03. Validación en el servidor

- **CA-03.1** [Dato inválido] **Dado** el formulario de nueva sesión, **cuando** A lo envía con el título vacío, **entonces** sale un error en el campo título y no se guarda nada en MySQL.
- **CA-03.2** [Dato inválido] **Dado** el formulario, **cuando** A pone una hora de fin igual o anterior a la de inicio, **entonces** sale el error «La hora de fin debe ser posterior a la de inicio» y no se crea la sesión.
- **CA-03.3** [Dato inválido] **Dado** el formulario, **cuando** A pone un aforo de 0, negativo o que no es un número, **entonces** sale un error en el aforo y no se crea la sesión.
- **CA-03.4** [Dato inválido] **Dado** que se quita la validación del navegador (por ejemplo, borrando el atributo `required` con DevTools), **cuando** A envía datos incorrectos, **entonces** el servidor también los rechaza y no se crea la sesión.

## RQ-04. Mensajes, formulario y teclado

- **CA-04.1** [Dato inválido] **Dado** que A ha rellenado el formulario con un aforo incorrecto, **cuando** lo envía, **entonces** el resto de campos siguen con lo que había escrito y el error sale al lado del campo aforo.
- **CA-04.2** [Correcto] **Dado** el formulario de nueva sesión, **cuando** A lo usa solo con el teclado (Tab y Enter), **entonces** puede llegar a todos los campos y botones, cada campo tiene su etiqueta, se ve en todo momento dónde está el foco y, si hay un error, el foco pasa al primer campo incorrecto.

## RQ-05. Permisos

- **CA-05.1** [Sin permiso] **Dado** que el organizador B ha iniciado sesión, **cuando** escribe en el navegador la dirección del programa de TechFest 2027 (que es de A), **entonces** le sale acceso denegado (403) o no encontrado (404) y no ve ningún dato del evento.
- **CA-05.2** [Sin permiso] **Dado** que B ha iniciado sesión, **cuando** envía directamente una petición para crear una sesión en el evento de A, **entonces** se rechaza y no se crea ninguna sesión.
- **CA-05.3** [Sin permiso] **Dado** que A ha iniciado sesión, **cuando** envía la petición de crear sesión añadiendo a mano el id de una cuenta o de un evento que no es suyo, **entonces** ese dato no se tiene en cuenta o se rechaza la petición, y en ningún caso se crea la sesión en otra cuenta.
- **CA-05.4** [Sin permiso] **Dado** un visitante que no ha iniciado sesión, **cuando** intenta abrir el programa desde la dirección del panel, **entonces** le lleva a la pantalla de inicio de sesión.

## RQ-06. Programa público en React

- **CA-06.1** [Correcto] **Dado** que TechFest 2027 está publicado y tiene 2 sesiones publicadas y 1 sin publicar, **cuando** un visitante abre el programa público, **entonces** solo ve las 2 publicadas, ordenadas por hora.
- **CA-06.2** [Sin permiso] **Dado** que Meetup Borrador no está publicado, **cuando** un visitante intenta ver su programa, **entonces** le sale una página de no encontrado.
- **CA-06.3** [Correcto] **Dado** el programa público, **cuando** un visitante lo abre con un ancho de 375 px (móvil) y de 1280 px (ordenador) y lo recorre también con el teclado, **entonces** se lee bien sin tener que hacer scroll hacia los lados y puede llegar a todos los enlaces viendo dónde está el foco.

## RQ-07. API REST de sesiones publicadas

- **CA-07.1** [Correcto] **Dado** TechFest 2027 publicado con 2 sesiones publicadas, **cuando** se consulta la API de sesiones del evento, **entonces** responde con código 200 y un JSON con esas 2 sesiones (título, inicio, fin, sala y aforo), sin datos internos como la cuenta o el usuario.
- **CA-07.2** [Dato inválido] **Dado** un evento que no existe o no está publicado, **cuando** se consulta la API, **entonces** responde 404 con un JSON de error como el que está documentado.
- **CA-07.3** [Vacío o límite] **Dado** un evento publicado sin ninguna sesión publicada, **cuando** se consulta la API, **entonces** responde 200 con una lista vacía.
- **CA-07.4** [Correcto] **Dada** la vista React del programa público, **cuando** se carga, **entonces** en DevTools > Network se ve la petición a la API y lo que sale en pantalla coincide con lo que devuelve.

## RQ-08. Los datos se mantienen

- **CA-08.1** [Correcto] **Dado** que A ha creado la sesión de CA-02.1, **cuando** se hace `docker compose stop` y luego `docker compose start`, **entonces** la sesión sigue apareciendo con los mismos datos.

## RQ-09. Funciones que se conservan

- **CA-09.1** [Correcto] **Dados** los datos de prueba, **cuando** A inicia sesión, entra en el panel, ve la lista de sus eventos, abre TechFest 2027 y mira los tipos de entrada y los asistentes, **entonces** todas las pantallas cargan sin errores y con los datos que ya había.
- **CA-09.2** [Dato inválido] **Dada** la pantalla de inicio de sesión, **cuando** alguien pone una contraseña incorrecta, **entonces** sale un error y no entra.
- **CA-09.3** [Dato inválido] **Dado** el formulario para editar un evento, **cuando** A lo guarda con el título vacío, **entonces** el servidor muestra un error y el evento no cambia.
- **CA-09.4** [Correcto] **Dado** que TechFest 2027 está publicado, **cuando** un visitante abre su página pública, **entonces** ve el título, la descripción, las fechas y las entradas disponibles.

## RQ-10. Migración de datos

- **CA-10.1** [Correcto] **Dada** una copia de seguridad de la base de datos original, **cuando** se ejecuta la migración, **entonces** hay el mismo número de cuentas, usuarios, organizadores, eventos, tipos de entrada, pedidos y asistentes que en el origen, y si hay alguna diferencia está apuntada y explicada.
- **CA-10.2** [Vacío o límite] **Dado** que la migración ya se ha hecho una vez, **cuando** se vuelve a ejecutar, **entonces** el número de registros no cambia (no se duplica nada).
- **CA-10.3** [Correcto] **Dada** la copia de seguridad, **cuando** se restaura, **entonces** A puede iniciar sesión y abrir TechFest 2027 con sus datos.

---

## Requisitos deseables (si me da tiempo)

- **CA-11.1** [Correcto] **Dada** una sesión de TechFest 2027, **cuando** A le cambia la sala y la marca como no publicada, **entonces** se guardan los cambios y deja de salir en el programa público y en la API.
- **CA-12.1** [Dato inválido] **Dada** una sesión en la Sala 1 de 10:00 a 11:00, **cuando** A crea otra en la Sala 1 de 10:30 a 11:30, **entonces** sale un aviso de que se solapan. (Depende de lo que responda el cliente a la duda 1.)

---

## Relación con los requisitos y el Anexo A

| Requisito | Criterios | Criterio del caso (Anexo A) |
|---|---|---|
| RQ-01 | CA-01.1, CA-01.2 | A2 |
| RQ-02 | CA-02.1 | A2 |
| RQ-03 | CA-03.1 a CA-03.4 | A3 |
| RQ-04 | CA-04.1, CA-04.2 | A3 |
| RQ-05 | CA-05.1 a CA-05.4 | A4 |
| RQ-06 | CA-06.1 a CA-06.3 | A5 |
| RQ-07 | CA-07.1 a CA-07.4 | A5 |
| RQ-08 | CA-08.1 | A2 |
| RQ-09 | CA-09.1 a CA-09.4 | A1, A6 |
| RQ-10 | CA-10.1 a CA-10.3 | A7 |
