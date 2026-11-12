# Sistema de gestión de restaurante

Repositorio del proyecto académico de gestión de un restaurante o bufet en Nicaragua.

## Alcance de esta entrega

Este repositorio contiene la migración SQL inicial de la base de datos PostgreSQL alojada en Supabase (`RestauranteDB`). El script crea la estructura y los objetos de base de datos definidos por el proyecto e incluye datos de prueba y configuración de roles/permisos.

## Tecnologías

- PostgreSQL: motor relacional de base de datos.
- Supabase: plataforma donde está alojada la instancia PostgreSQL.
- ASP.NET Core: capa de aplicación que desarrollará el equipo.
- Figma: prototipo de la interfaz.

## Estructura

```
Restaurante-Bufet/
├── DATABASE/
│   ├── migrations/
│   │   └── 00_Migracion_Completa_pg.sql
│   └── README.md
├── DOCUMENTACION/
│   ├── Relaciones.md
│   └── Seguridad.md
├── .gitignore
└── README.md
```

## Contenido de la base de datos

| Objeto | Cantidad |
|---|---|
| Tablas | 25 |
| Funciones CRUD (`sp_...`) | 48 |
| Triggers | 6 (con 6 funciones de trigger) |
| Vistas de reportes e inventario | 9 |
| Roles | 3 (Administrador, Cajero, Mesero) |

Las 25 tablas tienen RLS activado. Las funciones son `SECURITY DEFINER` con `search_path = public`.

## Estado de la base de datos

La migración inicial ya fue ejecutada en el proyecto Supabase del equipo. Este archivo se conserva como referencia reproducible y respaldo del esquema inicial. **No se debe volver a ejecutar sobre la base de datos compartida ya creada**, porque contiene instrucciones de creación de objetos que podrían fallar al existir previamente.

Para una instalación limpia, usar un proyecto/base de datos de desarrollo vacío y revisar primero el script completo. Ejecutarlo desde Supabase SQL Editor o mediante una conexión PostgreSQL autorizada.

## Lo que todavía no está incluido

- **Función de login.** Ninguna función entrega el hash de la contraseña; está pendiente `sp_ObtenerCredencialesUsuario`.
- **CRUD del módulo de inventario.** Las tablas de ingredientes, inventario, lotes, compras, recetas, mermas, movimientos, proveedores y almacenes existen y tienen datos de prueba, pero aún no tienen funciones de escritura. Solo hay vistas de lectura.
- **Descuento de inventario al cobrar.** Pendiente de decisión de diseño.
- **Contraseñas de prueba.** Los usuarios de prueba se entregan por canal privado y deben cambiarse antes de publicar el sitio.

## Cómo llamar a las funciones

Las funciones se invocan con `SELECT * FROM`. PostgreSQL exige que los parámetros opcionales vayan al final de la firma, por lo que el orden de parámetros cambió respecto a los procedimientos originales de SQL Server. **Usar siempre parámetros por nombre** para evitar errores de orden:

```sql
SELECT * FROM sp_ConsultarClientes();
SELECT * FROM sp_InsertarCargo(p_nombreCargo => 'Supervisor', p_descripcion => 'Coordina turnos');
```

Los nombres de columnas se guardan en minúsculas (`idCliente` se devuelve como `idcliente`). Los errores de validación se lanzan con `RAISE EXCEPTION` y un mensaje en español.

## Conexión desde ASP.NET Core

El equipo de backend debe acordar una sola estrategia de conexión:

1. Conexión directa a PostgreSQL mediante Npgsql (recomendable cuando ASP.NET Core ejecuta consultas SQL/funciones directamente).
2. Consumo de la API de Supabase (si el backend decide usar los endpoints de Supabase).

No mezclar ambas estrategias sin una decisión explícita de arquitectura. Solicitar las credenciales por un canal privado y configurar los secretos mediante variables de entorno o User Secrets de .NET.

**Nunca subir al repositorio:** contraseña de `postgres`, cadena de conexión real, `service_role key`, tokens, ni archivos `.env` con secretos. La clave `anon`/publishable tampoco debe confundirse con la clave `service_role`; revisar las políticas antes de publicar cualquier valor.

## Flujo de cambios

- La migración inicial se conserva sin editar después de haber sido aplicada.
- Cada cambio futuro se agrega en un archivo SQL nuevo, con prefijo numérico y descripción, por ejemplo `01_ajuste_indice_productos.sql`.
- Probar primero en una base de datos de desarrollo antes de aplicar cambios en la compartida.
- Documentar cambios incompatibles y respaldar datos antes de modificar o eliminar columnas/tablas.

## Documentación

Consultar `DATABASE/README.md` y los documentos dentro de `DOCUMENTACION/`.
