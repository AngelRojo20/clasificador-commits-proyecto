# Clasificador de Commits de Git con IA Local

Servicio que recibe el texto de un commit de Git y determina su categoría (feat, fix, docs, test, chore, refactor) utilizando un modelo de lenguaje local (qwen2.5-coder:1.5b) vía Ollama o un motor por reglas.

## Integrantes y Perfil de Hardware
- **Desarrollador:** Miguel (Perfil A - 16 GB RAM, SSD, WSL2 Ubuntu)

## Requisitos Previos
- Docker Engine y Docker Compose Plugin
- Ollama instalado localmente
- Python 3.12+

## Instalación y Despliegue Rápido

1. Clonar el repositorio:
   git clone git@github.com:AngelRojo20/clasificador-commits-proyecto.git
   cd clasificador-commits-proyecto

2. Configurar variables de entorno:
   cp .env.example .env

3. Descargar el modelo local:
   ollama pull qwen2.5-coder:1.5b

4. Desplegar la solución con Docker Compose:
   docker compose up -d --build

## Verificación de Servicios
- Documentación interactiva (Swagger): http://localhost:8000/docs
- Estado de salud: curl http://localhost:8000/health
- Probar clasificación:
  curl -X POST "http://localhost:8000/clasificar" -H "Content-Type: application/json" -d '{"texto": "corrige el error de conexion", "motor": "eco"}'

## Solución de Problemas Frecuentes
1. Error de conexión a la base de datos: Verificar que los puertos 5432 estén expuestos en docker-compose.yml.
2. Error al ejecutar ruff en CI: Ejecutar ruff check app/ --fix localmente antes de hacer push.
3. Módulo no encontrado en pytest: Asegurar la variable PYTHONPATH=. pytest -v.
