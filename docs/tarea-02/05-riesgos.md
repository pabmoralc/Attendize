# Tarea 2 - Riesgos

- **Probabilidad e impacto:** bajo, medio o alto.
- **Responsable:** yo (Pablo), porque el trabajo es individual.
- **Revisión:** revisaré la tabla al terminar cada tarea (T3 a T6).

| Código | Riesgo | Probabilidad | Impacto | Cómo prevenirlo | Qué hacer si pasa | Responsable |
|---|---|---|---|---|---|---|
| **RS-01** | **Tecnología antigua.** Attendize usa versiones sin soporte (PHP 7.3, Laravel 6.5, MySQL 5.7). Al sacar los datos o compararlo con la versión nueva pueden salir errores, como ya pasó en la Tarea 1 (H3, H4). | Alta | Medio | No actualizar Attendize, sino hacer un proyecto nuevo (ver documento 04). Dejar Attendize sin tocar en `tarea-01-entrega` y usarlo solo para sacar los datos. | Apuntar el error en el README, buscar la causa y hacer el cambio mínimo, como en la Tarea 1. | Pablo |
| **RS-02** | **Perder o estropear datos en la migración**: que falten registros, se rompan relaciones o se dupliquen datos. | Media | Alto | Hacer una copia con `mysqldump` antes de cada prueba y probar primero sobre una copia. Hacer la migración de forma que se pueda repetir. | Restaurar la copia, mirar qué registros han fallado y corregir el script. Comparar cuántos registros hay antes y después. | Pablo |
| **RS-03** | **Seguridad.** Que un usuario vea datos de otra cuenta porque el permiso solo se comprueba en la interfaz, o subir sin querer contraseñas o claves a GitHub (`.env`, claves de AWS). | Media | Alto | Comprobar los permisos en Laravel y hacer pruebas con un segundo organizador (CA-05.x). Tener el `.env` en `.gitignore` y en `.env.example` solo valores de ejemplo. Mirar `git status` antes de cada commit. | Corregir el permiso y añadir una prueba para ese caso. Si se sube una contraseña o clave, cambiarla en el momento y borrarla del historial. | Pablo |
| **RS-04** | **Dependencias que no funcionan juntas**: versiones de Laravel, React, Inertia, TypeScript y Vite incompatibles. | Media | Medio | Usar las versiones que diga el profesor y subir `composer.lock` y `package-lock.json`. Empezar desde el kit oficial de Laravel con React. | Volver al último commit que funcionaba y actualizar las dependencias de una en una. | Pablo |
| **RS-05** | **Falta de tiempo o de experiencia** con Laravel, React e Inertia. | Alta | Alto | Hacer primero lo obligatorio y dividir el trabajo en hitos pequeños (documento 07). Lo deseable, solo al final. | Quitar primero lo deseable y lo opcional. Si me quedo atascado más de una sesión de trabajo, preguntar al profesor. | Pablo |
| **RS-06** | **Organización**: que coincida con entregas de otros módulos o con el trabajo. | Media | Medio | Planificar cada semana con la tabla del documento 07, reservar horas fijas y hacer commits pequeños. | Reordenar las tareas y avisar al profesor si no llego a una fecha. | Pablo |
| **RS-07** | **Despliegue en AWS Learner Lab**: la sesión del laboratorio caduca, el crédito es limitado y hay servicios que no se pueden usar. | Media | Alto | Apuntar los pasos del despliegue, parar los recursos al terminar, usar instancias pequeñas y guardar copias de la base de datos fuera del laboratorio. | Volver a desplegar siguiendo los pasos apuntados. Si un servicio no está disponible, hablar con el profesor para buscar otra forma. | Pablo |
| **RS-08** | **Entorno local muy lento**: cada página tarda unos 22 s (H5) y se pierde mucho tiempo al probar. | Alta | Medio | Probar a ejecutar el proyecto nuevo dentro de WSL2 en vez de desde una carpeta de Windows. | Si sigue lento, programar con el servidor de Laravel y Vite sin Docker y usar Docker solo para comprobar al final. | Pablo |

## Los que más me preocupan

Son los que tienen probabilidad o impacto alto:
- **RS-05**, el tiempo;
- **RS-02**, los datos;
- **RS-03**, la seguridad;
- **RS-07**, AWS.

Por eso en la planificación hay tareas pensadas para ellos: las pruebas de permisos en el hito H2, y la migración y el despliegue en H5.
