## Corpus Legal y Aprendizaje Contrastivo para Recuperación de Información

## 1. El Corpus Contrastivo: TripLegal-CL

Presentamos **TripLegal-CL**, un corpus especializado diseñado para el entrenamiento de modelos de recuperación de información en el dominio legal latinoamericano.

### Metodología de Construcción
El proceso de creación del dataset siguió un pipeline de tres etapas rigurosas:
1.  **Adquisición:** Recolección de documentos legales desde repositorios públicos oficiales de varios países de Latinoamérica y organismos internacionales.
2.  **Normalización:** Conversión y limpieza de los documentos originales (PDF/HTML) a texto plano estructurado.
3.  **Procesamiento con LLM (Gemini 2.0):** Se utilizó el modelo **Gemini 2.0** de Google para el procesamiento semántico, garantizando una alta calidad en la generación de tripletas y la minería de negativos difíciles.

### Distribución Geográfica y Volumen
El corpus consta de un total de **148,637 documentos**, con una representación mayoritaria de legislación y jurisprudencia de Ecuador, complementada con datos de Colombia, México, Perú, Bolivia y la Corte Interamericana de Derechos Humanos (CIDH).

| País / Fuente | # Documentos | % del Total |
| :--- | :---: | :---: |
| **Ecuador** | 77,597 | 52.2% |
| **Colombia** | 30,835 | 20.7% |
| **México** | 17,650 | 11.9% |
| **Perú** | 12,000 | 8.1% |
| **Bolivia** | 10,000 | 6.7% |
| **CIDH** | 555 | 0.4% |
| **TOTAL** | **148,637** | **100.0%** |

> **Nota:** La diversidad geográfica permite que los modelos entrenados con *TripLegal-CL* aprendan variaciones terminológicas del español jurídico en diferentes jurisdicciones, mejorando su capacidad de generalización.


## 2. Evaluación de Línea Base (Zero-Shot)

Para establecer un punto de partida riguroso, evaluamos el desempeño *out-of-the-box* de tres arquitecturas del estado del arte antes de cualquier adaptación al dominio.

**Modelos Evaluados:**
1.  **Multilingual E5-Large:** Un modelo denso robusto para tareas generales.
2.  **BAAI BGE-M3:** Conocido por su capacidad multilingüe y soporte de contextos largos.
3.  **Google Gemma Embeddings:** Basado en la arquitectura de LLM generativo, evaluando su capacidad semántica intrínseca.

**Protocolo Experimental:**
*   **Conjunto de Prueba:** `wilfredomartel/spanish-legal-dataset (huggingface - solicitar acceso a dataset)`
*   **Dimensión:** 60,000 consultas (*queries*) con sus respectivos documentos relevantes y distractores.
*   **Métrica Principal:** NDCG@10 (Normalized Discounted Cumulative Gain).

## 3. ⚙️ Fine-Tuning: Adaptación de Dominio

La fase crítica del experimento consiste en la especialización de los modelos utilizando el corpus **TripLegal-CL**.

### Estrategia de Entrenamiento
Implementamos un esquema de **Aprendizaje Contrastivo** (Contrastive Learning) supervisado. El objetivo es minimizar la distancia vectorial entre la consulta jurídica y su ley/sentencia correcta, mientras se maximiza la distancia con documentos confusos (*hard negatives*).

*   **Función de Pérdida:** `MultipleNegativesRankingLoss` (MNRL) con escala de temperatura aprendible.
*   **Datos de Entrada:** Tripletas generadas y curadas semánticamente (gracias al procesamiento con Gemini 2.0 en la fase de construcción del corpus).

## 4. Resultados Experimentales

Evaluamos la eficacia del corpus de aprendizaje contrastivo comparando el rendimiento "Zero-shot" (línea base) frente a los modelos re-entrenados (*Fine-tuned*) en el dominio legal.

Todas las métricas se reportan sobre el conjunto de prueba `legalspanish-eval` (60k *queries*).

### Resumen de Impacto (Performance Gains)

La siguiente tabla destaca la mejora en las métricas principales de recuperación (**NDCG@10** y **MRR@10**) tras el entrenamiento contrastivo.

| Modelo | Estado | NDCG@10 | MRR@10 | Accuracy@1 |
| :--- | :--- | :---: | :---: | :---: |
| **Multilingual E5-Large** | *Base* | 0.8296 | 0.8078 | 0.7594 |
| | **Fine-tuned** | **0.8870** | **0.8709** | **0.8351** |
| | *Mejora* | *+5.74%* | *+6.31%* | *+7.57%* |
| | | | | |
| **BGE-M3** | *Base* | 0.8127 | 0.7901 | 0.7407 |
| | **Fine-tuned** | **0.9066** | **0.8927** | **0.8612** |
| | *Mejora* | *+9.39%* | *+10.26%* | *+12.05%* |
| | | | | |
| **Gemma Embeddings** | *Base* | 0.8801 | 0.8627 | 0.8228 |
| | **Fine-tuned** | **0.9438** | **0.9325** | **0.9060** |
| | *Mejora* | *+6.37%* | *+6.98%* | *+8.32%* |


### Análisis de los Resultados

1.  **Mejor Desempeño Global:** Gemma Embeddings (Fine-tuned) obtiene los mejores resultados en  tareas de recuperación  (**NDCG@10 de 0.94**).
2.  **Mayor Capacidad de Aprendizaje:** BGE-M3 presenta la evolución más notable tras el ajuste (+12.05 pp en Accuracy@1). A pesar de un inicio más bajo, su arquitectura demostró ser la más eficiente asimilando la terminología jurídica del corpus.
3.  **Efectividad del Corpus:** El incremento generalizado de rendimiento en todas las métricas y modelos confirma que los datos de entrenamiento aportan la señal semántica necesaria para especializar modelos genéricos en la parte legal en español.

---

### Desglose Detallado por Modelo

A continuación se presentan las métricas completas (Accuracy, Precision, Recall, NDCG, MRR, MAP).

#### 1. Multilingual E5-Large
Comparativa entre el modelo original y `wilfredomartel/multilingual-e5-large-es-legal-v2`.

| Métrica | Base (Zero-shot) | Fine-tuned (Legal) | Δ (Dif) |
| :--- | :---: | :---: | :---: |
| **Accuracy@1** | 0.7594 | **0.8351** | +0.0757 |
| **Accuracy@5** | 0.8699 | **0.9172** | +0.0473 |
| **NDCG@10** | 0.8296 | **0.8870** | +0.0574 |
| **MRR@10** | 0.8078 | **0.8709** | +0.0631 |
| **MAP@100** | 0.8106 | **0.8727** | +0.0621 |

#### 2. BAAI BGE-M3
Comparativa entre el modelo original y `wilfredomartel/bge-m3-es-legal-v3`. Este modelo mostró la adaptación más drástica al dominio.

| Métrica | Base (Zero-shot) | Fine-tuned (Legal) | Δ (Dif) |
| :--- | :---: | :---: | :---: |
| **Accuracy@1** | 0.7407 | **0.8612** | +0.1205 |
| **Accuracy@5** | 0.8544 | **0.9333** | +0.0789 |
| **NDCG@10** | 0.8127 | **0.9066** | +0.0939 |
| **MRR@10** | 0.7901 | **0.8927** | +0.1026 |
| **MAP@100** | 0.7930 | **0.8942** | +0.1012 |

#### 3. Google Gemma Embeddings
Comparativa entre el modelo original y `wilfredomartel/embeddinggemma-300m-legal-spanish-100k`. Este modelo obtiene los mejores resultados en  tareas de recuperación  (**NDCG@10 de 0.94**)

| Métrica | Base (Zero-shot) | Fine-tuned (Legal) | Δ (Dif) |
| :--- | :---: | :---: | :---: |
| **Accuracy@1** | 0.8228 | **0.9060** | +0.0832 |
| **Accuracy@5** | 0.9140 | **0.9663** | +0.0523 |
| **NDCG@10** | 0.8801 | **0.9438** | +0.0637 |
| **MRR@10** | 0.8627 | **0.9325** | +0.0698 |
| **MAP@100** | 0.8645 | **0.9333** | +0.0688 |


## 5. Cita
Si utiliza TripLegal-CL o reproduce estos experimentos, por favor cite nuestro trabajo:
@article{TripLegalCL202X,
  title={TripLegal-CL: A Contrastive Learning Corpus for Legal Information Retrieval in Spanish},
  author={Tu Apellido, Nombre and Coautores},
  journal={Procesamiento del Lenguaje Natural (SEPLN)},
  year={202X}
}
