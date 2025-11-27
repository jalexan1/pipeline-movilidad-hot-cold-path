Pipeline de Movilidad — Hot Path & Cold Path
Ingeniero de Datos

📌 Descripción General

Este proyecto implementa un pipeline completo de movilidad capaz de procesar eventos (viajes de taxis de NYC) simulados como un flujo de datos (streaming), alimentando dos capas o rutas:

Hot Path (Operacional): Mantiene el último estado de cada vehículo con consultas de baja latencia.

Cold Path (Analítico): Conserva el historial completo para análisis avanzados y reportes.

La lógica y procesamiento se desarrollaron en un Notebook de Google Colab, haciendo el proyecto portable, ejecutable sin instalaciones y totalmente reproducible.

🌐 Repositorio & Notebook Público

📁 Repositorio GitHub (código fuente):
https://github.com/jalexan1/pipeline-movilidad-hot-cold-path.git

📘 Notebook ejecutable en Google Colab:
https://colab.research.google.com/drive/1TtxphEppf_30IgVFy56oR2YPgz9Xp77P?usp=sharing

🏗️ Arquitectura Implementada

El pipeline fue diseñado siguiendo una arquitectura de doble ruta:

                   +---------------------------+
                   |      Data Source (Parquet)|
                   +-------------+-------------+
                                 |
                                 ↓
                      Simulated Stream (for loop)
                                 |
          +----------------------+----------------------+
          |                                             |
          ↓                                             ↓
 +------------------+                         +--------------------+
 |   HOT PATH       |                         |   COLD PATH        |
 |  (Operacional)   |                         |   (Analítico)      |
 +------------------+                         +--------------------+
 | Redis Simulado   |                         | Historial completo |
 | con diccionario  |                         | en DataFrame       |
 +------------------+                         +--------------------+
 | Último estado    |                         | Consultas OLAP     |
 +------------------+                         +--------------------+


✔ Hot Path → Último estado por vehículo (consultas OLTP rápidas)
✔ Cold Path → Historial completo (consultas OLAP y agregaciones)

| Tecnología                              | Uso                   | Justificación                                             |
| --------------------------------------- | --------------------- | --------------------------------------------------------- |
| **Python + Pandas**                     | Ingesta y preparación | Fácil manejo del dataset Parquet y procesamiento rápido.  |
| **requests**                            | Descarga de datos     | Permite bajar archivos remotos en stream.                 |
| **Diccionario Python (Redis simulado)** | Hot Path              | Ideal para representar estructura Key-Value en memoria.   |
| **DataFrame (Cold Path)**               | Analítica             | Permite consultas históricas, agregaciones, estadísticas. |
| **Google Colab**                        | Entorno de ejecución  | Ejecutable sin instalación, reproduce todo con un clic.   |



Cómo Ejecutar el Pipeline
Opción 1 — Ejecutar directo en Google Colab (recomendado)

Abrir el notebook:
👉 https://colab.research.google.com/drive/1TtxphEppf_30IgVFy56oR2YPgz9Xp77P?usp=sharing

Hacer clic en Runtime → Run all

Esperar que se complete el pipeline (descarga, procesamiento, consultas).

Opción 2 — Ejecutarlo localmente

Clonar el repositorio:
Cómo Ejecutar el Pipeline
Opción 1 — Ejecutar directo en Google Colab (recomendado)

Abrir el notebook:
👉 https://colab.research.google.com/drive/1TtxphEppf_30IgVFy56oR2YPgz9Xp77P?usp=sharing

Hacer clic en Runtime → Run all
Esperar que se complete el pipeline (descarga, procesamiento, consultas).

Opción 2 — Ejecutarlo localmente
Clonar el repositorio:
git clone https://github.com/jalexan1/pipeline-movilidad-hot-cold-path.git
cd pipeline-movilidad-hot-cold-path

2.Instalar dependencias:
pip install pandas pyarrow duckdb redis requests

3.Ejecutar el notebook o correr el .py (si agregas versión script).

Consultas de Demostración

Estas consultas prueban que los requisitos operacionales y analíticos están completamente cubiertos.

🟦 Consultas Operacionales (Hot Path)

Simulan un almacén de estado tipo Redis.

1. Último estado de un vehículo
   vehicle_state = operational_store["2_186"]
   vehicle_state

2. Top zonas con más vehículos activos
   sorted_zones = sorted(vehicles_by_zone.items(), key=lambda x: len(x[1]), reverse=True)
   sorted_zones[:5]


Consultas Analíticas (Cold Path)
Sobre historial completo tipo OLAP.
3. Estadísticas generales
df_analytics.describe()

4. Zonas con más viajes
   df_analytics['pickup_location'].value_counts().head(10)

5. Análisis por hora del día
 df_analytics.groupby('hour')['vehicle_id'].count()


Métricas del Pipeline
Durante ejecución con 10.000 viajes:
| Métrica                | Resultado                      |
| ---------------------- | ------------------------------ |
| Vehículos únicos       | 193                            |
| Eventos procesados     | 10.000                         |
| Velocidad del pipeline | ~8.300 eventos/seg             |
| Hot Store              | Último estado de cada vehículo |
| Cold Store             | 10.000 registros históricos    |


Requisitos Cubiertos
| Requisito                          | Estado |
| ---------------------------------- | ------ |
| Ingesta en streaming simulado      | ✔      |
| Hot Path — Último estado vehicular | ✔      |
| Cold Path — Historial completo     | ✔      |
| Consultas operacionales            | ✔      |
| Consultas analíticas               | ✔      |
| Documentación y entregables        | ✔      |


Autor

Jhon Alexander Tuquerrez
Ingeniero de Datos | Backend & BI Developer





