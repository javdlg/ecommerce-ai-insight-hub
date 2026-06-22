# 📊 E-Commerce AI Insight Hub: Orquestación Multi-Agente para Decisiones de Negocio

## 📝 Descripción del Proyecto
E-Commerce AI Insight Hub es una solución empresarial inteligente diseñada para automatizar la extracción de *insights* a partir de datos transaccionales y retroalimentación de clientes. Utilizando una arquitectura multi-agente orquestada con **LangGraph** y **Gemini**, el sistema es capaz de analizar ventas, consultar bases de datos en tiempo real mediante SQL automatizado, y aplicar técnicas de RAG avanzado sobre reseñas de clientes para identificar cuellos de botella en la logística y el nivel de satisfacción.

Este proyecto fue desarrollado como parte del desafío ONE AI for TECH de Oracle y Alura Latam, enfocándose en la aplicación práctica de IA Generativa en entornos corporativos.

## 🎯 Caso de Uso (Business Value)
Los analistas de negocio suelen perder horas cruzando datos transaccionales con quejas cualitativas de clientes. Este sistema permite a un gerente hacer preguntas en lenguaje natural como:
> *"¿Cuáles fueron los productos con mayores caídas de ventas en el último trimestre y qué dicen las reseñas negativas sobre ellos?"*

El sistema enruta la consulta a dos agentes especializados (Data Analyst y NLP Specialist), sintetiza los hallazgos y presenta un reporte accionable.

## 🗄️ Dataset Utilizado
**Brazilian E-Commerce Public Dataset by Olist (Kaggle)**
Este dataset es ideal para el ecosistema de Data Science corporativo. Contiene información real y anonimizada de más de 100,000 pedidos, incluyendo estado del envío, dimensiones del producto, ubicación del cliente y reseñas de texto (ideal para el análisis de sentimiento y RAG).
*Enlace: [Kaggle - Olist Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)*

## 🛠️ Arquitectura y Tecnologías (Stack)
* **LLM Core:** Gemini API (Google).
* **Orquestación:** LangChain & LangGraph (Manejo de estados, ruteo condicional y multi-agente). Reemplaza herramientas No-Code para un control programático total.
* **Agente 1 (SQL Data Analyst):** LangChain Data Analysis. Convierte NLP a consultas SQL para extraer métricas de ventas.
* **Agente 2 (RAG & NLP):** Búsqueda semántica sobre las reseñas de clientes para clasificar quejas y extraer tópicos de insatisfacción.
* **Infraestructura (OCI):** * **Base de Datos:** Oracle Cloud Autonomous Database.
  * **Despliegue:** Implementación de la aplicación en OCI (usando contenedores/Compute Instances).
  * **IaC:** Terraform para aprovisionar la infraestructura en Oracle Cloud.

## 📂 Estructura del Proyecto
```text
├── data/
│   ├── raw/                 # Archivos CSV originales de Olist
│   └── processed/           # Datos limpios y normalizados (Parquet)
├── infrastructure/
│   └── terraform/           # Scripts de IaC para OCI
├── src/
│   ├── agents/
│   │   ├── sql_agent.py     # Agente LangChain para consultas a BD
│   │   └── rag_agent.py     # Agente para análisis de reseñas (VectorStore)
│   ├── graph/
│   │   └── workflow.py      # Orquestación de LangGraph (nodos y edges)
│   ├── database/
│   │   └── oci_db_setup.py  # Conexión y carga inicial en Oracle DB
│   └── main.py              # Punto de entrada de la aplicación
├── requirements.txt
└── README.md