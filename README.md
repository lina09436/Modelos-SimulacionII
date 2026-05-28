# Modelos y Simulación II — Proyecto Final

Clasificación binaria de eventos del bosón de Higgs usando el dataset HIGGS de UCI.

**Universidad de Antioquia** — Facultad de Ingeniería  
**Autores:** Lina María Montoya Zuluaga, Diego Armando Muñoz Carvajal  
**Profesor:** Julián D. Arias-Londoño

---

## Contenido del repositorio

| Archivo | Descripción |
|---------|-------------|
| `ProyectoModelos2.pdf` | Informe final del proyecto (IEEE Transactions, ≤10 páginas) |
| `Video.mov` | Video de sustentación (10 minutos) |
| `Modelos/` | Notebooks de Jupyter con el pipeline completo |

### Notebooks

Ejecutar en este orden:

```
01_EDA → 03_modelos_* → 03_modelos_final → 04_reduccion_dimension → 05_conclusiones
02_estado_del_arte (independiente)
```

| Notebook | Contenido |
|----------|-----------|
| `01_EDA.ipynb` | Descarga del dataset, análisis exploratorio, split 70/15/15 |
| `02_estado_del_arte.ipynb` | Revisión de literatura (4 papers) |
| `03_modelos_LR.ipynb` | Regresión Logística — baseline paramétrico |
| `03_modelos_KNN.ipynb` | K-Nearest Neighbors — no paramétrico |
| `03_modelos_RF.ipynb` | Random Forest — ensamble de árboles |
| `03_modelos_MLP.ipynb` | Red Neuronal (MLP) |
| `03_modelos_SVM.ipynb` | SVM lineal + Platt scaling |
| `03_modelos_HistGB.ipynb` | HistGradientBoosting (sklearn) |
| `03_modelos_XGBoost.ipynb` | XGBoost |
| `03_modelos_LightGBM.ipynb` | LightGBM |
| `03_modelos_CatBoost.ipynb` | CatBoost |
| `03_modelos_Stacking.ipynb` | Stacking (5 base + LR meta) |
| `03_modelos_DNN_Baldi.ipynb` | Intento de replicación Baldi et al. (no usado en informe) |
| `03_modelos_final.ipynb` | Ensamblaje: tabla comparativa y top-2 |
| `04_reduccion_dimension.ipynb` | Análisis d', selección de features, PCA, UMAP |
| `05_conclusiones.ipynb` | Tabla maestra, comparación literatura, conclusiones |

## Resultados principales

- **Dataset:** 500,000 eventos estratificados de UCI HIGGS (28 features, 11M total)
- **Split:** 70% train / 15% validación / 15% test, estratificado
- **Modelos evaluados:** 10 familias, 15 configuraciones, 126 combinaciones de hiperparámetros
- **Mejor modelo:** Red Neuronal MLP (128, 64) y XGBoost — ambos AUC = 0.826 en test
- **Reducción de dimensionalidad:** Selección por d' (28→21, ΔAUC = −0.002) >> PCA (23 comp, ΔAUC = −0.04) >> UMAP (16 comp, ΔAUC = −0.17)

## Guía de ejecución

### Requisitos previos

1. Cuenta de Google con acceso a **Google Colab**
2. Espacio en Google Drive (≥5 GB libres para el dataset y artefactos)
3. Cuenta de **wandb** (weights & biases) para registrar experimentos
4. Cuenta de **GitHub** con acceso a este repositorio

### Paso 1: Configurar Google Drive

Crear la carpeta de trabajo en Google Drive:

```
MiDrive/ProyectoHIGGS/
```

Todos los notebooks leen y escriben en esta ruta:
```python
DRIVE_PATH = '/content/drive/MyDrive/ProyectoHIGGS/'
```

### Paso 2: Abrir notebooks en Colab

En Google Colab: `Archivo` → `Abrir notebook` → `GitHub` → pegar URL del repositorio.

Alternativa: clonar este repositorio en Google Colab:
```python
!git clone https://github.com/lina09436/Modelos-SimulacionII.git
```

### Paso 3: Ejecutar en orden

Los notebooks **deben ejecutarse secuencialmente** porque cada uno consume artefactos generados por el anterior:

| Orden | Notebook | Tiempo estimado | Genera |
|-------|----------|-----------------|--------|
| **1** | `01_EDA.ipynb` | 10-20 min | Descarga HIGGS (~2.6 GB), genera `X_train.npy`, `X_val.npy`, `X_test.npy`, `y_*.npy`, `feature_names.npy` |
| **2** | `02_estado_del_arte.ipynb` | — | Independiente. Solo markdown, no requiere datos |
| **3** | `03_modelos_*.ipynb` (10 notebooks) | 2-25 min c/u | Cada uno guarda `resultado_*.csv` y `modelo_*.pkl` en Drive |
| **4** | `03_modelos_final.ipynb` | 2 min | `tabla_comparativa_modelos.csv`, `top2_modelos.npy` |
| **5** | `04_reduccion_dimension.ipynb` | 20-30 min | `tabla_comparativa_reduccion.csv`, tablas PCA/UMAP |
| **6** | `05_conclusiones.ipynb` | 5 min | `tabla_maestra.csv`, `tabla_comparacion_literatura.csv` |

> **Nota:** Los 10 notebooks `03_modelos_*` son independientes entre sí. Se pueden ejecutar en paralelo abriendo varias pestañas de Colab para acelerar el proceso.

### Artefactos generados (Drive)

| Archivo | Generado por | Consumido por |
|---------|-------------|---------------|
| `X_train.npy`, `X_val.npy`, `X_test.npy` | `01_EDA` | `03_*`, `04_*` |
| `y_train.npy`, `y_val.npy`, `y_test.npy` | `01_EDA` | `03_*`, `04_*` |
| `feature_names.npy` | `01_EDA` | `03_*`, `04_*` |
| `resultado_*.csv` | Cada `03_modelos_*` | `03_modelos_final` |
| `modelo_*.pkl` | Cada `03_modelos_*` | `04_reduccion_dimension` |
| `tabla_comparativa_modelos.csv` | `03_modelos_final` | `04_*`, `05_*` |
| `top2_modelos.npy` | `03_modelos_final` | `04_reduccion_dimension` |
| `tabla_comparativa_reduccion.csv` | `04_reduccion_dimension` | `05_conclusiones` |

### Configuración de wandb

Cada notebook de `03_modelos_*` registra experimentos en wandb. Para usar tu propia cuenta:

1. Crear cuenta en [wandb.ai](https://wandb.ai)
2. Obtener API key en [wandb.ai/authorize](https://wandb.ai/authorize)
3. En Colab, guardar la key como secreto: `WANDB_API_KEY`
4. O reemplazar `wandb.login(key=userdata.get('WANDB_API_KEY'))` por `wandb.login(key="tu-api-key")`

Si no se desea usar wandb, comentar las líneas `wandb.init()`, `wandb.log()` y `wandb.finish()` en cada notebook.

### Modelos que requieren más RAM/tiempo

| Modelo | Recurso | Solución |
|--------|---------|----------|
| KNN | 100K muestras (submuestreo automático) | Ya optimizado en el notebook |
| XGBoost, LightGBM, CatBoost | ~20-25 min c/u | Ejecutar en paralelo en pestañas separadas |
| UMAP (en `04_reduccion_dimension`) | Ajuste en 200K muestras | Ya optimizado; ~10 min |
| Todos los demás | ≤15 min | RAM gratuita de Colab suficiente |

## Referencias

1. Baldi, P., Sadowski, P., Whiteson, D. (2014). Searching for exotic particles in high-energy physics with deep learning. *Nature Communications*, 5, 4308.
2. Azhari, M. et al. (2020). Higgs Boson Discovery using Machine Learning Methods with PySpark. *Procedia Computer Science*, 170, 1141–1146.
3. Chen, T., Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *Proceedings of KDD*, 785–794.
4. Ke, G. et al. (2017). LightGBM: A Highly Efficient Gradient Boosting Decision Tree. *NeurIPS*, 30, 3146–3154.
