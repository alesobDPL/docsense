# Hoja de ruta · docsense

[Volver al inicio](00-inicio.md)

Estado: **Propuesto** el 2026-11-25. Dice **qué** se construye y en qué orden, sin detalle de implementación: cada funcionalidad tiene su propio spec y plan justo antes de implementarse.

## Funcionalidades

| # | Funcionalidad | Depende de | Terminada cuando | Estado | Spec · Plan · Rama |
| --- | --- | --- | --- | --- | --- |
| F0 | MVP: ingesta de hasta 5 PDFs, chat con citas y guardrails, resúmenes, comparación por pares y global, clasificación por temas, Docker y CI | — | Ya funciona: `docker compose up` levanta todo y las pruebas pasan | en main | — · — · `main` |
| F1 | Nueva identidad visual y UI/UX (alcance y tecnología de UI por decidir en el brainstorming) | F0 | El propietario recorre las pantallas clave en escritorio y móvil y las aprueba; capturas nuevas en `docs/img/` | pendiente | — |
| F2 | Funcionalidad destacada para el portafolio (por decidir en el brainstorming) | F0; F1 si toca la UI | Criterio observable que fije su spec | pendiente | — |
| F3 | README definitivo con capturas nuevas y demo | F1, F2 | README publicado en `main` | pendiente | — |

<!-- Estados: pendiente | diseño | plan | en curso | integrada | en main | descartada.
     "integrada": todos sus lotes están en la rama de la funcionalidad; "en main": solo tras el merge autorizado.
     Una funcionalidad activa a la vez (diseño, plan o en curso), salvo que el propietario apruebe otra cosa.
     Claude actualiza el Estado y los enlaces al cambiar de fase. -->

El orden entre F1 y F2, y si conviene unirlas en un solo spec, se decide en el brainstorming.

## Fuera del MVP

- Deuda técnica de [arquitectura](arquitectura/README.md#deuda-técnica-conocida): entra en F1 o F2 solo si su spec lo pide.
- OCR para PDFs escaneados, reordenamiento con cross-encoder, autenticación, despliegue público.
