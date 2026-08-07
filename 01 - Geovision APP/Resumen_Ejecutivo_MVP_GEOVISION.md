# Resumen Ejecutivo: Proyecto Geovisión
**Industria de Consumo Masivo S.A. — Versión MVP — Por Angel Villalobos Paz**

## 1. Diagnóstico

*   **Calidad de Datos Deficiente (Data Quality):** El ecosistema actual no garantiza la calidad del dato de entrada al sistema central.
*   **Efecto Dominó Operativo:** Si el vendedor comete un error en el ingreso de datos en terreno, toda la cadena de valor interna falla, propagando el error hasta el proceso de despacho logístico.
*   **Pocas Herramientas de Auditoría:** Imposibilidad de validar de forma georeferenciada si el vendedor realmente visitó el local al momento de la creación de la ficha de cliente.
*   **Fragilidad del Registro Manual:** Ingreso propenso a errores humanos (errores de tipeo, direcciones falsas, datos tributarios incorrectos).
*   **Desarticulación de Rutas:** Alta ineficiencia logística en la planificación de despachos debido a coordenadas de entrega erróneas, incrementando el gasto operativo de la flota.

---

## 2. Solución Tecnológica Propuesta

*   **GPS Obligatorio y Auditable (Offline-First):** Bloqueo de la aplicación móvil que fuerza la identificación GPS del sitio exacto del cliente. Cuenta con funcionamiento fuera de línea (offline) para asegurar la continuidad de la operación en zonas con problemas de señal.
*   **Auditoría de Datos Integrada con IA:** Implementación de modelos de procesamiento de lenguaje y visión en terreno para auditar la información ingresada en tiempo real, cruzando y validando los datos en segundos.
*   **Sincronización Directa vía API (SOM/ERP):** Integración vía API que se complementa con los sistemas actuales, inyectando clientes saneados y validados directamente al inicio de la cadena de operación.

---

## 3. Costos y Comparativa Estratégica
*Pronóstico proyectado a 1 año, estimado para 81 usuarios.*

| Criterio de Comparación | Adopción Externa (Actual) | Solución In-House (Propuesta) |
| :--- | :--- | :--- |
| **Costo de Implementación** | $13.213 USD | $0 USD |
| **Costo Anual de Licencias** | $77.760 USD | $4.860 USD (Licencias CSP + $600 IA)<br>o $9.720 USD (Licencias PAYG + $600 IA) |
| **Costos Humanos (Anual)** | 240 horas (~$1.500 USD) | 41 horas (~$240 USD) |
| **Tiempo de Lanzamiento** | > 8 Meses | 6 a 8 Semanas |

---

## 4. Cronograma "Fast-Track" de Implementación

*   **Fase 1 (Semanas 1-2):** Arquitectura de base de datos, habilitación de soporte offline y validación de GPS obligatorio en terreno.
*   **Fase 2 (Semanas 3-4):** Integración bidireccional, desarrollo del motor de IA y conexión con el sistema central (SOM).
*   **Fase 3 (Semanas 5-6):** Lead Scoring automático con IA y automatización de flujos "Zero Touch".
*   **Fase 4 (Semanas 7-8):** Capacitaciones de usuarios finales, despliegue supervisado y monitoreo de métricas de calidad de datos.
