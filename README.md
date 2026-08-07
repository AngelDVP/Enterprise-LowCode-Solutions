# Geovisión: Sistema de Geolocalización y Automatización de Clientes

Solución end-to-end de transformación digital diseñada para optimizar la planificación territorial, asegurar la calidad de datos en terreno (Data Quality) y automatizar el flujo de creación de clientes para una red de distribución de consumo masivo con más de 19.000 clientes activos.

El proyecto integra análisis geoespacial en QGIS, desarrollo low-code en Power Apps, flujos de integración en Power Automate y almacenamiento estructurado en Dataverse.

---

## Estructura del Repositorio

* **Resumen_Ejecutivo_MVP_GEOVISION.md**: Documento de análisis estratégico, comparativa de costos de licenciamiento (In-house vs. Externo) y cronograma de implementación de la solución.
* **Geovision_Client_Creation_App.zip**: Paquete de exportación de la aplicación en Power Apps (Canvas App) que incluye el diseño de pantallas, variables locales y la lógica de validación.

---

## Problema de Negocio

La operación comercial en terreno presentaba ineficiencias críticas debido a:
* **Fragilidad del dato manual**: Errores humanos de tipeo en direcciones y coordenadas falsas ingresadas por la fuerza de venta.
* **Impacto logístico (Efecto Dominó)**: Coordenadas de entrega erróneas que causaban fallas en la planificación de rutas de despacho, aumentando el consumo de combustible de la flota y generando devoluciones.
* **Falta de auditoría**: Imposibilidad de verificar si el vendedor realmente visitaba físicamente al cliente al momento de registrarlo.

---

## Solución Arquitectónica

### 1. Aplicación Móvil (Power Apps & Dataverse)
* **Captura GPS Obligatoria**: Bloqueo de registro que fuerza la obtención de las coordenadas GPS exactas en terreno.
* **Soporte Offline-First**: Almacenamiento local temporal para asegurar el registro en zonas rurales o de baja conectividad.
* **Consumo Seguro**: Conexión nativa a tablas relacionales de Dataverse como almacenamiento intermedio antes de inyectar los datos al ERP (QAD/SOM).

### 2. Integración y Lógica (Power Automate)
* **Validación en Tiempo Real**: Flujos automáticos que evalúan banderas de duplicados de clientes (mediante RUT) antes de autorizar la creación.
* **Procesamiento Documental con IA (OpenAI API)**: Integración con modelos de visión para la extracción y validación automática de datos desde documentos contables de los clientes, reduciendo el ciclo de procesamiento de **3 días a 20 minutos**.

### 3. Modelamiento Geoespacial (QGIS)
* **Optimización Territorial**: Agrupamiento espacial de 19.317 clientes activos en 90 zonas de venta balanceadas, disminuyendo los tiempos de traslado del equipo comercial en terreno.

---

## Cómo importar la aplicación

1. Descarga el archivo `Geovision_Client_Creation_App.zip` de este repositorio.
2. Accede a tu entorno de Microsoft Power Apps.
3. En el panel izquierdo, selecciona **Aplicaciones** y haz clic en **Importar aplicación de lienzo**.
4. Sube el archivo ZIP y configura las conexiones requeridas (Dataverse y conectores de flujo).
