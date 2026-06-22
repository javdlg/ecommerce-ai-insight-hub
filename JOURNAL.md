# 📓 Bitácora de Desarrollo: E-Commerce AI Insight Hub

Este documento sirve como registro de progreso y guía de ejecución técnica por fases. Está diseñado para orientar la implementación modular de la solución, garantizando que cada componente (Ingeniería de Datos, Infraestructura, IA y Orquestación) cumpla con estándares de nivel empresarial.

---

## 📌 Fase 1: Ingeniería de Datos, Extracción y Modelado (Local)
**Objetivo:** Preparar, limpiar y estructurar el dataset de Olist (Kaggle) mediante un pipeline robusto antes de su migración a la nube.

- [ ] **Hito 1.1: Análisis Exploratorio de Datos (EDA) y Calidad de Datos**
  - **Instrucciones para el Agente:** Crear un script de inspección para identificar valores nulos, inconsistencias en tipos de datos y claves duplicadas en las tablas principales (`orders`, `items`, `products`, `reviews`).
  - **Criterio de Aceptación:** Reporte automatizado de calidad de datos que asegure la integridad referencial entre pedidos y reseñas.
- [ ] **Hito 1.2: Normalización y Arquitectura de Capas**
  - **Instrucciones para el Agente:** Diseñar un script de transformación que limpie y normalice los archivos de texto de las reseñas (remoción de caracteres especiales, manejo de codificación UTF-8). Implementar una estrategia de almacenamiento intermedio eficiente (ej. formato estructurado optimizado como Parquet o particionado local) para simular una arquitectura de datos empresarial (Capas Bronze y Silver).
  - **Criterio de Aceptación:** Datos limpios, tipados correctamente y listos para la carga relacional.

---

## 📌 Fase 2: Infraestructura como Código (IaC) y Base de Datos en OCI
**Objetivo:** Desplegar de forma automatizada y segura los recursos en Oracle Cloud Infrastructure.

- [ ] **Hito 2.1: Definición de Manifiestos de Terraform**
  - **Instrucciones para el Agente:** Escribir los archivos de configuración de Terraform (`main.tf`, `variables.tf`, `outputs.tf`) para aprovisionar una instancia de Oracle Autonomous Database en OCI, asegurando las políticas de acceso y la configuración de red (VCN).
  - **Criterio de Aceptación:** Despliegue e infraestructura aprovisionada en OCI mediante `terraform apply` sin errores de configuración.
- [ ] **Hito 2.2: Pipeline de Carga y Conectividad Segura**
  - **Instrucciones para el Agente:** Desarrollar el módulo `oci_db_setup.py` que utilice la Wallet de seguridad de Oracle o drivers nativos para conectarse a la base de datos en la nube. Configurar la creación del esquema relacional optimizado y realizar la inserción masiva (*bulk insert*) de los datos procesados en la Fase 1.
  - **Criterio de Aceptación:** Tablas creadas con restricciones de llave primaria/foránea en OCI y datos completamente poblados.

---

## 📌 Fase 3: Pipeline de Procesamiento de Lenguaje Natural (NLP) y RAG
**Objetivo:** Construir el motor de búsqueda semántica y contextualización para el análisis cualitativo de las opiniones de los clientes.

- [ ] **Hito 3.1: Tokenización, Chunking y Generación de Embeddings**
  - **Instrucciones para el Agente:** Crear un pipeline utilizando LangChain para segmentar los textos de las reseñas (`review_comment_message`). Configurar la integración con Gemini para generar representaciones vectoriales (embeddings) de alta calidad en español/portugués.
  - **Criterio de Aceptación:** Almacenamiento eficiente de vectores emparejados con los metadatos del producto y del pedido para permitir consultas cruzadas.
- [ ] **Hito 3.2: Motor de Recuperación Contextual (Retrieval)**
  - **Instrucciones para el Agente:** Implementar la lógica de búsqueda de similitud de cosenos para recuperar las quejas o comentarios más relevantes dado un producto específico o una categoría de insatisfacción (logística, calidad, precio).
  - **Criterio de Aceptación:** Una función que reciba un ID de producto y devuelva un contexto consolidado de sus reseñas más críticas.

---

## 📌 Fase 4: Desarrollo de Agentes y Orquestación Cíclica con LangGraph
**Objetivo:** Implementar la lógica de negocio inteligente mediante un sistema multi-agente capaz de razonar, consultar bases de datos y corregir sus propios errores.

- [ ] **Hito 4.1: Definición del Estado del Grafo (`AgentState`)**
  - **Instrucciones para el Agente:** Modelar el estado global utilizando Pydantic para almacenar la consulta del usuario, las consultas SQL generadas, los registros recuperados de la base de datos, el contexto del RAG y el reporte final de negocio.
- [ ] **Hito 4.2: Implementación del Agente Analítico (Text-to-SQL)**
  - **Instrucciones para el Agente:** Desarrollar el nodo del Agente de Datos. Utilizando LangChain, configurar el LLM para escribir consultas SQL basadas en lenguaje natural, incorporando mecanismos de validación interna (si la consulta falla, el agente lee el error de ejecución de la base de datos y autocorrige la sintaxis o cláusulas como `HAVING` o `GROUP BY`).
- [ ] **Hito 4.3: Implementación del Agente de Reseñas (RAG Specialist)**
  - **Instrucciones para el Agente:** Desarrollar el nodo del Agente de NLP que consuma el motor de recuperación de la Fase 3 para extraer patrones cualitativos de insatisfacción.
- [ ] **Hito 4.4: Enrutador Condicional e Integración del Grafo**
  - **Instrucciones para el Agente:** Construir el flujo lógico en `workflow.py` con LangGraph. Definir un nodo supervisor que analice la pregunta inicial del usuario y decida mediante aristas condicionales (*conditional edges*) si se requiere análisis cuantitativo (SQL), cualitativo (RAG) o ambos de forma paralela.
  - **Criterio de Aceptación:** Flujo completo que reciba una pregunta compleja de negocio, invoque los nodos correctos y unifique las respuestas en un único reporte estructurado.

---

## 📌 Fase 5: Implementación de la Aplicación y Despliegue en la Nube
**Objetivo:** Llevar la aplicación orquestada a un entorno de ejecución estable en OCI.

- [ ] **Hito 5.1: Interfaz de Consola / API de Servicio**
  - **Instrucciones para el Agente:** Crear el punto de entrada de la aplicación (`main.tf` / `main.py`) para interactuar con el grafo a través de una interfaz limpia, ideal para ser consumida posteriormente por un frontend o un dashboard ejecutivo.
- [ ] **Hito 5.2: Despliegue en OCI Compute**
  - **Instrucciones para el Agente:** Configurar el entorno en una instancia de cómputo de OCI, automatizando el manejo de variables de entorno sensibles de forma segura mediante políticas de Oracle Cloud.
  - **Criterio de Aceptación:** Aplicación ejecutándose de extremo a extremo en la nube, procesando consultas de negocio y devolviendo análisis integrados basados en datos reales de Olist.