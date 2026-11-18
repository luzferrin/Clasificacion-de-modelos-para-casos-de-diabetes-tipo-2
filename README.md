# Clasificacion-de-modelos-para-casos-de-diabetes-tipo-2
Comparación de seis modelos supervisados (árbol de decisión, bosque aleatorio, MLP, ResNet, SVM y CatBoost) para clasificar diabetes tipo 2 con datos tabulares de Bangladesh. Se evalúan con y sin una regla experimental basada en glucosa en ayunas.


# Autores
1. Luz N. Ferrin R - https://github.com/luzferrin
2. Antonio F. L. Jacob Jr. - https://github.com/jacobjr

## Conjunto de datos
Este proyecto utiliza el conjunto de datos de Bangladesh de los autores [Buiyan et al. 2026].
El dataset está disponible en: https://www.kaggle.com/datasets/kumargh/pimaindiansdiabetescsv 
**Los modelos**: árbol de decisión, bosque aleatorio y MLP, fueron adaptados en este ejercicio de acuerdo con el baseline seleccionado [Sirocchi et al. 2024].
**Artículo**: https://link.springer.com/article/10.1186/s12911-024-02582-4
**Código**: https://github.com/ChristelSirocchi/medical-informed-ML

El dataset de esta referencia TabZilla de los autores [McElfresh et al., 2023] Se utilizó este trabajo para seleccionar 3 modelos adicionales de acuerdo con sus resultados en datasets tabulares.
**Los modelos** escogidos fueron: ResNet, SVM y CatBoost.
**Artículo**: https://arxiv.org/pdf/2305.02997
**Código**:https://github.com/naszilla/tabzilla
Los datasets utilizados en el trabajo de TabZilla se encuentra está disponible en:
1. https://github.com/naszilla/tabzilla/blob/main/scripts/DATASETS_A.sh
2. https://github.com/naszilla/tabzilla/blob/main/scripts/DATASETS_B.sh

## Evaluación de 6 Modelos de Aprendizaje Automático para la Detección de Diabetes Tipo 2

Este repositorio contiene el pipeline de experimentación, entrenamiento, evaluación clínica y análisis multi-criterio para clasificar pacientes con **Diabetes Tipo 2** utilizando variables clínicas y demográficas de entrada.

## Datos y Preprocesamiento
El conjunto de datos consta de **496 filas** distribuidas de la siguiente manera:
- **Clase 0 (Sanos):** 119 casos
- **Clase 1 (Diabéticos):** 377 casos

### Predictores (Features)
1. `No. of Pregnancy` (Número de embarazos)
2. `Age` (Edad)
3. `BMI` (Índice de Masa Corporal)
4. `BP(Systolic)` (Presión arterial sistólica)
5. `BP(Diastolic)` (Presión arterial diastólica)
6. `DiabetesPedigreeFunction` (Función de pedigrí de la diabetes)
7. `Insulin` (Nivel de insulina - imputado con la mediana del fold de entrenamiento)
8. `Skin Thickness(mm)` (Espesor del pliegue cutáneo)
9. `Glucose` (Glucosa plasmática en ayunas)

---

## Metodología de Validación
Se implementó un esquema robusto de **Validación Cruzada Estratificada de 10 pliegues (10-Fold CV)** para evitar la fuga de información (*data leakage*). 
Para cada pliegue:
1. Se divide en **Entrenamiento (80%)**, **Validación (10%)** y **Prueba (10%)**.
2. La imputación de valores faltantes (usando la mediana) y el escalado de características (StandardScaler) se calculan **únicamente** sobre el conjunto de entrenamiento de cada pliegue y se aplican de forma idéntica a validación y prueba.

---

## Modelos Evaluados
Se entrenaron y compararon 6 clasificadores bajo dos variantes:

1. **Árbol de Decisión**
2. **Bosque Aleatorio (Random Forest)**
3. **MLP (Perceptrón Multicapa)**
4. **SVM (Máquinas de Soporte Vectorial)**
5. **ResNet Tabular** (Red neuronal profunda para datos tabulares)
6. **CatBoost**

### Variantes de Evaluación
- **Variante Pura (Modelo):** Las predicciones se basan exclusivamente en las salidas del clasificador entrenado.
- **Variante Híbrida (Modelo + Reglas):** Se aplica una regla clínica basada en la glucosa plasmática:
  - Si `Glucosa < 100` -> Se fuerza predicción a `0` (Sano).
  - Si `Glucosa >= 126` -> Se fuerza predicción a `1` (Diabético).
  - Si la glucosa se encuentra en el rango gris (`100 a 125`) -> Se conserva la predicción original del modelo.

---

## Resultados y Entregables

Todos los resultados métricos, parámetros y gráficos generados se estructuran dentro de la carpeta `resultados_6_modelos_10fold/`:

- `metricas_por_pliegue.csv`: Métricas detalladas para cada pliegue, modelo y variante.
- `resumen_metricas_media_sd.csv`: Promedios y desviaciones estándar globales.
- `predicciones_oof_matrices.csv`: Predicciones fuera de muestra (*Out-of-Fold*) acumuladas para el cálculo clínico global.
- `ranking_mcda_4_metricas.csv`: Ranking óptimo obtenido mediante análisis de decisión multi-criterio (MCDA) aplicando pesos iguales (25%) a Exactitud, Exactitud Balanceada, F1 Macro y Sensibilidad.
- `matrices_confusion_conteos.csv`: Conteo consolidado de aciertos y errores por clase.

### Gráficos Incluidos
- `matriz_unica_integradora_modelos.png`: Visualización consolidada del desempeño clínico con porcentajes relativos.
- `grafico_modelo.png` y `grafico_modelo_mas_reglas.png`: Gráficos de barras y perfiles de línea de rango de las métricas principales.
- `aciertos_desaciertos_modelo[_reglas].png`: Análisis comparativo de aciertos (TP, TN) y desaciertos (FP, FN) por modelo y variante.
