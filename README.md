# Instrucciones para Generación de Trabajo Académico en Quarto

## 1. ESTRUCTURA ACADÉMICA OBLIGATORIA

### Secciones Principales (Orden Estricto)

1. **Introducción** (800-1200 palabras)

   - Contexto del problema y relevancia
   - Revisión de literatura especializada con citas
   - Planteamiento específico del problema
   - Objetivos general y específicos numerados
2. **Metodología** (1000-1500 palabras)

   - Descripción detallada de datos (origen, tamaño, variables)
   - Preprocesamiento paso a paso con código
   - Selección de características con múltiples métodos
   - Especificación de modelos y hiperparámetros
   - Métricas de evaluación justificadas
3. **Resultados** (1200-1800 palabras)

   - Presentación objetiva de hallazgos
   - Métricas de rendimiento comparativas
   - Visualizaciones con análisis descriptivo
   - Sin interpretación prematura
4. **Discusión** (800-1200 palabras)

   - Interpretación de resultados en contexto
   - Comparación con literatura existente
   - Limitaciones del estudio identificadas
   - Implicaciones prácticas y teóricas
5. **Conclusiones** (400-600 palabras)

   - Aportes principales resumidos
   - Respuesta a objetivos planteados
   - Direcciones para trabajo futuro
   - Recomendaciones específicas
6. **Bibliografía**

   - Mínimo 15-20 referencias académicas
   - Preferencia por últimos 5 años
   - Formato IEEE o APA según especificación

### Jerarquía de Encabezados

```markdown
# Sección Principal {#sec-etiqueta}
## Subsección Principal
### Subsección Específica
#### Detalles Técnicos
```

## 2. MANEJO ESPECÍFICO DE DATOS Y TABLAS

### 🎯 REGLA FUNDAMENTAL PARA TABLAS:

- **Contenido estático** → Markdown directo
- **Variables Python** → Celda Python con `display()`

### Tablas Estáticas (Markdown Directo)

```markdown
| **Métrica** | **Valor** | **Interpretación** |
|-------------|-----------|-------------------|
| AUC-ROC | 0.8823 | Excelente discriminación |
| F1-Score | 0.4286 | Balanceado |

: Título descriptivo {#tbl-etiqueta tbl-colwidths="[30,20,50]"}
```

### Tablas Dinámicas (Celda Python)

```{python}
#| label: tbl-resultados
#| tbl-cap: "Métricas de evaluación del modelo"
#| tbl-colwidths: [25,20,20,15,20]

# Para DataFrames con styling
display(df_metricas.style.format({
    'AUC-ROC': '{:.4f}',
    'Precisión': '{:.2%}'
}).set_caption("Resultados finales"))

# Para tablas con tabulate
from tabulate import tabulate
display(Markdown(tabulate(df_resumen, headers='keys', showindex=False)))
```

### Manejo de Encabezados Largos

```{python}
import textwrap

def wrap_text(text, width=12):
    if isinstance(text, str) and len(text) > width:
        return '\n'.join(textwrap.wrap(text, width=width))
    return text

# Aplicar a columnas problemáticas
df_display['Variable'] = df_display['Variable'].apply(lambda x: wrap_text(str(x), 12))
```

### Formateo Consistente de Números

```{python}
# Configuración estándar
formato_metricas = {
    'AUC-ROC': '{:.4f}',
    'F1-Score': '{:.4f}', 
    'Precisión': '{:.4f}',
    'Recall': '{:.4f}',
    'Porcentaje': '{:.2f}%',
    'Tiempo': '{:.2f}s'
}

# Aplicar formato
df.style.format(formato_metricas)
```

## 3. VISUALIZACIONES Y FIGURAS

### Configuración Estándar

```{python}
#| label: fig-nombre-descriptivo
#| fig-cap: "Descripción completa y específica de lo que muestra la figura"
#| fig-width: 12
#| fig-height: 8

import matplotlib.pyplot as plt
import seaborn as sns

# Configuración de estilo
plt.figure(figsize=(12, 8))
sns.set_style("whitegrid")

# Código de plotting aquí
plt.title('Título descriptivo', fontsize=14, pad=20)
plt.xlabel('Etiqueta del eje X')
plt.ylabel('Etiqueta del eje Y')
plt.legend()
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

### Requisitos Obligatorios

- **Títulos descriptivos** que expliquen el contenido
- **Ejes etiquetados** con unidades cuando corresponda
- **Leyendas claras** para múltiples series
- **Colores accesibles** (evitar solo rojo-verde)
- **Tamaño legible** de texto y elementos
- **Grillas sutiles** para facilitar lectura

### Tipos de Gráficos Recomendados

```{python}
# Comparaciones de modelos
fig, axes = plt.subplots(2, 2, figsize=(15, 12))

# ROC Curves
axes[0,0].plot(fpr, tpr, label=f'Modelo (AUC={auc:.3f})')
axes[0,0].set_title('Curvas ROC')

# Confusion Matrix
sns.heatmap(cm, annot=True, fmt='d', ax=axes[0,1])
axes[0,1].set_title('Matriz de Confusión')

# Feature Importance
axes[1,0].barh(features, importance)
axes[1,0].set_title('Importancia de Variables')

# Distribution plots
axes[1,1].hist(predictions, bins=50, alpha=0.7)
axes[1,1].set_title('Distribución de Predicciones')
```

## 4. PRESENTACIÓN DE INFORMACIÓN DINÁMICA

### Uso Correcto de display(Markdown())

**SOLO usar para contenido generado dinámicamente:**

```{python}
from IPython.display import display, Markdown

# ✅ Correcto: Información calculada
display(Markdown(f"""
#### Resumen del Dataset
- **Observaciones:** {len(df):,}
- **Variables:** {df.shape[1]}
- **Tasa de default:** {df['target'].mean():.2%}
- **Valores faltantes:** {df.isnull().sum().sum():,} ({df.isnull().sum().sum()/df.size:.2%})
"""))

# ✅ Correcto: Resultados de modelos
display(Markdown(f"""
### Resultados Finales
- **AUC-ROC:** {auc_score:.4f}
- **Tiempo de entrenamiento:** {training_time:.2f} segundos
- **Mejor iteración:** {best_iteration}
"""))
```

### Texto Estático (Markdown Normal)

## Metodología {#sec-metodologia}

En esta sección se describe el enfoque metodológico utilizado...

### Selección de Variables

Los criterios aplicados incluyen:
- Relevancia estadística medida por test F
- Información mutua superior a 0.1
- Importancia en Random Forest > 0.01


### ❌ Errores Comunes a Evitar

```{python}
# ❌ NO hacer: print() para información formateada
print(f"AUC: {auc}")  # Se ve mal en Quarto

# ❌ NO hacer: display(Markdown()) para texto estático
display(Markdown("## Metodología"))  # Innecesario

# ❌ NO hacer: Variables Python en markdown directo
# Esto NO funcionará:
```

## Resultados: AUC =  # Error

## 5. CÓDIGO REPRODUCIBLE Y DOCUMENTADO

### Estructura de Celdas
```{python}
#| label: code-descriptive-name
#| echo: false  # o true según se quiera mostrar código
#| warning: false
#| message: false

# Importaciones al inicio
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split

# Configuración de reproducibilidad
np.random.seed(42)

# Código bien comentado
# Cargar y preparar datos
df = pd.read_csv('data/dataset.csv')

# Dividir datos manteniendo distribución
X_train, X_test, y_train, y_test = train_test_split(
    X, y, 
    test_size=0.2, 
    random_state=42, 
    stratify=y
)

# Mostrar dimensiones
display(Markdown(f"""
**Dimensiones finales:**
- Entrenamiento: {X_train.shape}
- Prueba: {X_test.shape}
"""))
```

### Documentación de Decisiones

```{python}
# Documentar TODAS las decisiones importantes
display(Markdown("""
#### Justificación de Parámetros:
- **test_size=0.2:** Estándar industria (80/20 split)
- **random_state=42:** Reproducibilidad
- **stratify=y:** Mantener distribución de clases
"""))
```

### Manejo de Versiones y Dependencias

```{python}
# Al inicio del documento
import sys
import sklearn
import xgboost
import torch

display(Markdown(f"""
#### Entorno de Desarrollo:
- **Python:** {sys.version.split()[0]}
- **scikit-learn:** {sklearn.__version__}
- **XGBoost:** {xgboost.__version__}
- **PyTorch:** {torch.__version__}
- **CUDA disponible:** {torch.cuda.is_available()}
"""))
```

## 6. METODOLOGÍA ESPECÍFICA PARA ML

### Preprocesamiento Documentado

```{python}
# Análisis de valores faltantes
missing_info = df.isnull().sum()
display(Markdown(f"""
#### Calidad de Datos:
- **Total observaciones:** {len(df):,}
- **Variables:** {len(df.columns)}
- **Valores faltantes:** {missing_info.sum()} ({missing_info.sum()/df.size:.2%})
"""))

# Tratamiento de outliers con justificación
def detectar_outliers_iqr(data, factor=1.5):
    Q1, Q3 = data.quantile([0.25, 0.75])
    IQR = Q3 - Q1
    return (data < Q1 - factor*IQR) | (data > Q3 + factor*IQR)

outliers = detectar_outliers_iqr(df['variable'])
display(Markdown(f"Outliers detectados: {outliers.sum()} ({outliers.mean():.2%})"))
```

### Selección de Características Múltiple

```{python}
from sklearn.feature_selection import SelectKBest, f_classif, RFE
from sklearn.ensemble import RandomForestClassifier

# Método 1: Estadístico
selector_f = SelectKBest(f_classif, k=15)
X_f = selector_f.fit_transform(X_train, y_train)
features_f = X_train.columns[selector_f.get_support()]

# Método 2: Wrapper (RFE)
rf = RandomForestClassifier(n_estimators=100, random_state=42)
rfe = RFE(rf, n_features_to_select=15)
X_rfe = rfe.fit_transform(X_train, y_train)
features_rfe = X_train.columns[rfe.support_]

# Método 3: Embedded (Feature Importance)
rf.fit(X_train, y_train)
importance_df = pd.DataFrame({
    'feature': X_train.columns,
    'importance': rf.feature_importances_
}).sort_values('importance', ascending=False)

features_rf = importance_df.head(15)['feature'].tolist()

# Consenso entre métodos
features_final = list(set(features_f) & set(features_rfe) & set(features_rf))

display(Markdown(f"""
#### Selección de Características:
- **Método F-test:** {len(features_f)} variables
- **Método RFE:** {len(features_rfe)} variables  
- **Método RF Importance:** {len(features_rf)} variables
- **Consenso:** {len(features_final)} variables
"""))
```

### Entrenamiento y Evaluación

```{python}
from sklearn.metrics import roc_auc_score, classification_report

# Configuración del modelo con documentación
modelo_params = {
    'n_estimators': 500,
    'max_depth': 6,
    'learning_rate': 0.1,
    'subsample': 0.8,
    'random_state': 42
}

# Entrenamiento
start_time = time.time()
modelo = XGBClassifier(**modelo_params)
modelo.fit(X_train, y_train)
training_time = time.time() - start_time

# Evaluación completa
y_pred_proba = modelo.predict_proba(X_test)[:, 1]
y_pred = modelo.predict(X_test)

metricas = {
    'AUC-ROC': roc_auc_score(y_test, y_pred_proba),
    'Accuracy': accuracy_score(y_test, y_pred),
    'F1-Score': f1_score(y_test, y_pred),
    'Precisión': precision_score(y_test, y_pred),
    'Recall': recall_score(y_test, y_pred)
}

# Presentar resultados
metricas_df = pd.DataFrame(list(metricas.items()), columns=['Métrica', 'Valor'])
display(metricas_df.style.format({'Valor': '{:.4f}'}).hide(axis='index'))

display(Markdown(f"**Tiempo de entrenamiento:** {training_time:.2f} segundos"))
```

## 7. ECUACIONES Y FORMALIZACIÓN MATEMÁTICA

### Ecuaciones en Bloque

La métrica AUC-ROC se define como:

$$ 
AUC = \int_0^1 TPR(FPR^{-1}(x)) dx = \int_0^1 \text{Recall}(\text{FPR}^{-1}(x)) dx
$$ {#eq-auc}

donde $TPR$ es la tasa de verdaderos positivos y $FPR$ la tasa de falsos positivos.

### Ecuaciones en Línea

La precisión se calcula como $P = \frac{TP}{TP + FP}$, donde $TP$ son los verdaderos positivos y $FP$ los falsos positivos.


### Notación Matemática Estándar

#### Definición de Variables:
- $X \in \mathbb{R}^{n \times p}$: Matriz de características ($n$ observaciones, $p$ variables)
- $y \in \{0,1\}^n$: Vector de variable objetivo binaria  
- $\hat{y} \in [0,1]^n$: Vector de predicciones de probabilidad
- $\theta \in \mathbb{R}^p$: Vector de parámetros del modelo

### Referencias a Ecuaciones

Como se muestra en @eq-auc, el área bajo la curva ROC proporciona una medida...

## 8. CITACIÓN Y REFERENCIAS

### Formato de Citas

# En el texto
Según [@chen2016xgboost], los métodos de boosting...
Los estudios previos [@autor2023; @otro2022] demuestran que...
Como indica [@lessmann2015benchmarking, p. 15], la validación cruzada...

# Referencias múltiples
Varios autores [@baesens2003; @thomas2017credit; @hand2007consumer] coinciden en...

### Referencias Cruzadas

# A figuras
Como se observa en @fig-roc-curves, el modelo XGBoost...

# A tablas  
Los resultados en @tbl-metricas-finales muestran...

# A secciones
Según se describió en @sec-metodologia, el preprocesamiento...

# A ecuaciones
La ecuación @eq-auc define formalmente...


### Bibliografía (Archivo references.bib)

```bibtex
@article{chen2016xgboost,
  title={XGBoost: A scalable tree boosting system},
  author={Chen, Tianqi and Guestrin, Carlos},
  journal={Proceedings of the 22nd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining},
  pages={785--794},
  year={2016}
}

@article{lessmann2015benchmarking,
  title={Benchmarking credit scoring models},
  author={Lessmann, Stefan and Baesens, Bart and Seow, Hsin-Vonn and Thomas, Lyn C},
  journal={European Journal of Operational Research},
  volume={247},
  number={1},
  pages={124--136},
  year={2015}
}
```

## 9. EVALUACIÓN DE MODELOS ESPECIALIZADA

### Métricas para Clasificación Binaria

```{python}
from sklearn.metrics import (
    roc_auc_score, precision_recall_curve, average_precision_score,
    confusion_matrix, classification_report
)

def evaluar_modelo_completo(y_true, y_pred, y_pred_proba, nombre_modelo):
    """Evaluación completa para modelos de riesgo crediticio"""
  
    # Métricas básicas
    metricas = {
        'AUC-ROC': roc_auc_score(y_true, y_pred_proba),
        'AUC-PR': average_precision_score(y_true, y_pred_proba),
        'F1-Score': f1_score(y_true, y_pred),
        'Precisión': precision_score(y_true, y_pred),
        'Recall': recall_score(y_true, y_pred),
        'Accuracy': accuracy_score(y_true, y_pred)
    }
  
    # Métricas específicas para riesgo crediticio
    tn, fp, fn, tp = confusion_matrix(y_true, y_pred).ravel()
  
    metricas_credito = {
        'Especificidad': tn / (tn + fp),  # Identificar correctamente no-defaults
        'NPV': tn / (tn + fn),  # Valor predictivo negativo
        'Lift': (tp / (tp + fp)) / (y_true.sum() / len(y_true)),  # Mejora vs azar
        'Tasa_Error_Tipo_I': fp / (fp + tn),  # Falsos positivos
        'Tasa_Error_Tipo_II': fn / (fn + tp)  # Falsos negativos
    }
  
    metricas.update(metricas_credito)
  
    return pd.DataFrame(list(metricas.items()), 
                       columns=['Métrica', nombre_modelo])
```

### Validación Cruzada Estratificada

```{python}
from sklearn.model_selection import StratifiedKFold, cross_val_score

# Configuración robusta para validación
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Múltiples métricas
scoring = ['roc_auc', 'f1', 'precision', 'recall']

resultados_cv = {}
for score in scoring:
    scores = cross_val_score(modelo, X_train, y_train, cv=skf, scoring=score)
    resultados_cv[score] = {
        'media': scores.mean(),
        'std': scores.std(),
        'cv': scores.std() / scores.mean(),  # Coeficiente de variación
        'min': scores.min(),
        'max': scores.max()
    }

# Crear DataFrame de resultados
cv_df = pd.DataFrame(resultados_cv).T
display(cv_df.style.format({
    'media': '{:.4f}',
    'std': '{:.4f}', 
    'cv': '{:.2%}',
    'min': '{:.4f}',
    'max': '{:.4f}'
}).set_caption("Resultados de Validación Cruzada 5-Fold"))
```

### Análisis de Curvas de Aprendizaje

```{python}
from sklearn.model_selection import learning_curve

# Generar curvas de aprendizaje
train_sizes, train_scores, val_scores = learning_curve(
    modelo, X_train, y_train,
    train_sizes=np.linspace(0.1, 1.0, 10),
    cv=5, scoring='roc_auc', n_jobs=-1
)

# Visualizar
plt.figure(figsize=(10, 6))
plt.plot(train_sizes, train_scores.mean(axis=1), 'o-', label='Entrenamiento')
plt.plot(train_sizes, val_scores.mean(axis=1), 'o-', label='Validación')
plt.fill_between(train_sizes, 
                 train_scores.mean(axis=1) - train_scores.std(axis=1),
                 train_scores.mean(axis=1) + train_scores.std(axis=1),
                 alpha=0.1)
plt.xlabel('Tamaño del Conjunto de Entrenamiento')
plt.ylabel('AUC-ROC')
plt.title('Curvas de Aprendizaje')
plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

## 10. INTERPRETABILIDAD Y EXPLICABILIDAD

### Importancia de Características

```{python}
# Para modelos basados en árboles
feature_importance = pd.DataFrame({
    'feature': features_finales,
    'importance': modelo.feature_importances_
}).sort_values('importance', ascending=False)

# Visualización
plt.figure(figsize=(10, 8))
plt.barh(range(len(feature_importance.head(15))), 
         feature_importance.head(15)['importance'],
         color='skyblue')
plt.yticks(range(15), feature_importance.head(15)['feature'])
plt.xlabel('Importancia')
plt.title('Top 15 Variables más Importantes')
plt.gca().invert_yaxis()
plt.tight_layout()
plt.show()

# Tabla complementaria
display(feature_importance.head(10).style.format({'importance': '{:.4f}'}))
```

### Análisis SHAP (Opcional)

```{python}
# Si se usa SHAP
import shap

# Crear explainer
explainer = shap.Explainer(modelo)
shap_values = explainer(X_test.head(100))  # Muestra pequeña

# Gráfico de importancia
shap.plots.beeswarm(shap_values, max_display=15)
```

## 11. GESTIÓN DE ARCHIVOS Y ESTRUCTURA

### Estructura de Directorios Recomendada

```
proyecto/
├── data/
│   ├── raw/
│   ├── processed/
│   └── diccionario.csv
├── models/
│   ├── xgboost_model.pkl
│   └── preprocessing_pipeline.pkl
├── results/
│   ├── figures/
│   └── tables/
├── template/
│   ├── ieee.csl # Estilo incluido previamente no es necesario editar
│   ├── packages.tex # Paquetes LaTeX ya incluidos no es necesario editar
│   ├── header-footer.tex # Encabezado y pie de página ya establecidos no es necesario editar
│   └── before-body.tex # Parte antes del cuerpo del documento ya establecidos no es necesario editar
├── documento_CPP.qmd # Documento Quarto principal Caso Practico Propuesto (CPP_NumeroIDL_Apellido_Estudiante.qmd)
└── references.bib # Generar BibTeX file for references
```

### Carga y Guardado de Datos

```{python}
# Carga con verificación
try:
    df = pd.read_csv('data/dataset.csv')
    display(Markdown(f"✅ Datos cargados: {df.shape}"))
except FileNotFoundError:
    display(Markdown("❌ Error: Archivo no encontrado"))

# Guardado de modelos
import joblib
joblib.dump(modelo, 'models/modelo_final.pkl')
display(Markdown("✅ Modelo guardado exitosamente"))
```

## 12. CONFIGURACIÓN DE QUARTO

### YAML Header Completo en documento CPP.qmd
```yaml
---
idl_numero: "" # Indicador de Logro, esta variable debe ser completada con el número de IDL del estudiante
curso: ""
estudiantes:
    - "APAZA PEREZ OSCAR GONZALO"
    - "PONCE DE LEON TORRES FABYOLA KORAYMA"    
    - "RUIZ ALVA JERSON ENMANUEL"
profesores:
    - "" # Estas variables las toma el before-body.tex no es necesario editarlo si no se te proporcionan dichos datos
execute:
  warning: false
  cache: true
  freeze: true

bibliography: references.bib
csl: template/ieee.csl

format:
  pdf:
    cite-method: biblatex
    toc: true
    lof: true  # List of figures
    lot: true  # List of tables
    documentclass: article
    include-in-header:
      - template/packages.tex
      - template/header-footer.tex
    template-partials:
      - template/before-body.tex
    number-sections: true
    papersize: a4
    geometry:
      - left=3cm
      - right=3cm
      - top=2cm
      - bottom=2.7cm
    mainfont: "Times New Roman"
---
```
### Configuración de Celdas

```{python}
#| label: descriptive-name
#| fig-cap: "Descripción de la figura"
#| fig-width: 12
#| fig-height: 8
#| output: asis         # Para tablas markdown
#| tbl-cap: "Título de tabla"
#| tbl-colwidths: [25,25,25,25]
```

## 13. CONTROL DE CALIDAD Y CHECKLIST

### Checklist Pre-Entrega

#### Contenido Académico:
- [ ] Introducción con contexto y objetivos claros
- [ ] Metodología replicable con suficiente detalle
- [ ] Resultados presentados objetivamente
- [ ] Discusión que interpreta hallazgos
- [ ] Conclusiones que responden a objetivos
- [ ] Mínimo 15 referencias actuales

#### Calidad Técnica:
- [ ] Código reproducible con semillas fijas
- [ ] Todas las figuras tienen títulos y son legibles
- [ ] Tablas autoexplicativas con formateo correcto
- [ ] Métricas apropiadas para el problema
- [ ] Validación cruzada implementada
- [ ] Limitaciones identificadas y discutidas

#### Formato y Estilo:
- [ ] Referencias cruzadas funcionando (@fig-, @tbl-, @sec-)
- [ ] Citaciones en formato correcto
- [ ] Numeración automática de figuras y tablas
- [ ] Texto en español académico formal
- [ ] Sin errores de ortografía o gramática
- [ ] Estructura lógica y fluida

## 14. ESTÁNDARES DE CALIDAD ESPECÍFICOS

### Números y Precisión

```{python}
# Estándares de redondeo
formatos_estandar = {
    'porcentajes': '{:.2f}%',           # 85.23%
    'metricas_ml': '{:.4f}',            # 0.8823
    'p_valores': '{:.3f}',              # 0.001
    'tiempos': '{:.2f}s',               # 12.35s
    'montos': '${:,.0f}',               # $1,234,567
    'enteros': '{:,}',                  # 10,000
    'decimales_2': '{:.2f}'             # 12.34
}
```

### Lenguaje Académico

#### ✅ Estilo Correcto:
- "Los resultados demuestran que..."
- "Se observa una mejora significativa..."
- "El análisis revela patrones consistentes..."
- "La implementación permite optimizar..."

#### ❌ Evitar:
- "Creemos que..." (opinión personal)
- "Es obvio que..." (asunción)
- "Los datos son buenos" (impreciso)
- "El modelo funciona bien" (vago)

### Terminología Técnica Precisa

#### Machine Learning:
- Hiperparámetros (no "parámetros de configuración")
- Validación cruzada (no "validación múltiple")
- Sobreajuste/overfitting (consistente)
- Características/variables predictoras

#### Estadística:
- Significancia estadística (con p-valores)
- Intervalos de confianza (con nivel especificado)
- Correlación vs causalidad (distinción clara)
- Sesgo y varianza (términos técnicos)

#### Riesgo Crediticio:
- Tasa de default/incumplimiento
- Scoring crediticio/calificación
- Morosidad/atraso
- Provisiones/pérdidas esperadas

## 15. OPTIMIZACIÓN DE RENDIMIENTO

### Configuración GPU/CUDA

```{python}
# Verificación de hardware
import torch
import tensorflow as tf

display(Markdown(f"""
#### Configuración de Hardware:
- **GPU disponible:** {torch.cuda.is_available()}
- **Dispositivos CUDA:** {torch.cuda.device_count()}
- **GPU actual:** {torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'N/A'}
- **Memoria GPU:** {torch.cuda.get_device_properties(0).total_memory / 1024**3:.1f} GB
"""))

# Configuración para XGBoost
xgb_params_gpu = {
    'tree_method': 'hist',
    'device': 'cuda' if torch.cuda.is_available() else 'cpu',
    'gpu_id': 0
}
```

### Manejo de Memoria

```{python}
# Para datasets grandes
import gc

# Liberar memoria después de operaciones pesadas
del df_temp
gc.collect()

# Monitoreo de memoria
import psutil
memory_usage = psutil.virtual_memory().percent
display(Markdown(f"**Uso de memoria:** {memory_usage:.1f}%"))
```

## 16. ELEMENTOS DE REPRODUCIBILIDAD

### Registro de Entorno

```{python}
# Guardar información completa del entorno
import sys, platform
from datetime import datetime

env_info = {
    'timestamp': datetime.now().isoformat(),
    'python_version': sys.version,
    'platform': platform.platform(),
    'processor': platform.processor(),
    'python_executable': sys.executable
}

# Versiones de librerías críticas
import pkg_resources
installed_packages = [d for d in pkg_resources.working_set]
ml_packages = [p for p in installed_packages 
               if any(lib in str(p) for lib in ['sklearn', 'xgboost', 'torch', 'pandas', 'numpy'])]

display(Markdown("#### Librerías Críticas:"))
for package in ml_packages:
    display(Markdown(f"- **{package.project_name}:** {package.version}"))
```

### Configuración de Semillas

```{python}
# Configuración completa de reproducibilidad
import random
import os

def set_all_seeds(seed=42):
    """Configura todas las semillas para reproducibilidad"""
    random.seed(seed)
    os.environ['PYTHONHASHSEED'] = str(seed)
    np.random.seed(seed)
  
    # Para PyTorch
    if 'torch' in sys.modules:
        torch.manual_seed(seed)
        torch.cuda.manual_seed(seed)
        torch.cuda.manual_seed_all(seed)
        torch.backends.cudnn.deterministic = True
        torch.backends.cudnn.benchmark = False
  
    # Para TensorFlow
    if 'tensorflow' in sys.modules:
        tf.random.set_seed(seed)

set_all_seeds(42)
```

### Documentación de Decisiones

```{python}
# Registro de decisiones importantes
decisiones_metodologicas = {
    'division_datos': {
        'test_size': 0.2,
        'validacion_size': 0.2,
        'estratificacion': True,
        'justificacion': 'Mantener distribución de clases en todas las particiones'
    },
    'preprocesamiento': {
        'outliers_method': 'winsorization',
        'normalizacion': 'StandardScaler',
        'imputacion': 'No requerida (0% faltantes)'
    },
    'seleccion_features': {
        'metodos_usados': ['F-test', 'RFE', 'RF-importance', 'Lasso', 'MI'],
        'consenso_minimo': 4,
        'features_finales': len(variables_finales)
    }
}

# Guardar como JSON para trazabilidad
import json
with open('models/decisiones_metodologicas.json', 'w') as f:
    json.dump(decisiones_metodologicas, f, indent=2)
```

Este titulo debe ir al final del documento, para que bibliografia tenga un titulo propio y no se confunda con el resto del contenido.
# Bibliografía