# Solución para el error de acceso a la base de datos

El error `P1010: User was denied access on the database` indica que el usuario de PostgreSQL no tiene permisos.

## Soluciones:

### Opción 1: Verificar y crear la base de datos (recomendado)

Ejecuta estos comandos en tu terminal (necesitas acceso a psql):

```bash
# Conectarse como superusuario de PostgreSQL
psql -U postgres

# Crear la base de datos si no existe
CREATE DATABASE school_saas;

# Otorgar todos los permisos al usuario postgres
GRANT ALL PRIVILEGES ON DATABASE school_saas TO postgres;

# Conectarse a la base de datos y otorgar permisos en el schema
\c school_saas
GRANT ALL ON SCHEMA public TO postgres;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO postgres;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO postgres;

# Salir
\q
```

### Opción 2: Verificar las credenciales en .env

Asegúrate de que el archivo `.env` tenga las credenciales correctas:

```env
DATABASE_URL="postgresql://USUARIO:CONTRASEÑA@localhost:5432/school_saas?schema=public"
```

### Opción 3: Usar un usuario diferente

Si tienes otro usuario con permisos, actualiza el `.env`:

```env
DATABASE_URL="postgresql://tu_usuario:tu_contraseña@localhost:5432/school_saas?schema=public"
```

### Opción 4: Verificar que PostgreSQL esté corriendo

```bash
# macOS (con Homebrew)
brew services list | grep postgresql

# O verificar el proceso
pg_isready -h localhost -p 5432
```

