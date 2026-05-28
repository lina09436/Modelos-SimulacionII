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

## Ejecución

Los notebooks están diseñados para Google Colab con datos en Google Drive:

```python
DRIVE_PATH = '/content/drive/MyDrive/ProyectoHIGGS/'
```

`01_EDA.ipynb` descarga el dataset de UCI (~2.6 GB) y genera los artefactos que los demás notebooks cargan. Ejecutar primero y en orden.

### Dependencias

```
pandas==2.2.2, numpy==1.26.4, scipy==1.13.0, scikit-learn==1.4.2
umap-learn==0.5.6, matplotlib==3.8.4, seaborn==0.13.2, gdown==5.2.0
xgboost, lightgbm, catboost, wandb
```

## Referencias

1. Baldi, P., Sadowski, P., Whiteson, D. (2014). Searching for exotic particles in high-energy physics with deep learning. *Nature Communications*, 5, 4308.
2. Azhari, M. et al. (2020). Higgs Boson Discovery using Machine Learning Methods with PySpark. *Procedia Computer Science*, 170, 1141–1146.
3. Chen, T., Guestrin, C. (2016). XGBoost: A Scalable Tree Boosting System. *Proceedings of KDD*, 785–794.
4. Ke, G. et al. (2017). LightGBM: A Highly Efficient Gradient Boosting Decision Tree. *NeurIPS*, 30, 3146–3154.
