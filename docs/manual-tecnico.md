# Manual Técnico - Clasificador de Commits con IA Local

## 1. Arquitectura del Sistema
- **Cliente:** Navegador web / Postman
- **API REST:** FastAPI (Puerto `8000`)
- **Base de Datos:** PostgreSQL 16 Alpine (Puerto `5432`)
- **Motores de Inferencia:**
  - **Motor Eco:** Implementación por reglas regex (sin consumo de RAM).
  - **Motor Ollama:** Modelo local `qwen2.5-coder:1.5b` (Puerto `11434`).

## 2. Política de Seguridad y Roles
- **Variables de Entorno:** Credenciales gestionadas exclusivamente vía `.env`.
- **Principio de Privilegio Mínimo:** 
  - Rol de aplicación `app_ia` restringido únicamente a permisos de `SELECT` e `INSERT` sobre la tabla `inferencias`.
  - Intento de ejecución de sentencias como `DELETE` o `DROP` es denegado por PostgreSQL.

## 3. Endpoints de la API
- `GET /health`: Estado del servicio y conectividad con la base de datos.
- `POST /clasificar`: Recibe el texto del commit y el motor a utilizar (`eco` u `ollama`), retorna la categoría (`feat`, `fix`, `docs`, `test`, `chore`, `refactor`) y registra la inferencia.
- `GET /inferencias`: Consulta el historial de inferencias almacenadas.

## 4. Modelo de Datos
Tabla `inferencias`:
- `id`: SERIAL PRIMARY KEY
- `fecha`: TIMESTAMP NOT NULL
- `motor`: VARCHAR(20) NOT NULL
- `modelo`: VARCHAR(120) NOT NULL
- `entrada`: TEXT NOT NULL
- `salida`: TEXT NOT NULL
- `latencia_ms`: INTEGER NOT NULL

## 5. Plan de Respaldo y Restauración
- **Creación de copia:** `docker compose exec -T db pg_dump -U postgres iadb > backups/respaldo.sql`
- **Restauración:** `cat backups/respaldo.sql | docker compose exec -T db psql -U postgres -d iadb`

## 6. Decisiones de Diseño y Limitaciones
- **Dockerfile Multi-etapa:** Reduce el tamaño de la imagen final copiando únicamente los binarios instalados.
- **Usuario no-root:** Contenedor ejecuta bajo `appuser` (UID 1001) por seguridad.
- **Limitación de Latencia:** En inferencias con Ollama, la latencia depende del hardware local. El Motor Eco sirve como alternativa de alta disponibilidad.
