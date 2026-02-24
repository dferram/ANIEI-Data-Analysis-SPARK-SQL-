# ANIEI-Data-Analysis-SPARK-SQL
Este repositorio contiene un análisis de datos de ventas realizado con PySpark y visualizaciones finales con matplotlib y seaborn. El objetivo es demostrar cómo cargar, limpiar, transformar y analizar un dataset de ventas del mundo real usando Spark SQL para obtener insights accionables (ventas por producto, tendencia temporal, top clientes, etc.).

Descripción
--------------------------
El notebook procesa un conjunto de datos de ventas, aplica transformaciones y agrega información con SQL y DataFrame API de PySpark. Después realiza análisis exploratorio y produce visualizaciones que ayudan a entender patrones de ventas por producto, categoría, región y tiempo.

Características principales
--------------------------
- Lectura eficiente de grandes archivos CSV con PySpark.
- Limpieza y normalización de campos (fechas, nulos, tipos).
- Cálculo de métricas derivadas (precio total, márgenes, unidades por pedido).
- Consultas SQL sobre vistas temporales para extraer métricas de negocio.
- Agregaciones y ventanas para identificar top-N productos/clientes.
- Series temporales mensuales y análisis de estacionalidad.
- Exportación de resultados a Parquet/CSV y gráficos con matplotlib/seaborn.

Requisitos
----------
- Python 3.8+  
- Apache Spark compatible con PySpark (recomendado 3.x)  
- Paquetes Python: pyspark, pandas, matplotlib, seaborn, jupyter (o jupyterlab)  
- Opcional: entorno con más memoria si trabaja con datasets grandes (Spark local config)

Estructura del repositorio
--------------------------
- ANIEI_Data_Analysis.ipynb  — Notebook principal con todo el flujo.
- data/                      — Carpeta esperada para los CSV de entrada (no incluida).
- outputs/                   — Carpeta para resultados exportados (parquet/csv/png).
- scripts/                   — Scripts auxiliares (si existen).

Ejemplos de consultas útiles
----------------------------
- Ventas totales por año:
  SELECT year, SUM(total_sale) AS total FROM sales GROUP BY year ORDER BY year
- Top 10 clientes por ingresos:
  SELECT customer_id, SUM(total_sale) AS revenue FROM sales GROUP BY customer_id ORDER BY revenue DESC LIMIT 10

Qué esperar como resultado
--------------------------
- Tablas agregadas con métricas de negocio (ventas totales, unidades vendidas).
- Visualizaciones PNG/figuras dentro del notebook.
- Archivos Parquet/CSV con resultados listos para reporting.

