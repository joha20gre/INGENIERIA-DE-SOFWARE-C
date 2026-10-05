# SIGEPP – Sistema de Gestión de Equipos de Protección Personal

![CI SIGEPP](https://github.com/USUARIO/sigepp/actions/workflows/ci.yml/badge.svg)

Aplicación web (Python + Flask + SQLite) para registrar la dotación de EPP del personal de un
taladro de workover: trabajadores, inventario, entregas, fechas de reposición y alertas.

Proyecto integrador de la asignatura **Ingeniería de Software (UEA-L-UFPTI-011)**,
Universidad Estatal Amazónica.

## Funciones
- Registro de trabajadores con validación de cédula ecuatoriana (módulo 10).
- Inventario de EPP con stock mínimo, talla y vida útil; reabastecimiento.
- Registro de entregas con descuento de stock y cálculo de la fecha de reposición.
- Bloqueo de entregas sin stock (transacción con rollback).
- Panel con alertas de stock bajo y de EPP vencido o por vencer (30 días).
- API JSON: `GET /api/alertas`, `GET /salud`.

## Instalación y ejecución
```bash
python -m venv .venv
# Windows: .venv\Scripts\activate    |   Linux/macOS: source .venv/bin/activate
pip install -r requirements-dev.txt
flask --app app seed-db      # crea la base con datos de ejemplo
flask --app app run --debug  # abre http://127.0.0.1:5000
```

## Pruebas (tres niveles)
```bash
pytest tests/unit          # pruebas unitarias (modelo de dominio)
pytest tests/integration   # servicios + SQLite
pytest tests/system        # flujos HTTP completos
pytest --cov=app --cov-report=term-missing   # todo + cobertura
flake8 app tests           # análisis estático
```

## Integración continua
`.github/workflows/ci.yml` se ejecuta en cada `push` y `pull request` a `main`, con Python 3.11
y 3.12: instala dependencias → flake8 → pruebas unitarias → integración → sistema con
cobertura mínima de 80 %.

## Estructura
```
app/            código fuente (models, services, routes, db, plantillas)
tests/          unit/, integration/, system/
docs/           SRS, plan del proyecto, evaluación ISO/IEC 25010, diagramas UML
jira/           backlog_jira.csv para importar en Jira
scripts/        medición de rendimiento
```

## Publicar en GitHub y obtener las evidencias del informe
1. Crear un repositorio vacío en GitHub llamado `sigepp` (sin README).
2. En la carpeta del proyecto:
   ```bash
   git init -b main
   git add .
   git commit -m "SIGEPP v1.0"
   git remote add origin https://github.com/USUARIO/sigepp.git
   git push -u origin main
   ```
3. Abrir la pestaña **Actions**: el flujo «CI SIGEPP» se ejecuta solo. Capturar la ejecución en
   verde (Anexo C del informe) y reemplazar `USUARIO` en la insignia de este README.
4. En Jira Software: crear un proyecto con plantilla **Kanban** → *Configuración del proyecto* →
   *Importar incidencias desde CSV* → seleccionar `jira/backlog_jira.csv`. Mover las tarjetas a
   sus columnas y capturar el tablero (Anexo D).
