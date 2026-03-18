# TP4 — Autoencoder para Compresión y Generación de Música

Proyecto desarrollado para **Tecnología Digital 6**, centrado en el uso de **redes neuronales autoencoder** para aprender representaciones latentes de canciones a partir de sus **espectrogramas Mel**.

El objetivo principal es comprimir canciones en un espacio latente de menor dimensión, reconstruirlas a partir de esa representación y analizar si ese espacio aprendido captura estructura musical útil, tanto para **agrupar canciones por género** como para **generar variaciones musicales nuevas**.

---

## Objetivos del proyecto

Este trabajo busca responder cuatro preguntas principales:

1. **¿Se puede comprimir una canción en un vector latente pequeño sin perder demasiada calidad?**
2. **¿Ese espacio latente conserva estructura musical útil para distinguir géneros?**
3. **¿El modelo generaliza bien a canciones nuevas no vistas durante el entrenamiento?**
4. **¿Es posible generar nuevas variantes musicales modificando los vectores latentes?**

---

## Enfoque

Para resolver el problema:

- Se transforman canciones `.wav` en **espectrogramas Mel**.
- Se entrena un **autoencoder convolucional** para comprimir y reconstruir esos espectrogramas.
- Se prueban distintas dimensiones latentes para encontrar el mejor equilibrio entre:
  - compresión
  - calidad de reconstrucción
- Se analiza el espacio latente usando:
  - **PCA**
  - **t-SNE**
  - **clustering**
- Se evalúa el modelo con canciones nuevas fuera del dataset.
- Se explora generación musical modificando los vectores latentes con:
  - **ruido**
  - **interpolación**

---

## Resultados principales

- Se probaron distintas dimensiones latentes: **128, 64, 32, 16 y 8**.
- La mejor relación entre compresión y calidad se obtuvo con **latent_dim = 32**.
- A partir de dimensiones menores, especialmente **16 y 8**, la calidad de reconstrucción cae de forma más clara.
- El análisis exploratorio mostró que con dimensiones latentes mayores hay mejor agrupamiento por género.
- El modelo logró representar razonablemente bien canciones nuevas fuera del conjunto de entrenamiento.
- También fue posible generar variaciones musicales perceptibles modificando el espacio latente.

---

## Arquitectura del modelo

El modelo implementado es un **autoencoder convolucional** compuesto por:

### Encoder
Tres capas convolucionales con activación ReLU que reducen progresivamente la dimensionalidad del espectrograma.

### Decoder
Tres capas de convolución transpuesta que reconstruyen el espectrograma a partir del vector latente.

### Función de pérdida
- **MSELoss** para comparar espectrogramas originales vs reconstruidos.

### Optimizador
- **Adam**

---

## Pipeline del proyecto

### 1. Carga y preprocesamiento
- Lectura de archivos `.wav`
- Conversión a mono cuando corresponde
- Re-muestreo a **22050 Hz**
- Transformación a **Mel Spectrogram**
- Conversión a escala logarítmica
- Normalización a rango `[0, 1]`

### 2. Entrenamiento
- División del dataset en:
  - train
  - validation
  - test
- Entrenamiento del autoencoder con distintas dimensiones latentes

### 3. Evaluación de reconstrucción
- Comparación de pérdidas de entrenamiento y validación
- Comparación visual entre espectrogramas originales y reconstruidos
- Escucha cualitativa de audios reconstruidos

### 4. Análisis exploratorio del espacio latente
- Extracción de vectores latentes
- Reducción dimensional con PCA y t-SNE
- Clustering y métricas de agrupamiento

### 5. Generalización
- Codificación y reconstrucción de canciones nuevas no vistas durante el entrenamiento

### 6. Generación musical
- Modificación de vectores latentes mediante:
  - ruido aleatorio
  - interpolación con vectores aleatorios
- Reconstrucción de nuevas variantes musicales

---

## Estructura esperada del proyecto

```text
.
├── Codigo_TP4_G24.ipynb        # Notebook principal con todo el flujo del proyecto
├── Informe_TP4_TD6.pdf         # Informe con análisis, gráficos y conclusiones
├── data/                       # Dataset de audios por género (no incluido en el repo)
│   ├── rock/
│   ├── reggae/
│   ├── pop/
│   ├── jazz/
│   ├── disco/
│   ├── country/
│   ├── hiphop/
│   ├── metal/
│   ├── classical/
│   └── blues/
└── README.md
