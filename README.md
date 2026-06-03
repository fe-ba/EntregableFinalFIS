## Ferney Eduardo Ballesta Avila 20231020141  &  Mariam Betin Escobar 20232020300


# Hogar Viejito Canoso — Sistema de administración

Aplicación web para la gestión de un hogar gerontológico. Desarrollada con Python/Flask y PostgreSQL.

---

## Lo que necesitas tener instalado antes de empezar

1. **Python 3.10 o superior** — https://www.python.org/downloads/
   - Durante la instalación en Windows, marcar la casilla **"Add Python to PATH"**
   - Para verificar que está instalado: `python --version`

2. **PostgreSQL 14 o superior** — https://www.postgresql.org/download/
   - Anotar la contraseña que se asigna al usuario `postgres` durante la instalación
   - Para verificar que está instalado: `psql --version`

---

## Paso 1 — Extraer el zip

Descomprimir el archivo en la carpeta que se prefiera. La estructura resultante debe ser:

```
proyecto/
├── README.md
└── src/
    ├── app.py
    ├── config.py
    ├── init_db.py
    ├── requirements.txt
    ├── .env
    ├── models/
    ├── routes/
    └── static/
```

---

## Paso 2 — Abrir una terminal en la carpeta `src`

**Windows:** clic derecho dentro de la carpeta `src` → "Abrir en Terminal" (o buscar `cmd` en el menú inicio y navegar con `cd ruta\al\proyecto\src`).

**Linux / macOS:**
```bash
cd ruta/al/proyecto/src
```

Todos los comandos del resto de esta guía se ejecutan desde dentro de `src`.

---

## Paso 3 — Crear y activar el entorno virtual

```bash
python -m venv venv
```

Activar:

| Sistema | Comando |
|---|---|
| Windows (cmd) | `venv\Scripts\activate` |
| Windows (PowerShell) | `venv\Scripts\Activate.ps1` |
| Linux / macOS | `source venv/bin/activate` |

Cuando esté activo, el prompt mostrará `(venv)` al inicio.

---

## Paso 4 — Instalar las dependencias

Con el entorno virtual activo:

```bash
pip install -r requirements.txt
```

Esto instala Flask, SQLAlchemy, psycopg2 y el resto de paquetes necesarios. Puede tardar un minuto.

---

## Paso 5 — Crear la base de datos en PostgreSQL

Abrir una terminal y conectarse a PostgreSQL:

```bash
psql -U postgres
```

Si pide contraseña, ingresar la que se configuró al instalar PostgreSQL.

Una vez dentro, ejecutar:

```sql
CREATE DATABASE hogares_gerontologicos;
\q
```

---

## Paso 6 — Configurar la conexión en el archivo `.env`

El archivo `src/.env` ya viene incluido con valores por defecto:

```
SECRET_KEY=dev-secret-hogares-2024
DATABASE_URL=postgresql://postgres@localhost:5432/hogares_gerontologicos
```

**Si PostgreSQL tiene contraseña**, editar `DATABASE_URL` así:

```
DATABASE_URL=postgresql://postgres:TU_CONTRASEÑA@localhost:5432/hogares_gerontologicos
```

Reemplazar `TU_CONTRASEÑA` por la contraseña real del usuario `postgres`.

Si no se configuró contraseña al instalar (común en Linux), el archivo no necesita cambios.

---

## Paso 7 — Inicializar la base de datos

Desde la carpeta `src`, con el entorno virtual activo:

```bash
python init_db.py
```

Salida esperada:

```
Tablas creadas.
Usuario administrador creado: documento=0000000001, contraseña=Admin1234
Roles creados: ADMINISTRADOR_USUARIOS, ADMINISTRADOR_HOGARES, ADMINISTRADOR_RESIDENTES
Inicialización completada.
```

> Este paso solo hay que hacerlo una vez. Si se ejecuta de nuevo, no duplica datos.

---

## Paso 8 — Arrancar la aplicación

```bash
python app.py
```

Salida esperada:

```
 * Running on http://127.0.0.1:5000
 * Running on http://0.0.0.0:5000
```

Abrir el navegador en **http://localhost:5000**

Para detener la aplicación: `Ctrl + C` en la terminal.

---

## Páginas disponibles

| URL | Descripción |
|---|---|
| `http://localhost:5000` | Pantalla de login |
| `http://localhost:5000/preinscripcion` | Formulario público de preinscripción |

---

## Credenciales iniciales

| Rol | Documento | Contraseña |
|---|---|---|
| Administrador de usuarios | `0000000001` | `Admin1234` |

Desde ese usuario se crean los demás usuarios del sistema.

### Contraseña automática para nuevos usuarios

Al crear un usuario, el sistema genera su contraseña inicial automáticamente:

```
PrimerNombre (primera letra mayúscula) + primeros 3 dígitos del documento + últimos 3 dígitos
```

Ejemplo — nombre `Pedro`, documento `3344556677` → contraseña `Pedro334677`

La contraseña aparece una sola vez en pantalla al confirmar la creación.

---

## Roles del sistema

| Rol | Qué puede hacer |
|---|---|
| **Administrador de usuarios** | Crear, editar y retirar usuarios del sistema |
| **Administrador de hogares** | Crear, editar y eliminar hogares y sus habitaciones |
| **Administrador de residentes** | Registrar adultos mayores, gestionar su estado y preinscripciones |

---

## Solución de problemas frecuentes

**`psycopg2.OperationalError: could not connect to server`**
→ PostgreSQL no está corriendo. Iniciarlo desde Servicios (Windows) o con `sudo systemctl start postgresql` (Linux).

**`psycopg2.OperationalError: password authentication failed`**
→ La contraseña en `DATABASE_URL` no es correcta. Verificar el paso 6.

**`sqlalchemy.exc.OperationalError: database "hogares_gerontologicos" does not exist`**
→ Falta crear la base de datos. Volver al paso 5.

**`ModuleNotFoundError: No module named 'flask'`**
→ El entorno virtual no está activo. Repetir el comando `activate` del paso 3.

**PowerShell dice "la ejecución de scripts está deshabilitada"**
→ Ejecutar primero: `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`

---

## Dependencias

| Paquete | Versión |
|---|---|
| Flask | 3.1.3 |
| Flask-SQLAlchemy | 3.1.1 |
| Flask-Bcrypt | 1.0.1 |
| Flask-Login | 0.6.3 |
| SQLAlchemy | 2.0.50 |
| psycopg2-binary | 2.9.12 |
| python-dotenv | 1.2.2 |
# EntregableFinalFIS
