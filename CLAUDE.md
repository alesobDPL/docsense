# docsense · Claude Code

Claude Code es arquitecto y revisor; Codex implementa; el propietario aprueba. El proceso está en la skill `proyecto-ordenado`. Reglas comunes y comandos: [AGENTS.md](AGENTS.md).

## Mapa

- **Stack (actual, heredado del MVP):** Python 3.11 · FastAPI + Uvicorn (API) · Streamlit (UI) · ChromaDB persistente · OpenAI (`text-embedding-3-small`, `gpt-4o-mini`) · pdfplumber + tiktoken · scikit-learn · pytest · Docker Compose · GitHub Actions. El rediseño puede cambiar la UI: se decide en su spec.
- **Directorios:** `backend/main.py` (endpoints), `backend/core/` (orquestador, RAG, LLM, PDF, vector stores), `frontend/app.py` (UI Streamlit), `tests/` (pytest), `samples/` (PDFs de ejemplo), `docs/img/` (capturas), `docs/` (solo Claude)
- **Arquitectura:** [visión](docs/arquitectura/README.md) · [decisiones](docs/decisiones/README.md) · [criterios](docs/arquitectura/criterios-desarrollo.md)
- **Estado del proyecto:** solo en [docs/00-inicio.md](docs/00-inicio.md) § Estado actual y en los encargos. Nunca aquí.

## Al iniciar

Usar `proyecto-ordenado` en modo **Retomar**. No abrir specs, plans, bitácora ni código si el siguiente paso no lo exige.

## Modelo

La sesión corre en Sonnet y coordina. Al *retomemos*, ejecuta el siguiente paso de `estado.ps1` sin preguntar qué modelo usar. El propietario autoriza delegar sin preguntar en un **subagente Opus** (herramienta Agent, `model: "opus"`) el diseño, la hoja de ruta, el plan, la redacción de los encargos de cada lote, las decisiones de arquitectura, contratos, alcance, seguridad e integridad de datos, los hallazgos críticos y la revisión final. Sonnet sigue desde su dictamen. El subagente solo escribe borradores **Propuestos** en `docs/` y no aprueba nada.

## Frontend

En el diseño, usar `ui-ux-pro-max:ui-ux-pro-max` y `ui-ux-pro-max:design-system`; el resultado va en el spec. (Borrar esta sección si no hay frontend.)

## Límites

- No escribo código de aplicación, ni en la sesión ni en un subagente. Tras `writing-plans` no uso `subagent-driven-development` ni `executing-plans`: la implementación son encargos a Codex.
- Sin merge a `main`, push, despliegue ni repos remotos sin un sí explícito del propietario.
- Repo público: lo sensible solo en `docs/privado/`. Nunca versionar secretos ni `.env`.
- Distinguir siempre **Aprobado** (con fecha), **Propuesto** y **Pendiente**.
