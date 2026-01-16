# ADF Movie History

Una solución **Enterprise** de procesamiento de datos de películas utilizando **Azure Data Factory (ADF)** que implementa una arquitectura moderna de data ingestion en capas (Bronze, Silver, Gold).

## 📋 Tabla de Contenidos

- [Descripción General](#descripción-general)
- [Características](#características)
- [Tecnologías](#tecnologías)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Requisitos Previos](#requisitos-previos)
- [Instalación y Configuración](#instalación-y-configuración)
- [Pipelines Principales](#pipelines-principales)
- [Datasets](#datasets)
- [Linked Services](#linked-services)
- [Triggers](#triggers)
- [Documentación](#documentación)

## 🎯 Descripción General

**ADF Movie History** es un proyecto de ingeniería de datos que automatiza el procesamiento, transformación y almacenamiento de información histórica sobre películas. Utiliza Azure Data Factory para orquestar pipelines ETL (Extract, Transform, Load) que integran datos desde múltiples fuentes hacia un data lake estructurado en capas.

El proyecto implementa las mejores prácticas de arquitectura moderna de datos, asegurando escalabilidad, mantenibilidad y calidad de datos.

## ✨ Características

- **Procesamiento en Capas (Medallion Architecture)**
  - **Bronze**: Ingesta bruta de datos sin transformar
  - **Silver**: Datos limpiados, validados y enriquecidos
  - **Gold**: Datos listos para análisis y reporting

- **Automatización Inteligente**
  - Pipelines programados automáticamente mediante triggers
  - Monitoreo y alertas en tiempo real
  - Manejo robusto de errores y reintentos

- **Escalabilidad Enterprise**
  - Procesamiento de grandes volúmenes de datos
  - Optimizado para costos en Azure
  - Arquitectura modular y reutilizable

- **Calidad de Datos**
  - Validaciones integradas en cada capa
  - Trazabilidad completa de transformaciones
  - Versionado de datos y auditoría

- **Orquestación Centralizada**
  - Control total desde Azure Data Factory
  - Dependencias entre pipelines bien definidas
  - Ejecución paralela cuando es posible

## 🔧 Tecnologías

| Tecnología | Descripción | Versión |
|-----------|-------------|---------|
| **Azure Data Factory** | Servicio ETL/ELT en la nube | Latest |
| **Azure Data Lake Storage Gen2** | Almacenamiento de datos escalable | Gen2 |
| **Azure SQL Database** | Base de datos relacional | SQL Server 2019+ |
| **Azure Synapse Analytics** | Data warehouse analítico (opcional) | - |
| **Power BI** | Visualización y reporting (opcional) | Latest |
| **Git/GitHub** | Control de versiones | - |
| **JSON** | Configuración de pipelines | - |

## 📁 Estructura del Proyecto

adf-movie-history/
├── dataset/ # Definiciones de datasets
│ ├── ds_data_history_bronze/ # Dataset de capa Bronze
│ ├── ds_data_history_silver/ # Dataset de capa Silver
│ └── ds_data_history_gold/ # Dataset de capa Gold
│
├── factory/ # Configuración de la factoría ADF
│ └── adf-config.json # Configuración principal
│
├── linkedService/ # Conexiones a servicios externos
│ ├── ls_azure_storage/ # Azure Data Lake Storage
│ ├── ls_azure_sql_db/ # Azure SQL Database
│ └── ls_data_sources/ # Fuentes de datos externas
│
├── pipeline/ # Pipelines de procesamiento
│ ├── pl_extract_movies/ # Extrae datos de fuentes
│ ├── pl_transform_silver/ # Transforma a capa Silver
│ ├── pl_transform_gold/ # Transforma a capa Gold
│ └── pl_main_orchestration/ # Orquesta todo el flujo
│
├── trigger/ # Triggers de ejecución
│ ├── tr_daily_schedule/ # Ejecución diaria programada
│ ├── tr_event_based/ # Trigger basado en eventos
│ └── tr_manual/ # Trigger manual
│
├── README.md # Este archivo
└── publish_config.json # Configuración de publicación


## 📋 Requisitos Previos

Antes de comenzar, asegúrate de tener:

- **Suscripción activa de Azure**
- **Credenciales de acceso** con permisos de:
  - Crear y gestionar Azure Data Factory
  - Acceder a Azure Data Lake Storage Gen2
  - Acceder a Azure SQL Database (si aplica)
- **Visual Studio Code** o **Azure Data Studio** (opcional, para desarrollo)
- **CLI de Azure** instalado (`az` command)
- **Git** para control de versiones

## 🚀 Instalación y Configuración

### 1. Clonar el Repositorio

```bash
git clone https://github.com/jordytiradotorres/adf-movie-history.git
cd adf-movie-history
```

### 2. Configurar Azure CLI

# Iniciar sesión en Azure
az login

# Seleccionar suscripción
az account set --subscription "Tu-ID-Suscripción"

### 3. Importar en Azure Data Factory

1. Abre Azure Portal
2. Navega a tu instancia de Azure Data Factory
3. Ve a Autor → Source Control
4. Conecta tu repositorio de GitHub
5. Importa todas las definiciones desde esta rama

### 4. Configurar Conexiones (Linked Services)

- Actualiza las credenciales en cada Linked Service
- Configura las cadenas de conexión correctas para tus recursos Azure
- Prueba la conectividad desde ADF

### 5. Ejecutar Pipeline Principal

# Desplegar cambios desde el repositorio
az datafactory pipeline create-run \
  --resource-group "Tu-Grupo-Recursos" \
  --factory-name "Tu-Factoría-ADF" \
  --name "pl_main_orchestration"

### 📊 Pipelines Principales

- pl_extract_movies
Extrae datos de películas desde fuentes externas (APIs, bases de datos, archivos CSV) y los carga en la capa Bronze.

Entrada: APIs públicas o fuentes de datos
Salida: Archivos Parquet/JSON en Bronze Layer

- pl_transform_silver
Limpia, valida y enriquece los datos de la capa Bronze. Elimina duplicados, normaliza formatos y aplica reglas de negocio.

Entrada: Datos Bronze
Salida: Datos limpios en Silver Layer

- pl_transform_gold
Transforma datos Silver en modelos analíticos optimizados para reporting y dashboards.

Entrada: Datos Silver
Salida: Tablas agregadas en Gold Layer

- pl_main_orchestration
Orquesta la ejecución secuencial de todos los pipelines anteriores, garantizando dependencias y manejo de errores.

### 📦 Datasets

- ds_data_history_bronze: Almacena datos crudos sin procesar
- ds_data_history_silver: Contiene datos transformados y validados
- ds_data_history_gold: Datasets listos para análisis y BI

## 🔗 Linked Services

- ls_azure_storage: Conexión a Azure Data Lake Storage Gen2
- ls_azure_sql_db: Conexión a Azure SQL Database
- ls_data_sources: Conexiones a fuentes externas de datos

## ⏲️ Triggers

- tr_daily_schedule: Ejecuta pipelines diariamente a las 2:00 AM
- tr_event_based: Se activa cuando se detectan cambios en los datos
- tr_manual: Permite ejecución manual desde Azure Portal

## 📚 Documentación Adicional

Para más información sobre Azure Data Factory:

Documentación oficial de Azure Data Factory
Patrones de arquitectura ETL
Mejores prácticas de Data Lake
Monitoreo y alertas en ADF
