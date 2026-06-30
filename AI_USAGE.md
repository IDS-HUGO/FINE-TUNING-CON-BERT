# AI_USAGE.md — Lab 6: Fine-tuning de BERT

## Herramienta utilizada
- **Claude (claude.ai)** — modelo Claude Sonnet

---

## Uso declarado

| Sección | Celda / actividad | Qué proporcionó la IA | Qué cambié o verifiqué yo |
|---------|-------------------|------------------------|---------------------------|
| 0 | `corpus_crudo.json` | No conservaba el texto crudo original del Lab 1, así que reconstruí 1-2 oraciones por documento consistentes con título y tokens lematizados | Verifiqué que cada texto reconstruido contuviera literalmente los conceptos clave de los `tokens` (p. ej. d02 menciona "hídrica", "líquido vital", "escasez") para que la inferencia de B.5/C.5 fuera representativa |
| A.1–A.2 | Pipeline de `SentenceTransformer.fit` con `MultipleNegativesRankingLoss` | Sintaxis de `InputExample`, `DataLoader` y parámetros de `fit` | Decidí el umbral `grado >= 2` para construir pares positivos desde qrels, consistente con cómo se interpretó la relevancia graduada en el Lab 3 |
| B.2 | Alineación de `tokenizar_batch` | Patrón estándar de `truncation + padding` con `AutoTokenizer` | Verifiqué `max_length=128` es razonable para tuits cortos |
| C.2 | `tokeniza_y_alinea` (alineación de subpalabras con `-100`) | Patrón canónico de Hugging Face para NER (usar `word_ids()` y marcar solo la primera subpalabra) | Revisé la lógica contra la documentación oficial de `transformers` para confirmar el manejo de `[CLS]`/`[SEP]`/padding |
| B.4, C.4 | Redacción de las respuestas en negritas sobre F1-macro vs. accuracy y seqeval vs. accuracy por token | Borrador de la justificación | Verifiqué el razonamiento contra el comportamiento esperado del esquema BIO y de datasets desbalanceados antes de aceptarlo |

---

## Lo que NO se generó con IA

- Las qrels del Lab 3 reutilizadas en A.1: etiquetadas manualmente en esa entrega anterior.
- La elección de documentos del corpus usados como demo en B.5 y C.5 (d02/d03/d07/d09), elegida a propósito para ilustrar domain shift y tipos de entidad esperables.
- Este mismo documento.

## Nota de entorno

Este notebook **no se ejecutó con salidas reales** en este entorno de trabajo: requiere GPU T4 y
acceso sin restricciones a Hugging Face Hub (modelos y datasets), mientras que el entorno usado
para construirlo tiene red en lista blanca y sin GPU. El código fue verificado por sintaxis
(`ast.parse` sobre cada celda) y por consistencia lógica con los Labs 1, 3 y 5, pero **debe
ejecutarse íntegramente en Google Colab antes de subir el notebook**, tal como exige el enunciado.

---

*Hugo Francisco Luis Inclán — UPCh 2026A — Minería de Datos*
