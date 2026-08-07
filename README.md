# Enterprise Power Platform & BI Solutions

Repositorio de soluciones de transformación digital, optimización de procesos de negocio y análisis de datos en entornos corporativos de consumo masivo.

Este portafolio contiene la arquitectura técnica, roadmaps de implementación y prototipos funcionales desarrollados con Microsoft Power Platform, integración de APIs y modelamiento espacial.

---

## Estructura del Repositorio

### 01 - Geovision APP
Sistema de geolocalización y auditoría en terreno para el registro de clientes. Diseñado para optimizar la planificación de rutas y asegurar la calidad de datos (Data Quality) sobre una red de +19.000 clientes activos.
* **Geovision_Client_Creation_App.zip**: Paquete de exportación de la aplicación en Power Apps (Canvas App) listo para importar.
* **Resumen_Ejecutivo_MVP_GEOVISION.md**: Documento estratégico de diagnóstico del negocio, comparativa de costos de licenciamiento (In-house vs. Externo) y cronograma del proyecto.
* **GEOVISION - TRANSFORMACION DIGITAL.pptx**: Presentación ejecutiva del proyecto, arquitectura técnica del sistema y propuesta comercial.

### 02 - Rendiciones
Sistema automatizado de rendición de gastos digitales integrado con inteligencia artificial para la lectura y procesamiento de documentos contables.
* **propuesta_rendiciones.md**: Documento de respaldo técnico que detalla la arquitectura de integración entre Power Apps, Power Automate y la API de OpenAI (GPT Vision), incluyendo fórmulas de Power Fx y payloads JSON.

---

## Proyectos Destacados

### 1. Proyecto Geovisión
* **Desafío**: Pérdida de eficiencia logística por coordenadas de entrega erróneas en el ERP, falta de herramientas de auditoría en terreno y registro propenso a errores humanos.
* **Solución**: Aplicación móvil con captura GPS obligatoria y funcionamiento offline-first, conectada a Dataverse para saneamiento previo de datos de clientes antes de la inyección al ERP.
* **Impacto**: Optimización geoespacial en QGIS agrupando a 19.317 clientes en 90 zonas de venta equilibradas, reduciendo tiempos de traslado.

### 2. Prototipo de Rendición de Gastos con IA
* **Desafío**: Procesamiento manual de boletas físicas de rendición de gastos que demoraba un promedio de 3 días hábiles.
* **Solución**: Flujo automatizado que captura la imagen del documento en Power Apps, extrae los datos clave (Monto, Fecha, Comercio, Folio) mediante una solicitud HTTP a la API de OpenAI (GPT Vision) en Power Automate, y registra la transacción auditada en Dataverse.
* **Impacto**: Reducción del ciclo de procesamiento contable de **3 días a 20 minutos** por rendición.

---

## Cómo importar las aplicaciones

### Power Apps (.zip)
1. Descarga el archivo `.zip` correspondiente de la carpeta `01 - Geovision APP`.
2. Accede a tu entorno de Microsoft Power Apps.
3. Selecciona **Aplicaciones** en el menú izquierdo y haz clic en **Importar aplicación de lienzo**.
4. Sube el paquete ZIP y configura las conexiones (Dataverse y Power Automate).
