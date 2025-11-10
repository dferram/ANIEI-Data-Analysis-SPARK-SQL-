```markdown
# ANIEI-Data-Analysis-SPARK-SQL
¡Bienvenido! 🚀  
Este repositorio contiene un análisis de datos de ventas realizado con PySpark y visualizaciones finales con matplotlib y seaborn. El objetivo es demostrar cómo cargar, limpiar, transformar y analizar un dataset de ventas del mundo real usando Spark SQL para obtener insights accionables (ventas por producto, tendencia temporal, top clientes, etc.).

Descripción corta
-----------------
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

Instalación rápida
------------------
1. Crear entorno virtual:
   python -m venv .venv && source .venv/bin/activate
2. Instalar dependencias:
   pip install pyspark pandas matplotlib seaborn jupyter

Cómo usar
--------
- Abrir y ejecutar el notebook en Jupyter:
  jupyter notebook ANIEI_Data_Analysis.ipynb
- O ejecutar transformaciones por línea con spark-submit si hay scripts .py:
  spark-submit --master local[*] scripts/analyze_sales.py

Estructura del repositorio
--------------------------
- ANIEI_Data_Analysis.ipynb  — Notebook principal con todo el flujo.
- data/                      — Carpeta esperada para los CSV de entrada (no incluida).
- outputs/                   — Carpeta para resultados exportados (parquet/csv/png).
- scripts/                   — Scripts auxiliares (si existen).

Flujo de procesamiento (paso a paso)
-----------------------------------
A continuación se explica detalladamente lo que hace el código, paso a paso:

1) Inicialización de Spark
   - Se crea un SparkSession con configuraciones iniciales (memoria, paralelismo).
   - Ejemplo: SparkSession.builder.appName("SalesAnalysis").getOrCreate()

2) Carga de datos
   - Lectura de archivos CSV con opciones: header=True, inferSchema=False (preferible definir schema).
   - Soporta particionado y lectura desde rutas locales o HDFS/S3.
   - Se cargan también tablas lookup opcionales (categorías, regiones).

3) Definición y aplicación de esquema
   - Se define un esquema explícito para forzar tipos correctos (date, int, double, string).
   - Conversión de columnas de fecha con to_date / unix_timestamp cuando sea necesario.

4) Limpieza y normalización
   - Eliminación o imputación de nulos en columnas críticas (cantidad, precio, fecha).
   - Eliminación de duplicados.
   - Normalización de texto (trim, lower) para categorías y nombres de producto.
   - Correcciones básicas (valores negativos en cantidad o precio).

5) Enriquecimiento y columnas derivadas
   - Creación de columnas calculadas: total_sale = quantity * unit_price.
   - Extracción de componentes temporales: year, month, day, year_month.
   - Mapeo de códigos a nombres usando joins con tablas de referencia (si existen).

6) Registro de vistas temporales (Spark SQL)
   - Las tablas transformadas se registran como vistas temporales:
     df.createOrReplaceTempView("sales")
   - Permite ejecutar consultas SQL para análisis ad-hoc y reportes.

7) Agregaciones y consultas SQL
   - Ventas por categoría / producto / región:
     SELECT category, SUM(total_sale) FROM sales GROUP BY category ORDER BY SUM(total_sale) DESC
   - Top N productos por ventas usando window functions:
     ROW_NUMBER() OVER (PARTITION BY category ORDER BY total_sale DESC)
   - Tendencias temporales: ventas mensuales, crecimiento año a año (YoY), promedio móvil.

8) Análisis avanzado
   - Identificación de clientes más valiosos (RFM / recency-frequency-monetary simple).
   - Detección de estacionalidad y picos en series temporales.
   - Pivot tables (por ejemplo, ventas por mes por categoría) con pivot() o SQL.

9) Guardado de resultados
   - Exportar tablas finales a Parquet para uso posterior:
     df.write.mode("overwrite").parquet("outputs/sales_aggregated.parquet")
   - Opcional export to CSV para compartir con stakeholders.

10) Visualización
   - Conversión de pequeñas muestras o agregaciones a pandas (toPandas) para graficar.
   - Gráficas incluidas: barras (top productos), series temporales (ventas por mes), heatmaps (categoría vs mes).
   - Uso de seaborn para estilos y matplotlib para ajustes finales.

11) Conclusiones y recomendaciones
   - El notebook sugiere insights (ej. categorías que impulsan la facturación, productos con caída en ventas).
   - Recomendaciones típicas: revisar pricing, campañas estacionales, optimizar inventario en top-N productos.

Buenas prácticas y rendimiento
------------------------------
- Defina esquemas en la lectura para evitar inferencia costosa.
- Use persist/caching en DataFrames reutilizados varias veces.
- Particione la salida Parquet por year_month si hará consultas por rango de fecha.
- Evite toPandas() sobre conjuntos grandes; convierta solo las agregaciones pequeñas.

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

Contribuciones y contacto
-------------------------
Si quieres mejorar el análisis (más KPIs, integraciones, pipelines), abre un issue o PR.  
Autor: ferramdr  
Contacto: ver perfil en GitHub

Licencia
--------
Incluye la licencia que prefieras (MIT, Apache-2.0, etc.) — actualmente no hay una licencia explícita en el repo.

¡Listo! Este README te da una guía completa para entender y ejecutar el análisis en el notebook. Si quieres, puedo:
- Generar snippets concretos de Spark SQL o PySpark para cada paso.
- Añadir instrucciones para desplegar en Databricks/AWS EMR.
- Crear un script spark-submit listo para producción.
Dime cuál prefieres y lo preparo.
```
