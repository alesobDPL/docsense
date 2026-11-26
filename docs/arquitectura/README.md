# Arquitectura

[Volver al inicio](../00-inicio.md)

Visión general del sistema: componentes, cómo se conectan, stack aprobado y convenciones (dónde va cada cosa). Se mantiene al día cuando un ADR lo cambia.

Estado: describe el **MVP existente** (en `main`), relevado el 2026-11-25. El rediseño de UI/UX y la funcionalidad destacada se decidirán en su spec; hasta entonces, nada de lo de abajo es un compromiso hacia adelante.

Las reglas de alcance, calidad y proceso están en los [criterios de desarrollo](criterios-desarrollo.md).

## Componentes

```
Streamlit (frontend/app.py) ──HTTP──▶ FastAPI (backend/main.py)
                                         │
                ┌────────────────────────┼─────────────────────────┐
                ▼                        ▼                         ▼
     core/orchestrator.py          core/rag.py               core/pdf.py
     intención · plan ·            recuperación híbrida      extracción por página
     guardrails · citas            (embeddings + léxico)     chunking (tiktoken)
                │                        │
                ▼                        ▼
          core/llm.py            core/vectorstore.py ──▶ vstore_chroma.py (./data/chroma)
     OpenAI o FAKE_LLM                                   vstore_mini.py  (./data/mini_store)
```

| Módulo | Responsabilidad |
| --- | --- |
| `backend/main.py` | Endpoints: `/healthz`, `/version`, `/ingest`, `/chat`, `/summary`, `/compare`, `/compare/global`, `/classify`, `/collections/{c}/docs` (GET), `/collections/{c}` (DELETE), `/debug/retrieval`. Además contiene la lógica de resumen, comparación y clasificación. |
| `core/orchestrator.py` | `run_chat`: detecta la intención por palabras clave (answer/summarize/compare/classify), arma el plan de recuperación desde variables de entorno, reordena, aplica guardrails (confianza, cobertura, diversidad de fuentes), cae en un modo débil o se abstiene, y fuerza citas `(Doc:<título>, pág <n>)`. Devuelve métricas de tiempo. |
| `core/rag.py` | `retrieve`: búsqueda por embeddings más un refuerzo léxico para preguntas por entidades o cuando la confianza es baja. |
| `core/llm.py` | `embed` y `chat` con OpenAI; con `FAKE_LLM=1`, versiones deterministas para pruebas sin clave. |
| `core/pdf.py` | Extracción con pdfplumber y chunks de 700 tokens con solape de 220. |
| `core/vectorstore.py` | Elige el almacén según `VSTORE_BACKEND` (`chroma` o `mini`; por defecto `mini`). |
| `frontend/app.py` | Una sola página: colección, subida de hasta 5 PDFs, lista de documentos, pestañas de resumen, comparación por pares, comparación global y clasificación, y chat. |

## Flujos

- **Ingesta:** PDF en memoria → páginas → chunks → embeddings → upsert con id `<hash_doc>|p<página>|c<chunk>|<hash_chunk>` y metadatos `doc_title`, `page`, `chunk_index`, `doc_id`.
- **Chat:** pregunta → intención → plan → recuperación híbrida → reordenamiento → guardrails → síntesis con citas (reintenta si falta la cita).
- **Comparación global:** centroides por documento → matriz de similitud coseno → umbral automático (P75) → pareja más similar, grupos (componentes conexas) y hasta 10 comparaciones redactadas por el LLM.
- **Clasificación:** KMeans sobre los embeddings de los chunks, con k automático por silueta → nombre de cada tópico con el LLM → mezcla de tópicos por documento.

## Configuración

Variables en `.env` (ver `.env.example`): `OPENAI_API_KEY`, `VSTORE_BACKEND`, `RAG_TOPK`, `RERANK_TOPK`, `MIN_SCORE`, `MIN_MEAN_TOP`, `MIN_COVERAGE`, `MIN_DIVERSITY`, `MAX_CTX_CHARS`, `FORCE_CITATIONS`, `ALLOW_WEAK_FALLBACK`, `BACKEND_URL`, `APP_VERSION`.

## Deuda técnica conocida

- `/debug/retrieval` está definido dos veces en `backend/main.py`.
- Los endpoints reciben `dict` sin modelos Pydantic; no hay contrato de API tipado.
- `main.py` mezcla rutas con lógica de negocio (resumen, comparación, clasificación).
- `rag.augment` y `rag.answer` no se usan.
- El reordenamiento solo ordena por score (falta el cross-encoder).
- La intención se detecta por palabras clave.
- `@app.on_event("startup")` está obsoleto en FastAPI.
- CORS abierto a todos, sin autenticación ni límites de uso.
- Las llamadas al LLM son síncronas dentro de los endpoints: la comparación global y la clasificación pueden tardar.
- Sin OCR para PDFs escaneados.
- Si no se define `VSTORE_BACKEND`, se usa el almacén `mini` en lugar de Chroma.
- `requirements.txt` de la raíz es un volcado antiguo en UTF-16 que nada usa.
- La UI es una sola página de Streamlit que muestra JSON y diccionarios crudos en varios lugares.
