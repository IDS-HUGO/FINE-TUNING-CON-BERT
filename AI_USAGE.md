
# AI_USAGE.md — Lab 6: Fine-tuning de BERT

## Herramienta utilizada

- **Claude (claude.ai)** — modelo Claude Haiku 4.5

## Uso declarado

| Momento | Para qué consulté la IA                                                                                                                                                 | Qué hice yo                                                                                                                                                                                                                                                               |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| B.1     | `RuntimeError: Dataset scripts are no longer supported` al cargar `cardiffnlp/tweet_sentiment_multilingual`                                                           | Cargué los Parquet auto-generados por Hugging Face directamente por URL en vez de depender del loading script                                                                                                                                                             |
| B.2     | `ImportError: cannot import name 'VideoReader'` al usar `set_format(type='torch', ...)`, por incompatibilidad entre `datasets` y `torchvision` en el entorno      | Probé desinstalar`torchvision`, y al persistir el error (por quedar en `sys.modules` de una importación previa) usé `datasets.config.TORCHVISION_AVAILABLE = False` para desactivar el chequeo problemático                                                      |
| C.1     | `RuntimeError`/`ValueError` repetidos en varios mirrors de `conll2002` (`conll2002` oficial, `eriktks/conll2002`, conversión Parquet con config inconsistente) | Usé la API`datasets-server.huggingface.co/parquet` para obtener las URLs exactas de los archivos Parquet de `tomaarsen/conll2002` (config `es`) y los cargué con `load_dataset('parquet', data_files=...)`, sin depender de la resolución automática de config |
| C.5     | Duda sobre por qué el extractor de NER marcaba fragmentos de una URL (`d07`) como entidades `MISC`                                                                   | Revisé los scores de esas detecciones (0.48–0.63, muy por debajo de las entidades reales) y confirmé que es un problema de tokenización sobre texto no lingüístico, no un fallo del fine-tuning                                                                      |

## Lo que NO se generó con IA

- El código de las celdas `# TODO` (`emb_corpus`, `buscar`, `ndcg_medio`, construcción de pares desde qrels, `tokeniza_y_alinea`, configuración de `Trainer`/`TrainingArguments`, evaluación con `seqeval`).
- Las qrels del Lab 3, reutilizadas sin cambios.
- Los análisis escritos en las celdas de markdown (comparación de los tres NDCG, clase más difícil en B.4, por qué seqeval en C.4, domain shift en B.5/C.5).
- Este documento.

## Nota de entorno

Notebook ejecutado en Google Colab con GPU T4, como exige el enunciado. `corpus_crudo.json` es una
reconstrucción a partir de los títulos/tokens del corpus del Lab 1 (no se conservó el texto crudo
original); si se cuenta con el JSON real del Lab 1 con el campo `texto`, debe reemplazarse antes de
volver a correr B.5/C.5, ya que el análisis de domain shift de esas celdas depende del texto exacto
usado.

---

*Hugo Francisco Luis Inclán — UPCh 2026A — Minería de Datos*