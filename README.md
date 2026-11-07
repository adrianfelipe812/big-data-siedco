# Big Data — Laboratorios en Databricks y Lakehouse predictivo con SIEDCO

Laboratorios del curso *Big Data* — Maestría en Ciencia de Datos, Universidad Santo Tomás (2026). Recorren el ciclo completo en **Databricks**: exploración y almacenamiento Delta, adquisición programática desde una API, y una arquitectura **Lakehouse (Bronze → Silver → Gold)** con modelado predictivo en **Spark ML** sobre datos de seguridad de Bogotá.

**Equipo:** Adrian Felipe Grateron · Jhonatan Smith Doncel · Jorge Emmanuel Marín

## Laboratorios

| # | Carpeta | Tema | Tecnologías |
|---|---|---|---|
| 1 | [`lab1-exploracion-delta`](lab1-exploracion-delta) | Exploración, almacenamiento en Delta y consultas SQL sobre un dataset de criptomonedas | PySpark, Delta Lake, Spark SQL |
| 2 | [`lab2-api-coingecko`](lab2-api-coingecko) | Pipeline de adquisición desde la API de CoinGecko: capa RAW (JSON), CSV vs. Parquet, tabla Delta en Unity Catalog y operaciones ACID (INSERT, UPDATE, DELETE, MERGE, *time travel*) | requests, pandas, PySpark, Delta Lake |
| 3 | [`lab3-siedco-lakehouse`](lab3-siedco-lakehouse) | Arquitectura medallion y clasificación de meses con alta variación delictiva por localidad y tipo de hecho | Databricks, Spark, Spark ML |

Cada laboratorio incluye el notebook `.ipynb` exportado de Databricks con sus resultados; los laboratorios 1 y 2 conservan además el export `.html`.

## Laboratorio 3 — SIEDCO: de una base administrativa a una señal predictiva

- **Pregunta:** ¿se pueden identificar condiciones asociadas con una variación alta en el número de casos del mes siguiente?
- **Datos:** SIEDCO (Secretaría Distrital de Seguridad, Convivencia y Justicia), 2018–2026, corte 31/07/2026.
- **Arquitectura:** Bronze 368.651 registros → Silver 18.746 filas mensuales (182 series) → Gold 17.229 registros modelables.
- **Unidad de análisis:** localidad × tipo de hecho × mes.
- **Variables:** cambios a 1, 3 y 7 meses; medias móviles y volatilidad a 3, 7 y 14 meses; contexto nocturno y arma de fuego.
- **Objetivo:** alta variación = cambio logarítmico por encima del percentil 75 (umbral aprendido solo con entrenamiento).
- **Validación temporal:** entrenamiento 2018-08 a 2024-11 (13.594) y prueba 2024-12 a 2026-07 (3.453), sin partición aleatoria.

| Métrica (prueba) | Regresión logística | Random Forest |
|---|---|---|
| Accuracy | 56,6 % | 67,6 % |
| Precision | 33,8 % | 43,4 % |
| Recall | 51,7 % | 38,3 % |
| F1 | 40,9 % | 40,7 % |
| ROC AUC | 57,8 % | 60,1 % |
| PR AUC | 39,5 % | 43,5 % |

El *baseline* de clase mayoritaria logra 71 % de accuracy con 0 % de recall, por eso se reportan varias métricas. La señal es moderada; los conteos administrativos reflejan dinámicas de denuncia y registro, y la importancia de variables no implica causalidad.

Material: [notebook](lab3-siedco-lakehouse/laboratorio3_siedco_lakehouse.ipynb), presentación del laboratorio y [propuesta de solución](docs/propuesta_solucion_nube.pdf). Video: https://youtu.be/tU7MgHjgajs

## Ejecución

Los notebooks se diseñaron para Databricks (Free Edition / Unity Catalog). Usan `spark`, `display` y `dbutils`, disponibles en ese entorno. El laboratorio 2 requiere acceso a Internet para consultar la API pública de CoinGecko.
