# Manual Técnico del Proyecto

## Arquitectura
- **Cliente:** Navegador web / Postman
- **API REST:** FastAPI corriendo en el puerto 8000
- **Base de Datos:** PostgreSQL 16 Alpine en el puerto 5432
- **Motores:** 
  - Motor Eco (Reglas con regex, sin modelo)
  - Motor Ollama (Modelo local `qwen2.5-coder:1.5b` en puerto 11434)

## Seguridad
- El puerto de PostgreSQL (5432) solo expone el servicio internamente.
- Las credenciales se gestionan exclusivamente mediante variables de entorno (`.env`).
- Se utiliza un rol con privilegios mínimos (`app_ia`) con permisos restringidos de solo `SELECT` e `INSERT`.
