# Proyecto del Curso de Machine Learning II

Este repositorio contiene el trabajo realizado por los estudiantes del curso "Y1017-Y04AN1-2025-2-Machine_Learning_II". El objetivo principal del proyecto es aplicar técnicas avanzadas de Machine Learning y documentar el proceso mediante informes personalizados utilizando Quarto.

## Contenido del Repositorio

- **Archivos de Proyecto de R**:
  - `CPP3_Apaza_Cabezas_Ponce_Ruiz.Rproj`: Archivo de proyecto de RStudio.
  - `CPP3_Apaza_Cabezas_Ponce_Ruiz.qmd`: Documento Quarto que contiene el informe detallado del proyecto.

- **Conjuntos de Datos**:
  - `X_train.npy`: Datos de entrenamiento en formato NumPy.
  - `X_test.npy`: Datos de prueba en formato NumPy.

- **Imágenes y Gráficos**:
  - `comparacion_modelos_optimized.png`: Gráfico comparativo de modelos optimizados.
  - `matriz_confusion_stacking_h2o_optimized.png`: Matriz de confusión del modelo de stacking optimizado con H2O.
  - `precision_prueba_cnn.png`: Gráfico de precisión de la prueba con CNN.
  - `precision_prueba_hibrido.png`: Gráfico de precisión del modelo híbrido.
  - `rf_grid_search_optimized.png`: Resultado de la búsqueda en malla para Random Forest optimizado.
  - `stacked_ensemble_entrenado_optimized.png`: Visualización del ensemble apilado entrenado y optimizado.

- **Archivos de Configuración y Recursos**:
  - `.gitignore`: Archivos y directorios ignorados por Git.
  - `_quarto.yml`: Archivo de configuración de Quarto.
  - `before-body.tex`: Archivo LaTeX para personalizar el preámbulo del documento.
  - `encabezado.png`: Imagen utilizada en el encabezado del informe.
  - `logo_continental.png`: Logo institucional utilizado en el informe.

## Uso de Quarto para la Generación de Informes Personalizados

Los estudiantes han utilizado Quarto para elaborar informes reproducibles y personalizados que documentan el proceso y los resultados del proyecto. Quarto permite integrar código, texto y visualizaciones en un solo documento, facilitando la comunicación efectiva de los hallazgos. citeturn0search0

Para generar el informe en formato PDF, ejecute el siguiente comando en la terminal:

```bash
quarto render CPP3_Apaza_Cabezas_Ponce_Ruiz.qmd --to pdf
```

Este comando procesará el archivo `.qmd` y producirá un documento PDF con el informe completo.

## Instrucciones para Reproducir el Proyecto

1. **Clonar el Repositorio**:

   ```bash
   git clone https://github.com/jersonalvr/quarto.git
   ```

2. **Instalar Dependencias**:

   Asegúrese de tener instaladas las siguientes herramientas y paquetes:

   - [R](https://cran.r-project.org/)
   - [RStudio](https://www.rstudio.com/)
   - [Python](https://www.python.org/)
   - Paquetes de R y Python necesarios (especificados en el archivo `CPP3_Apaza_Cabezas_Ponce_Ruiz.qmd`)

3. **Abrir el Proyecto en RStudio**:

   Abra el archivo `CPP3_Apaza_Cabezas_Ponce_Ruiz.Rproj` con RStudio para acceder al entorno de trabajo configurado.

4. **Ejecutar el Análisis**:

   Siga las instrucciones y bloques de código proporcionados en el archivo `CPP3_Apaza_Cabezas_Ponce_Ruiz.qmd` para reproducir el análisis y generar los resultados.

5. **Generar el Informe**:

   Utilice Quarto para renderizar el informe en el formato deseado (PDF, HTML, etc.) siguiendo el comando mencionado anteriormente.
