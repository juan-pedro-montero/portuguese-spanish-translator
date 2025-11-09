# Proyecto de Traducción Automática PT→ES

Este repositorio contiene todo el material necesario para extraer, preprocesar, modelar y evaluar un sistema de traducción automática de portugués a español (PT→ES) usando arquitecturas Transformer y fine‑tuning de modelos multilingües.

---

## Contenido del repositorio

```
├── data
│   ├── corpus.pt.txt                      # Corpus PT original
│   ├── corpus.es.txt                      # Corpus ES original
│   ├── clean_corpus_pt_es.json            # Corpus paralelo limpio (JSON)
│   ├── clean_corpus_pt.txt                # Corpus PT limpio
│   ├── clean_corpus_es.txt                # Corpus ES limpio
│   └── tokenized_facebook_m2m100_418M_len128/
│       ├── train.arrow
│       └── test.arrow
│
├── outputs
│   └── m2m100_finetuned_lora_infer        # Modelo y tokenizer guardados para inferencia
│
├── 1_EDA.ipynb         
├── 2_Tokenización_Split.ipynb
├── 3_Transformer_Scratch.ipynb
├── 4_Transformer_Finetunning.ipynb
├── 5_Modelo_Inferencia.ipynb
└── readme.md     
```

NOTA IMPORTANTE:
Debido a su gran tamaño (el dataset de 2.5 millones de frases y los modelos), los archivos de datos y los checkpoints no están incluidos en este repositorio de Git.
Sin embargo, puedes descargarlos desde los siguientes enlaces: https://drive.google.com/file/d/1CR6WpZJMI0NcCl8hB2nBot6Xfuqe1oH3/view?usp=sharing

---

## Requisitos previos

- Python 3.8 o superior
- GPU con soporte CUDA (recomendado) o CPU
- \~20 GB de espacio en disco para datos y checkpoints

---

## Instalación

1. Clonar el repositorio:

   ```bash
   git clone <URL_DEL_REPOSITORIO>
   cd <NOMBRE_DEL_PROYECTO>
   ```

2. Crear un entorno virtual e instalar dependencias:

   ```bash
   python -m venv venv
   source venv/bin/activate    # Linux / MacOS
   venv\Scripts\activate     # Windows
   pip install --upgrade pip
   pip install -r requirements.txt
   ```

---

## Flujo de trabajo

A continuación se detallan los pasos para reproducir el pipeline completo de extracción, preprocesamiento, entrenamiento e inferencia:

0. Cargar Data
   - Crear el directorio "data"
   - Descomprimir el archivo "corpus_pt_es" en carpeta "data"

2. **EDA y limpieza**\
   Abrir y ejecutar `notebooks/1_EDA.ipynb`. Este notebook realiza:

   - Muestreo y análisis de distribuciones de longitud.
   - Filtrado de vacíos, duplicados y pares extremos.
   - Exportación de `clean_corpus.pt` y `clean_corpus.es`.

3. **Tokenización y partición**\
   Ejecutar `notebooks/2_Tokenize_Split.ipynb`, que:

   - Carga el corpus limpio.
   - Aplica `M2M100Tokenizer` para tokenizar y truncar/padear.
   - Separa los datos en train (80%) y test (20%).
   - Guarda las particiones en formato Arrow.

4. **Fine‑tuning con M2M100 + LoRA**\
   Abrir `notebooks/4_Transformer_FT.ipynb` y ejecutar:

   - Carga de datos tokenizados.
   - Submuestreo de ejemplos (40 k train / 10 k eval).
   - Configuración de LoRA, `Seq2SeqTrainer` y entrenamiento por 2 épocas.
   - Cálculo de BLEU y perplexity en un subset de 1,000 ejemplos.

5. **Inferencia**\
   Ejecutar `notebooks/5_Inference.ipynb` para:

   - Cargar el mejor checkpoint y el tokenizer.
   - Traducir un conjunto de frases de ejemplo.
   - Decodificar y visualizar resultados.

---

## Resultados principales

- **Corpus limpio:** 2.48 M pares PT–ES
- **BLEU (subset 1,000):** 34.56
- **Perplexity (subset 1,000):** 3.46

---


