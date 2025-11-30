## 📊 Resultados Experimentales

Evaluamos la eficacia del corpus de aprendizaje contrastivo comparando el rendimiento "Zero-shot" (línea base) frente a los modelos re-entrenados (*Fine-tuned*) en el dominio legal.

Todas las métricas se reportan sobre el conjunto de prueba `legalspanish-eval` (60k *queries*).

### 🏆 Resumen de Impacto (Performance Gains)

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


### 💡 Análisis de los Resultados

1.  **Mejor Desempeño Global:** Gemma Embeddings (Fine-tuned) obtiene los mejores resultados en  tareas de recuperación  (**NDCG@10 de 0.94**).
2.  **Mayor Capacidad de Aprendizaje:** BGE-M3 presenta la evolución más notable tras el ajuste (+12.05 pp en Accuracy@1). A pesar de un inicio más bajo, su arquitectura demostró ser la más eficiente asimilando la terminología jurídica del corpus.
3.  **Efectividad del Corpus:** El incremento generalizado de rendimiento en todas las métricas y modelos confirma que los datos de entrenamiento aportan la señal semántica necesaria para especializar modelos genéricos en la parte legal en español.

---

### 📉 Desglose Detallado por Modelo

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
Comparativa entre el modelo base y la versión adaptada con 100k pasos (`Gema embedding 100k`).

| Métrica | Base (Zero-shot) | Fine-tuned (Legal) | Δ (Dif) |
| :--- | :---: | :---: | :---: |
| **Accuracy@1** | 0.8228 | **0.9060** | +0.0832 |
| **Accuracy@5** | 0.9140 | **0.9663** | +0.0523 |
| **NDCG@10** | 0.8801 | **0.9438** | +0.0637 |
| **MRR@10** | 0.8627 | **0.9325** | +0.0698 |
| **MAP@100** | 0.8645 | **0.9333** | +0.0688 |
