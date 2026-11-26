# docsense

Chat con respuestas citadas sobre colecciones de PDFs: preguntas, resúmenes, comparaciones y clasificación por temas.

## Estado actual · actualizado 2026-11-25

- **Fase:** descubrimiento
- **Funcionalidad activa:** ninguna (spec: — · plan: — · rama: —)
- **Lote en curso:** —
- **Último avance:** MVP adoptado (F0 en `main`): arquitectura y deuda técnica documentadas en [arquitectura/](arquitectura/README.md), pruebas 5/5 con `FAKE_LLM=1`. [Hoja de ruta](hoja-de-ruta.md) **Propuesta**.
- **Bloqueos:** ninguno.
- **Siguiente paso:** aprobar la hoja de ruta y hacer el brainstorming de F1/F2 (nueva UI/UX y funcionalidad destacada) → subagente Opus.
- **Última bitácora:** adopción (2026-11-25, privada).

| Encargo | Lote | Estado | Siguiente paso |
| --- | --- | --- | --- |

<!-- Fases: descubrimiento | diseño | plan | implementación | revisión | despliegue.
     Este bloque es el índice; el detalle de cada encargo vive en su Seguimiento (docs/privado/encargos/).
     Mantenerlo en unas 20 líneas. Repo público: si es público, sin datos sensibles aquí. -->

## Mapa de la documentación

| Carpeta | Contenido | ¿Va a GitHub? |
| --- | --- | --- |
| [hoja-de-ruta.md](hoja-de-ruta.md) | Funcionalidades en orden, con su estado: cuánto falta para terminar | Sí |
| [arquitectura/](arquitectura/README.md) | Visión general, convenciones y [criterios de desarrollo](arquitectura/criterios-desarrollo.md) | Sí |
| [decisiones/](decisiones/README.md) | Un ADR por decisión de arquitectura | Sí |
| [superpowers/specs/](superpowers/specs/README.md) | Diseños (brainstorming) | Sí |
| [superpowers/plans/](superpowers/plans/README.md) | Planes de implementación con lotes (writing-plans) | Sí |
| [privado/](privado/README.md) | Bitácora, encargos y sus ejecuciones, pruebas, infraestructura y producto | No: repo git propio |

## Estado de las decisiones

- **Pendiente:** no hay decisión; no usarla como requisito.
- **Propuesto:** alternativa presentada, sin aprobación.
- **Aprobado:** acuerdo explícito del propietario, con fecha y contexto.
