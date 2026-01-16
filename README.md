# ADF Movie History 🎬

Una solución **Enterprise** de procesamiento y transformación de datos de películas utilizando **Azure Data Factory (ADF)** que implementa una arquitectura moderna de data ingestion y orquestación en capas (Bronze → Silver → Gold) integrada con **Databricks** para análisis avanzados.

## 📋 Tabla de Contenidos

- [Descripción General](#descripción-general)
- [Características](#características)
- [Tecnologías](#tecnologías)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Requisitos Previos](#requisitos-previos)
- [Instalación y Configuración](#instalación-y-configuración)
- [Guía de Uso](#guía-de-uso)
- [Componentes Principales](#componentes-principales)
- [Pipelines](#pipelines)
- [Flujo de Datos](#flujo-de-datos)
- [Monitoreo y Triggers](#monitoreo-y-triggers)

## 🎯 Descripción General

**ADF Movie History** es una solución completa de ingeniería de datos que automatiza el procesamiento de información histórica sobre películas. Utiliza **Azure Data Factory** como orquestador central para gestionar pipelines ETL, conectándose a **Databricks** para análisis avanzados y almacenándose en **Azure Data Lake Storage (ADLS)**.

El proyecto implementa una arquitectura **medallion** (Bronze-Silver-Gold) asegurando:
- Escalabilidad enterprise
- Calidad de datos garantizada
- Automatización completa
- Monitoreo en tiempo real
- Transformaciones reproducibles

## ✨ Características

### 🔄 Orquestación Inteligente
- **Pipelines automáticos** con Azure Data Factory
- **Triggers programados** para ejecución periódica
- **Dependencias encadenadas** entre pipelines
- **Manejo robusto de errores** y reintentos automáticos
- **Monitoreo en tiempo real** de ejecuciones

### 📊 Arquitectura Medallion
- **Bronze**: Ingesta de datos raw sin transformar
- **Silver**: Datos limpios, validados y normalizados
- **Gold**: Datos listos para analytics y BI
- **Integración con Databricks** para procesamiento distribuido
- **Almacenamiento en Azure Data Lake Storage** escalable

### 🔗 Conectividad Empresarial
- **Linked Services configurados**: Databricks, ADLS
- **Autenticación segura** con credenciales Azure
- **Conexiones reutilizables** entre pipelines
- **Manejo centralizado** de credenciales
- **Soporte para múltiples fuentes** de datos

### 📈 Transformación de Datos
- **Actividades de copia** (Copy Activity) para ingesta
- **Actividades de Databricks** para procesamiento Spark
- **Actividades SQL** para transformaciones
- **Validaciones integradas** en cada capa
- **Auditoría y trazabilidad** completa

### 🎯 Automatización
- **Triggers programados** (daily, hourly, event-based)
- **Notificaciones automáticas** de errores
- **Alertas y monitoreo** integrado
- **Ejecución paralela** cuando es posible
- **Retry logic** inteligente

## 🔧 Tecnologías

| Tecnología | Descripción | Propósito |
|-----------|-------------|----------|
| **Azure Data Factory** | Orquestador ETL en la nube | Gestión de pipelines |
| **Databricks** | Plataforma de analytics | Procesamiento Spark |
| **Azure Data Lake Storage Gen2** | Almacenamiento escalable | Data lake central |
| **Azure Key Vault** | Gestión de secretos | Credenciales seguras |
| **Apache Spark** | Motor distribuido | Transformación datos |
| **Delta Lake** | Formato transaccional | Almacenamiento fiable |
| **SQL Server** | Base de datos relacional | Metadatos y configuración |
| **Azure Monitor** | Monitoreo y alertas | Observabilidad |
| **GitHub** | Control de versiones | Versionado de código |
| **JSON** | Configuración | Definición de componentes |

## 📁 Estructura del Proyecto

```yaml
adf-movie-history/
│
├── 📦 dataset/ # Definiciones de Datasets
│ └── ds_data_history_bronze.json # Dataset Bronze (datos crudos)
│
├── 🏭 factory/ # Configuración de Data Factory
│ └── databricks-course-with-azure-d... # Configuración principal ADF
│
├── 🔗 linkedService/ # Conexiones a Servicios
│ ├── LS_Databricks.json # Linked Service a Databricks
│ └── ls_movie_history_adls.json # Linked Service a ADLS
│
├── 🚀 pipeline/ # Pipelines de Orquestación
│ ├── pl_ingest_movie_history_data.json # Pipeline de ingesta
│ ├── pl_process_movie_history.json # Pipeline de procesamiento
│ └── pl_transformation_movie_histor... # Pipeline de transformación
│
├── ⏲️ trigger/ # Triggers de Ejecución
│ └── tg_process_movie_history.json # Trigger de ejecución programada
│
├── 📋 README.md # Documentación del proyecto
├── 📄 publish_config.json # Configuración de publicación
└── .gitignore # Archivos ignorados en Git
```

## 📋 Requisitos Previos

### Suscripción y Permisos Azure
- **Suscripción activa** de Azure
- **Permisos de Contributor** en el grupo de recursos
- **Acceso a**:
  - Azure Data Factory
  - Azure Data Lake Storage Gen2
  - Azure Key Vault (para credenciales)
  - Azure Databricks

### Herramientas Necesarias
- **Azure CLI** instalado y configurado
- **Git** para control de versiones
- **Visual Studio Code** (opcional, con extensión ARM)
- **Power BI Desktop** (opcional, para visualizaciones)
- **Azure Storage Explorer** (opcional)

### Credenciales y Secretos
- **Conexión a Databricks**: Token PAT (Personal Access Token)
- **Conexión a ADLS**: Clave de almacenamiento o Managed Identity
- **Service Principal** (recomendado para autenticación)
- **Workspace ID de Databricks**

## 🚀 Instalación y Configuración

### 1. Clonar el Repositorio

```bash
git clone https://github.com/jordytiradotorres/adf-movie-history.git
cd adf-movie-history
```

### 2. Iniciar Sesión en Azure

# Iniciar sesión
az login

# Seleccionar suscripción
az account set --subscription "Tu-ID-Suscripción"

# Verificar suscripción actual
az account show

### 3. Crear Grupo de Recursos

# Crear grupo de recursos
az group create \
  --name "rg-movie-history" \
  --location "eastus"

### 4. Crear Azure Data Factory

# Crear Data Factory
az datafactory create \
  --resource-group "rg-movie-history" \
  --factory-name "adf-movie-history" \
  --location "eastus"

### 5. Crear Azure Data Lake Storage

# Crear cuenta de almacenamiento
az storage account create \
  --resource-group "rg-movie-history" \
  --name "adlsmoviehistory" \
  --location "eastus" \
  --sku Standard_LRS \
  --kind StorageV2 \
  --hierarchical-namespace true

# Crear contenedores
az storage fs create \
  --account-name "adlsmoviehistory" \
  --name "bronze"

az storage fs create \
  --account-name "adlsmoviehistory" \
  --name "silver"

az storage fs create \
  --account-name "adlsmoviehistory" \
  --name "gold"

### 6. Crear Databricks Workspace

# Crear workspace de Databricks
az databricks workspace create \
  --resource-group "rg-movie-history" \
  --name "dbws-movie-history" \
  --location "eastus" \
  --sku premium

### 7. Importar en Azure Data Factory

# Opción 1: Usar Azure Portal
# 1. Ve a Azure Portal → Data Factory
# 2. Abre "Author & Monitor"
# 3. Ve a "Source Control" → Git configuration
# 4. Conecta tu repositorio GitHub
# 5. Importa desde la rama main

# Opción 2: Usar Azure CLI (si tus archivos son ARM templates)
# az deployment group create \
#   --resource-group "rg-movie-history" \
#   --template-file "factory/template.json" \
#   --parameters "factory/parameters.json"

### 8. Configurar Linked Services

En Azure Portal → Data Factory → Linked services:

1. LS_Databricks

  - Tipo: Databricks
  - Workspace URL: https://your-instance.cloud.databricks.com
  - Access Token: [Tu PAT token de Databricks]

2. ls_movie_history_adls

  - Tipo: Azure Data Lake Storage Gen2
  - Account name: adlsmoviehistory
  - Authentication method: Account key / Service principal

3. Prueba conexiones antes de continuar

### 9. Crear Datasets

```json
// ds_data_history_bronze.json
{
  "name": "ds_data_history_bronze",
  "type": "AzureBlobFS",
  "linkedServiceName": "ls_movie_history_adls",
  "typeProperties": {
    "folderPath": "bronze",
    "format": "ParquetFormat"
  }
}
```

### 10. Crear Pipelines

- En Azure Data Factory Studio:

1. Ve a "Author" → "Pipelines"
2. Importa los 3 pipelines JSON desde la carpeta /pipeline
3. Configura parámetros según tu ambiente

### 🎮 Guía de Uso

- Flujo de Ejecución

```text
1. INGESTA (pl_ingest_movie_history_data)
   └─ Copia datos de fuentes → Bronze Layer
   
2. PROCESAMIENTO (pl_process_movie_history)
   └─ Ejecuta notebooks Databricks
   └─ Transforma Bronze → Silver
   
3. TRANSFORMACIÓN (pl_transformation_movie_histor...)
   └─ Crea tablas Gold
   └─ Optimiza para análisis
   
4. TRIGGER (tg_process_movie_history)
   └─ Ejecuta automáticamente según horario
```

### Ejecutar Pipelines Manualmente

# Opción 1: Azure Portal
# 1. Ve a Data Factory Studio
# 2. Selecciona el pipeline
# 3. Click "Add trigger" → "Trigger now"

# Opción 2: Azure CLI
az datafactory pipeline create-run \
  --resource-group "rg-movie-history" \
  --factory-name "adf-movie-history" \
  --name "pl_ingest_movie_history_data"

### Monitorear Ejecuciones

# Ver estado de ejecución
az datafactory pipeline-run query-by-factory \
  --resource-group "rg-movie-history" \
  --factory-name "adf-movie-history"

# Ver detalles específicos
az datafactory pipeline-run show \
  --resource-group "rg-movie-history" \
  --factory-name "adf-movie-history" \
  --run-id "your-run-id"

### 🔧 Componentes Principales

- 📦 Datasets
Define la estructura de datos en cada capa:

| Dataset | Capa | Formato | Propósito |
|---------|------|---------|-----------|
| `ds_data_history_bronze` | Bronze | Parquet/JSON | Almacenar datos crudos |

- 🔗 Linked Services
Conexiones reutilizables a servicios externos:

| Servicio | Tipo | Autenticación |
|----------|------|---------------|
| `LS_Databricks` | Databricks | Token PAT |
| `ls_movie_history_adls` | ADLS Gen2 | Key/Service Principal |

# 🚀 Pipelines (3 pipelines principales)

## 1. `pl_ingest_movie_history_data`
**Propósito:** Ingesta de datos desde fuentes

**Actividades:**
- **Copy Activity:** Copia datos → Bronze
- **Validaciones:** Verifica integridad
- **Logging:** Registra metadatos
- **Salida:** Datos raw en `bronze/` container

## 2. `pl_process_movie_history`
**Propósito:** Procesamiento con Spark en Databricks

**Actividades:**
- **Databricks Notebook Activity**
- Ejecuta notebooks de transformación
- Procesa Bronze → Silver

**Notebooks ejecutados:**
- Limpieza de datos
- Normalización de formatos
- Enriquecimiento de datos

## 3. `pl_transformation_movie_history`
**Propósito:** Transformación final para análisis

**Actividades:**
- **SQL Activity:** Crear tablas Gold
- **Spark Activity:** Agregaciones
- Data validation
- **Salida:** Tablas optimizadas en Gold layer

# ⏲️ Triggers

## `tg_process_movie_history`
**Tipo:** Schedule trigger  
**Frecuencia:** Diaria (configurable)  
**Hora:** 2:00 AM (configurable)  
**Ejecuta:** `pl_ingest_movie_history_data`  
**Reintentos:** Automático en caso de fallo

### 📊 Flujo de Datos

```yaml
Fuentes de Datos (APIs, CSV, Databases)
              ↓
    ┌─────────────────────────┐
    │   INGESTION PIPELINE    │
    │ pl_ingest_movie_...data │
    └─────────────────────────┘
              ↓
    ┌─────────────────────────┐
    │   BRONZE LAYER (ADLS)   │
    │   - Datos raw/crudos    │
    │   - Sin transformar     │
    │   - Formato parquet     │
    └─────────────────────────┘
              ↓
    ┌─────────────────────────┐
    │ PROCESSING PIPELINE     │
    │ pl_process_movie_...ry  │
    │  (Databricks Spark)     │
    └─────────────────────────┘
              ↓
    ┌─────────────────────────┐
    │   SILVER LAYER (ADLS)   │
    │   - Datos limpios       │
    │   - Validados           │
    │   - Normalizados        │
    └─────────────────────────┘
              ↓
    ┌─────────────────────────┐
    │ TRANSFORMATION PIPELINE │
    │ pl_transformation_...ry │
    │   (Agregaciones SQL)    │
    └─────────────────────────┘
              ↓
    ┌─────────────────────────┐
    │   GOLD LAYER (ADLS)     │
    │  - Datos optimizados    │
    │  - Para análisis/BI     │
    │  - Tablas indexadas     │
    └─────────────────────────┘
              ↓
    ┌─────────────────────────┐
    │  ANALYSIS & BI          │
    │  Power BI, Databricks   │
    │  SQL Server, Dashboards │
    └─────────────────────────┘
```

### ⏲️ Monitoreo y Triggers
 Monitoreo de Pipelines

# Monitorar en tiempo real
# 1. Azure Portal → Data Factory
# 2. Monitor → Pipeline runs
# 3. Ver status, duración, errores

# Alertas automáticas
# Configurar en Azure Monitor
# - Fallos de pipeline
# - Duración excesiva
# - Errores de actividad

### Trigger Programado
Configuración del trigger tg_process_movie_history:

- Ejecuta pl_ingest_movie_history_data diariamente
- Hora: 2:00 AM UTC (ajustable)
- Reintentos: Hasta 2 intentos en caso de fallo
- Timeout: 48 horas

### Para modificar trigger:

# Ver configuración actual
az datafactory trigger show \
  --resource-group "rg-movie-history" \
  --factory-name "adf-movie-history" \
  --trigger-name "tg_process_movie_history"

### 📚 Configuración de Variables de Entorno
Crea un archivo .env (no commitear):

AZURE_SUBSCRIPTION_ID=xxx
AZURE_RESOURCE_GROUP=rg-movie-history
AZURE_DATA_FACTORY=adf-movie-history
DATABRICKS_WORKSPACE_URL=https://xxx.cloud.databricks.com
DATABRICKS_TOKEN=xxx
ADLS_ACCOUNT_NAME=adlsmoviehistory
ADLS_CONTAINER_NAME=bronze

### 🔒 Seguridad
Mejores Prácticas Implementadas

✅ Credenciales en Azure Key Vault
✅ Managed Identity para autenticación
✅ Encriptación en tránsito (HTTPS/TLS)
✅ Encriptación en reposo (Storage encryption)
✅ Control de acceso basado en roles (RBAC)
✅ Auditoría y logging habilitados

### Configurar Key Vault

# Crear Key Vault
az keyvault create \
  --resource-group "rg-movie-history" \
  --name "kv-movie-history"

# Agregar secreto
az keyvault secret set \
  --vault-name "kv-movie-history" \
  --name "databricks-token" \
  --value "your-token"

# Usar en ADF: @secretResourceName('kv-movie-history', 'databricks-token')

### 📈 Monitoreo y Alertas
Configura alertas en Azure Monitor:

# Crear alerta para fallos de pipeline
az monitor metrics alert create \
  --resource-group "rg-movie-history" \
  --name "adf-pipeline-failure-alert" \
  --scopes "/subscriptions/.../resourceGroups/rg-movie-history/providers/Microsoft.DataFactory/factories/adf-movie-history" \
  --condition "PipelineFailedRuns > 0" \
  --window-size "PT5M" \
  --evaluation-frequency "PT1M"
