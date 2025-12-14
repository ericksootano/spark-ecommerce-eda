# 📊 E-Commerce Data Analysis & Engineering con PySpark

![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=databricks&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache%20Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)

> **Un proyecto End-to-End de Ingeniería de Datos y Análisis Exploratorio (EDA) simulando un entorno de Big Data.**

## 📖 Descripción del Proyecto

Este repositorio contiene el flujo de trabajo completo para el procesamiento y análisis de un dataset transaccional de E-Commerce real (~540,000 registros). El objetivo principal fue transformar datos crudos y "sucios" en un **Dashboard Ejecutivo** accionable, utilizando la potencia de procesamiento distribuido de **Apache Spark (PySpark)** en la plataforma **Databricks Free Edition**.

El proyecto aborda desde la ingesta y limpieza de datos hasta la ingeniería de características compleja y la visualización de estrategias de negocio.

---

## 🚀 Dashboard Ejecutivo

*(Aquí puedes colocar la captura de pantalla de tu Dashboard completo)*
![Dashboard Preview](![Dashboard](1.DashboardE-commerce.png)

---

## ⚙️ Arquitectura y Tecnologías

* **Plataforma:** Databricks (Spark 4.0)
* **Lenguajes:** Python (PySpark) y Spark SQL.
* **Limpieza de Datos:** Manejo de formatos de fecha inconsistentes (`try_to_timestamp`, `coalesce`), imputación de nulos y eliminación de duplicados.
* **Ingeniería de Características:** Creación de métricas temporales (Year-Month) y financieras (Revenue por línea).
* **Análisis Avanzado:** Segmentación RFM (Recencia, Frecuencia, Monetario) utilizando **Window Functions** y **CTEs**.

---

## 💡 Insights de Negocio Clave

Tras procesar los datos, se descubrieron los siguientes patrones estratégicos:

| Insight | Descripción | Impacto |
| :--- | :--- | :--- |
| **👑 Dominio del Reino Unido** | El **~90%** de los ingresos provienen de UK. | Riesgo alto de dependencia de un solo mercado. Se recomienda expansión a Alemania/Francia. |
| **📅 La "Hora Dorada"** | El 80% de las transacciones ocurren entre **10:00 AM y 3:00 PM** (Lun-Jue). | Ventana crítica para soporte al cliente y campañas de marketing. Evitar mantenimientos en este horario. |
| **💎 Segmentación VIP** | Un pequeño grupo de clientes **"Champions"** (Score 4-4-4) genera la mayor parte del valor. | Prioridad absoluta en retención. Es más rentable fidelizarlos que adquirir nuevos. |
| **📈 Estacionalidad** | Pico dramático de ventas en **Noviembre**. | La planificación de inventario para Q4 debe comenzar en Septiembre. |

---

## 🛠️ Desafíos Técnicos Superados

### 1. Fechas en Spark 4.0
El dataset presentaba formatos mixtos (`M/d/yyyy` vs `MM/dd/yyyy`) que causaban fallos en el pipeline.
**Solución:** Implementación de una lógica de coalescencia robusta:

```python
# Snippet de la solución
df_cleaned = df_cleaned.withColumn(
    "InvoiceDate",
    F.coalesce(
        F.try_to_timestamp(F.col("InvoiceDate"), F.lit("M/d/yyyy H:m")),
        F.try_to_timestamp(F.col("InvoiceDate"), F.lit("MM/dd/yyyy HH:mm"))
    )
)
```

### 2. Variables SQL en Spark 4.0
La nueva versión de Spark maneja diferente la inyección de variables en SQL (SET variable...). Solución: Uso de f-strings de Python para inyectar parámetros dinámicos (como la fecha de corte para el análisis RFM) directamente en las consultas SQL.

## 📊 Visualizaciones Destacadas
Mapa de Calor (Patrones de Compra)
Muestra la concentración de ventas por día y hora.

### Top Productos (Pareto)
Identificación de los Best Sellers.

📂 Estructura del Repositorio
- EDA_ECommerce_Project.dbc: Archivo nativo de Databricks (incluye datos, código y dashboard).

- EDA_ECommerce_Project.ipynb: Versión Jupyter Notebook para visualización en GitHub.

- README.md: Documentación del proyecto.

👨‍💻 Autor
Erickson Otaño Ingeniero de Datos | Cloud Data Platforms

Este proyecto fue realizado como parte de una práctica intensiva de procesamiento de datos a gran escala.
