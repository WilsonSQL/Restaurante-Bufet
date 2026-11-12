# Base de datos PostgreSQL / Supabase

## Archivo principal

`migrations/00_Migracion_Completa_pg.sql` contiene la migración inicial compartida por el equipo.

El script tiene 2,244 líneas. Incluye, entre otros elementos, tablas, relaciones, restricciones, funciones, vistas, triggers, datos de prueba y configuración de roles/permisos. Revisar el SQL para conocer los nombres exactos y las firmas de cada función.

## Importante antes de ejecutar

- La base de datos del proyecto ya fue creada en Supabase y esta migración ya se ejecutó allí.
- No ejecutar nuevamente el archivo sobre esa misma instancia.
- Para reconstruirla, utilizar un entorno vacío y comprobar el resultado de cada sección.
- El script contiene usuarios/datos de prueba y una plantilla comentada para crear un usuario PostgreSQL. Cambiar cualquier contraseña de ejemplo antes de habilitarla.
- Las contraseñas de prueba descritas en comentarios usan SHA-256 sin sal. No utilizar ese mecanismo para autenticación real; el backend debe implementar hashing moderno con sal (por ejemplo, ASP.NET Core PasswordHasher o Argon2id) y planificar la migración de credenciales de prueba.

## Información que el backend necesita

El responsable de backend debe recibir de forma privada:

- Host y puerto de PostgreSQL (o configuración del pooler, si aplica).
- Nombre de la base de datos.
- Usuario con privilegios mínimos necesarios.
- Contraseña correspondiente.
- SSL habilitado según los parámetros de Supabase.

No compartir la contraseña `postgres` ni `service_role` en GitHub, capturas o archivos del repositorio. No usar el usuario administrador para la conexión normal de la aplicación. Definir un usuario/rol de aplicación con privilegios mínimos y probar explícitamente las políticas RLS y los permisos de las funciones.
