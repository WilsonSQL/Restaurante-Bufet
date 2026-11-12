# Seguridad y configuración

- No guardar secretos ni credenciales en Git.
- No conectar la aplicación con el superusuario `postgres`.
- Crear/usar un rol de aplicación con privilegios mínimos.
- Revisar RLS tabla por tabla y probar las políticas con el tipo de conexión real del backend.
- Las funciones `SECURITY DEFINER` requieren revisión de propietario, `search_path`, permisos de ejecución y validación de entradas.
- Sustituir las contraseñas de prueba basadas en SHA-256 sin sal por un mecanismo moderno de hashing de contraseñas antes de producción.
- Mantener secretos en variables de entorno o .NET User Secrets durante desarrollo.
