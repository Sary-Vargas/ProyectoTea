# ProyectoTEA

Sistema web para la gestion academica, evaluacion, evidencias, seguimiento, prediccion y reportes de estudiantes con Trastorno del Espectro Autista.

## Stack

- Frontend: React + Vite.
- Backend: Node.js + Express.
- Autenticacion: Firebase Authentication.
- Archivos: Firebase Storage.
- Base de datos operativa: MySQL.

## Estructura

```text
backend/    API, permisos, auditoria y conexion MySQL
database/   SQL base y migraciones
docs/       documentacion tecnica y reglas
firebase/   reglas y configuracion Firebase
frontend/   aplicacion web
ml/         espacio reservado para modelos IA
```

## Requisitos

- Node.js 24 o compatible.
- MySQL 8 o compatible.
- Proyecto Firebase con Authentication Email/Password y Storage.

## Configuracion

1. Crear la base de datos MySQL:

```sql
CREATE DATABASE proyectotea CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

2. Cargar el esquema:

```bash
mysql -u root -p proyectotea < database/proyectotea_mysql.sql
```

3. Crear el archivo de variables en la raiz:

```bash
copy .env.example .env
```

Completar `.env` con los datos reales. El backend y el frontend leen este unico archivo:

## Ejecucion local

Instalar dependencias:

```bash
cd backend
npm install
cd ..\frontend
npm install
```

Levantar backend:

```bash
cd backend
npm run dev
```

Levantar frontend:

```bash
cd frontend
npm run dev -- --host 127.0.0.1 --port 5180
```

URLs habituales:

- Frontend: `http://127.0.0.1:5180`
- Backend health: `http://localhost:4002/api/health`

## Ejecucion con Docker

Desde la raiz del proyecto:

```bash
docker compose up --build -d
```

Servicios esperados:

- Frontend: `http://127.0.0.1:5180`
- Backend: `http://127.0.0.1:4002/api/health`
- MySQL Docker: `127.0.0.1:3308`

Apagar el sistema:

```bash
docker compose down
```

Ver logs:

```bash
docker compose logs backend
docker compose logs frontend
docker compose logs mysql
```

## Roles

El acceso se realiza con Firebase Authentication. Los perfiles, roles y estados se guardan en MySQL en la tabla `users`.

Roles soportados:

- `director`
- `teacher`
- `parent`

El Director puede crear Profesores y Padres desde el sistema. Los usuarios creados quedan registrados en Firebase Auth y MySQL.

## Modulos principales

- Login con correo o CI.
- Usuarios.
- Aulas.
- Estudiantes.
- Actividades.
- Evidencias.
- Evaluaciones.
- Vision por Computadora.
- Predicciones y recomendaciones.
- Historial y progreso.
- Reportes PDF/CSV.
- Auditoria.
- Notificaciones internas.
- Configuracion institucional.

## Verificacion

Backend:

```bash
cd backend
npm run lint
```

Frontend:

```bash
cd frontend
npm run lint
npm run build
```

Verificacion integral desde la raiz:

```bash
node scripts/verify-system.mjs
```

Migraciones adicionales para bases ya creadas:

```bash
mysql -u root -p proyectotea < database/migrate_notifications.sql
mysql -u root -p proyectotea < database/migrate_system_settings.sql
```

## Respaldos

Crear respaldo de MySQL desde la raiz:

```bash
node scripts/backup-mysql.mjs
```

El archivo se guarda en `backups/mysql/` con fecha y hora.

Crear respaldo de Firebase Storage desde la raiz:

```bash
node scripts/backup-storage.mjs
```

Los archivos se guardan en `backups/storage/` con fecha y hora. Requiere tener `gcloud` instalado y autenticado con acceso al proyecto Firebase.

La carpeta `backups/` esta protegida por `.gitignore`.

Restaurar un respaldo:

```bash
mysql -u root -p proyectotea < backups/mysql/archivo-del-respaldo.sql
```

Desde el modulo Configuracion del Director tambien se pueden crear:

- Backup MySQL: genera un `.sql` y lo guarda en Firebase Storage.
- Backup completo: genera un `.zip` con MySQL y archivos relevantes de Firebase Storage.

Para migrar a otra laptop, se recomienda llevar el ZIP del proyecto completo y, si se requiere conservar datos reales, crear antes un backup desde Configuracion.

- `docs/PRODUCCION.md`
- `docs/VERIFICACION_SISTEMA.md`
