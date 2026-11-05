# Tarea 2 - Propuesta y planificación

En esta carpeta está la documentación de la Tarea 2 del Proyecto Intermodular. El objetivo es decidir, antes de empezar a programar:
- qué parte de Attendize v2.8.0 se va a modernizar para el caso TechFest;
- para quién;
- y cómo.

- **Rama:** `tarea-02-planificacion`
- **Inicio:** etiqueta `tarea-02-inicio` (es el mismo punto que `tarea-01-entrega`)
- **Entrega:** etiqueta `tarea-02-entrega`
- En esta tarea no he tocado el código de Attendize.

## Resumen

La parte que voy a modernizar es la **gestión del programa de sesiones** de TechFest:
- el organizador puede ver y crear las sesiones de sus eventos (título, inicio, fin, sala, aforo y si se publica);
- los visitantes pueden ver el programa publicado en una vista de React que usa una API REST.

Se hará con **Laravel, React, TypeScript, Inertia.js y MySQL**, en local con Docker Compose y, en la Fase 2, en AWS Learner Lab.

## Documentos

| N.º | Documento | Qué contiene |
|---|---|---|
| 01 | [01-propuesta-y-alcance.pdf](01-propuesta-y-alcance.pdf) | El problema (hechos y suposiciones), los perfiles, el recorrido principal, el alcance y los requisitos |
| 02 | [02-criterios-aceptacion.md](02-criterios-aceptacion.md) | Los criterios de aceptación de cada requisito y su relación con el Anexo A |
| 03 | [03-prototipo.pdf](03-prototipo.pdf) | El mapa de navegación y las pantallas en ordenador y móvil, con errores, estado vacío y uso con teclado |
| 04 | [04-arquitectura-y-datos.pdf](04-arquitectura-y-datos.pdf) | La arquitectura, cómo viaja una petición, las entidades, la correspondencia de datos y las reglas de migración |
| 05 | [05-riesgos.md](05-riesgos.md) | Los riesgos del proyecto y qué hacer con cada uno |
| 06 | [06-digitalizacion-sostenibilidad.pdf](06-digitalizacion-sostenibilidad.pdf) | La mejora digital del proceso, los indicadores y las medidas de sostenibilidad |
| 07 | [07-planificacion.md](07-planificacion.md) | Los hitos y las tareas con dependencias, horas, estado y cuándo se dan por terminadas |
| 08 | [08-validacion.md](08-validacion.md) | La revisión con el profesor y los cambios que salgan de ella |

El archivo `01-propuesta-y-alcance.md` es el texto del que sale el PDF 01, por si hay que hacer cambios.

## Relación entre documentos

Cada requisito obligatorio (RQ-01 a RQ-10):
- aparece en el documento 01 con su prioridad;
- tiene al menos un criterio en el 02;
- tiene una tarea en el 07.

## Herramientas

- Los documentos los he escrito en Markdown y los he pasado a PDF con Pandoc y Chrome.
- El prototipo y los diagramas están hechos en HTML/SVG y exportados a PDF.
- He usado IA (Claude) como ayuda para redactar y dar formato a los documentos. He revisado todo el contenido.
