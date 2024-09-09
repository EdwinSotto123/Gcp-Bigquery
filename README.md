# Gcp-Bigquery-Predict Weight Penguin
# Proyecto de Modelado de Regresión Lineal en BigQuery

## Descripción

Este proyecto utiliza BigQuery ML para crear y evaluar un modelo de regresión lineal que predice el peso de los pingüinos basado en diversas características. Incluye pasos para crear un conjunto de datos, entrenar el modelo, evaluar su rendimiento, y entender las predicciones generadas.

## Pasos del Proyecto

### 1. Crear el Dataset

1. Accede a BigQuery desde la Consola de Google Cloud.
2. En el panel de navegación, selecciona el proyecto y haz clic en **Crear dataset**.
3. Introduce `bqml_tutorial` como Dataset ID.
4. Selecciona la ubicación de datos como `us` (opcional).
5. Deja el resto de las configuraciones por defecto y haz clic en **Crear Dataset**.
![image](https://github.com/user-attachments/assets/e5a06335-b823-4ab8-bc81-72572b40ae7a)

### 2. Crear el Modelo

1. Ejecuta la siguiente consulta SQL para crear y entrenar un modelo de regresión lineal:

    ```sql
    #standardSQL
    CREATE OR REPLACE MODEL `bqml_tutorial.penguins_model`
    OPTIONS
      (model_type='linear_reg',
      input_label_cols=['body_mass_g']) AS
    SELECT
      *
    FROM
      `bigquery-public-data.ml_datasets.penguins`
    WHERE
      body_mass_g IS NOT NULL
    ```

2. Este comando crea un modelo de regresión lineal que predice el peso de los pingüinos.
![image](https://github.com/user-attachments/assets/7b9e4478-4ef4-48d1-b039-ece297bd87a1)

### 3. Obtener Estadísticas de Entrenamiento (Opcional)

1. Para ver las estadísticas del modelo, accede al panel de **Entrenamiento** en la consola de BigQuery.
2. Observa las métricas como el error cuadrático medio, que indican el rendimiento del modelo.

![image](https://github.com/user-attachments/assets/d0fe3cd4-6158-480a-87c5-16a45f2a990b)

### 4. Evaluar el Modelo

1. Ejecuta la siguiente consulta para evaluar el modelo:

    ```sql
    #standardSQL
    SELECT
      *
    FROM
      ML.EVALUATE(MODEL `bqml_tutorial.penguins_model`,
        (
        SELECT
          *
        FROM
          `bigquery-public-data.ml_datasets.penguins`
        WHERE
          body_mass_g IS NOT NULL))
    ```

2. Revisa las métricas de evaluación como el error absoluto medio y el R2 para entender el rendimiento del modelo.
![image](https://github.com/user-attachments/assets/bdb77848-c26f-4f43-bcb9-aae820e04f38)

### 5. Predecir Resultados

1. Utiliza el modelo para predecir el peso de los pingüinos en la isla Biscoe:

    ```sql
    #standardSQL
    SELECT
      *
    FROM
      ML.PREDICT(MODEL `bqml_tutorial.penguins_model`,
        (
        SELECT
          *
        FROM
          `bigquery-public-data.ml_datasets.penguins`
        WHERE
          body_mass_g IS NOT NULL
          AND island = "Biscoe"))
    ```
![image](https://github.com/user-attachments/assets/02f384b7-5528-4558-903f-c5e007cef46e)

### 6. Explicar Resultados de Predicción con Métodos Explicativos

1. Ejecuta la siguiente consulta para obtener explicaciones de las predicciones:

    ```sql
    #standardSQL
    SELECT
      *
    FROM
      ML.EXPLAIN_PREDICT(MODEL `bqml_tutorial.penguins_model`,
        (
        SELECT
          *
        FROM
          `bigquery-public-data.ml_datasets.penguins`
        WHERE
          body_mass_g IS NOT NULL
          AND island = "Biscoe"),
        STRUCT(3 as top_k_features))
    ```

2. Revisa las columnas de atribución de características para entender las contribuciones a las predicciones.
![image](https://github.com/user-attachments/assets/990c9d4b-c93e-43dc-8bf7-81d59a1515ef)

### 7. Explicación Global del Modelo (Opcional)

1. Reentrena el modelo con la opción de explicación global habilitada:

    ```sql
    #standardSQL
    CREATE OR REPLACE MODEL bqml_tutorial.penguins_model
    OPTIONS
      (model_type='linear_reg',
      input_label_cols=['body_mass_g'],
      enable_global_explain=TRUE) AS
    SELECT
      *
    FROM
      `bigquery-public-data.ml_datasets.penguins`
    WHERE
      body_mass_g IS NOT NULL
    ```

2. Obtén explicaciones globales con la siguiente consulta:

    ```sql
    #standardSQL
    SELECT
      *
    FROM
      ML.GLOBAL_EXPLAIN(MODEL `bqml_tutorial.penguins_model`)
    ```

---
![image](https://github.com/user-attachments/assets/cc1e9bd1-a74c-4db8-aac9-ad2888a33e73)

Este README proporciona una visión general de las tareas y comandos necesarios para completar el proyecto. Asegúrate de seguir cada paso y revisar los resultados en la Consola de Google Cloud para obtener una comprensión completa del rendimiento y las explicaciones del modelo.
