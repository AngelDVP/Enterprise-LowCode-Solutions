# 📊 Propuesta de Proyecto: Sistema de Rendición de Gastos Digital
**Industria de Alimentos Trendy S.A.**
*Versión borrador — Marzo 2026*

---

## Resumen Ejecutivo

El proceso actual de rendición de gastos involucra formularios físicos, boletas en papel, y múltiples intermediarios que generan retrasos y errores. Este proyecto propone digitalizar el flujo completo mediante una aplicación Power Apps con inteligencia artificial, reduciendo el tiempo de procesamiento estimado en un **70%** y eliminando el ingreso manual de datos por parte de los validadores.

---

## Problema Actual

```
Supervisor → Formulario Excel → Email → Angélica → 
  Compara físico → Yessenia → Ingresa 1x1 en SignRequest → 
    Gerente firma → Don Javier
```

| Problema | Impacto |
|---|---|
| Boletas físicas enviadas por separado | Riesgo de pérdida, desorden |
| Angélica compara manualmente boleta por boleta | Alto tiempo invertido |
| Yessenia ingresa dato a dato en SignRequest | Trabajo 100% evitable |
| Sin trazabilidad ni historial digital | Auditoría difícil |
| Sin ID único por rendición | Sin control de seguimiento |

---

## Solución Propuesta

### Stack Tecnológico
| Componente | Tecnología | Por qué |
|---|---|---|
| App móvil/web | **Power Apps Canvas** | Ya tienen licencias, app-per-app |
| Base de datos | **Dataverse** | Relaciones, roles, auditoría nativa |
| Automatizaciones | **Power Automate** | Conector nativo SignRequest + OpenAI |
| Extracción IA boletas | **OpenAI GPT-4o Vision** | Lee cualquier boleta sin entrenamiento previo |
| Firma digital | **SignRequest** (conector Premium) | Ya lo usan, tiene API completa |
| Notificaciones | **Microsoft Teams + Email** | Sin costo adicional |

---

## Arquitectura del Sistema

```
┌─────────────────────────────────────────────────────┐
│                  POWER APPS                          │
│  ┌──────────────┐      ┌──────────────────────────┐ │
│  │Vista Digitador│      │   Vista Validador         │ │
│  │(Supervisor)   │      │   (Angélica)              │ │
│  └──────┬───────┘      └───────────┬──────────────┘ │
└─────────┼─────────────────────────┼────────────────┘
          │                         │
          ▼                         ▼
┌─────────────────────────────────────────────────────┐
│                   DATAVERSE                          │
│  Rendicion ←── Detalle_Rendicion ←── Foto_Boleta    │
│  (cabecera)       (líneas)           (adjuntos)      │
└──────────────────────┬──────────────────────────────┘
                       │
          ┌────────────▼────────────┐
          │     POWER AUTOMATE      │
          │  Flow 1: IA procesa     │─── OpenAI GPT-4o Vision
          │  Flow 2: Envío firma    │─── SignRequest (envío automático)
          │  Flow 3: Cierre         │─── Notif. Don Javier
          └─────────────────────────┘
```

---

## Modelo de Datos (Dataverse)

### Tabla: Rendicion
| Campo | Tipo |
|---|---|
| Folio | Autonumérico (REN-2026-001) |
| Supervisor | Lookup Usuario |
| Centro de Costo | Choice |
| Fecha Envío | Fecha/Hora |
| Estado | Choice (Enviado / Revisión IA / Discrepancia / Aprobado / Rechazado / Cerrado) |
| Tipo | Choice (Rendición Fondo / Reembolso / Fondo Fijo) |
| Banco / Cuenta | Texto |
| Comentario Aprobador | Texto |

### Tabla: Detalle_Rendicion *(relacionada a Rendicion)*
| Campo | Tipo |
|---|---|
| Tipo de Gasto | Choice |
| Descripción | Texto |
| N° Boleta | Texto (valida duplicados) |
| Fecha Boleta | Fecha (valida antigüedad) |
| Monto IA | Moneda |
| Monto Declarado | Moneda (Editable) |
| Editado Manualmente | Sí/No (Alerta para auditoría) |
| Latitud / Longitud | Texto/Decimal (Geolocalización In-App) |
| Estado Línea | Choice (OK / Discrepancia) |
| Foto Boleta | Adjunto/Blob |

---

## Roles de Ingreso de Datos

### 🤖 1. Extracción Automática (IA / OpenAI)
*El supervisor solo saca la foto. Los siguientes datos los rellena la IA sin intervención humana:*

| Campo | Fuente IA | Observación |
|---|---|---|
| N° Boleta / Documento | OpenAI GPT-4o | N° de folio, comprobante o código único |
| Fecha del Gasto | OpenAI GPT-4o | Formateada DD-MM-YYYY |
| Monto Total | OpenAI GPT-4o | Total final con IVA incluido |
| Nombre / RUT del Comercio | OpenAI GPT-4o | Campo de seguridad antifraude |

---

### 🔄 2. Cruce de Información Automático (App / Dataverse)
*Datos que la App obtiene sola, sin que el supervisor los escriba:*

| Campo | Fuente | Observación |
|---|---|---|
| Nombre del Supervisor | Usuario logueado Office 365 | `User().FullName` |
| RUT del Supervisor | Tabla de empleados en Dataverse | Cruce por email del usuario logueado |
| Cargo del Supervisor | Tabla de empleados en Dataverse | Cruce por email del usuario logueado |
| Datos Bancarios | Tabla de empleados en Dataverse | Banco, tipo de cuenta, N° cuenta |
| Centro de Costo | Tabla de empleados en Dataverse | ⚠️ **Validar:** ¿Cada supervisor tiene un único CC, o puede rendir en múltiples? Si es múltiple, mostrar desplegable pre-filtrado por su perfil |

---

### ✍️ 3. Ingreso Manual (Supervisor en Power Apps)
*Solo estos campos los escribe el supervisor:*

| Campo | Tipo Control | Observación |
|---|---|---|
| Tipo de Gasto | Desplegable (Choice) | Nombre amigable → guarda código contable. Ej: `Peajes` = `51809500` |
| Descripción / Motivo | Texto libre | Breve justificación del gasto |
| Kilometraje recorrido | Número (Opcional) | Solo aplica para gastos de movilización |

---

## Estrategia de Calidad de Imagen (Producción Móvil)

Cuando los supervisores usen su celular en terreno, existen 3 capas de defensa para evitar fotos ilegibles:

| Capa | Descripción | Costo |
|---|---|---|
| **Capa 1 – UX** | Pantalla de instrucciones antes de abrir la cámara: *"Apoya el ticket en superficie plana. Espera que el texto se vea nítido antes de fotografiar."* | $0 |
| **Capa 2 – Feedback Instantáneo** | Si la IA devuelve `S/N` en más de 2 campos, la App muestra alerta ⚠️ *"Imagen ilegible, reintenta la foto"* sin guardar el registro. | $0 |
| **Capa 3 – Validación Azure** | Paso previo en Power Automate usando **Azure AI Vision** para evaluar nitidez de la imagen (`imageQuality score`). Si el score es < 0.6, el flujo responde con error sin consumir tokens de OpenAI. | Centavos de crédito Azure |

> **Para el MVP:** Implementar Capa 1 + Capa 2. La Capa 3 se agrega en producción.

---

## Pantallas de la App

### 👷 Vista Digitador (Supervisor)

**Pantalla 1 – Nueva Rendición**
- Tipo de rendición (radio button)
- Centro de costo destino
- Banco y cuenta corriente
- Botón: "Agregar Boleta"

**Pantalla 2 – Carga de Boleta (Extracción IA On-the-fly)**
- 📷 **Cámara en vivo (Galería bloqueada en móviles para evitar fraudes)**
- Captura oculta de **Geolocalización (GPS)**
- Llamada instantánea a **OpenAI (detail: high)** (Si es PDF, requiere conversión previa a imagen)
- Campos autocompletados por IA (TextInputs editables):
  - Tipo de Gasto
  - Descripción del gasto
  - N° Boleta o Factura (con validación de duplicados)
  - Fecha de la boleta (con validación de vigencia)
  - Total declarado ($)
  - *(Si el usuario modifica los datos de la IA, se marca `Editado_Manualmente = true`)*
- Botón: "Confirmar y Agregar otra" / "Ver resumen"

**Pantalla 3 – Resumen antes de enviar**
- Lista de todas las boletas cargadas
- Total general
- Botón: "Enviar Rendición" → genera Folio + dispara IA

---

### 🔍 Vista Validador (Angélica)

**Pantalla 1 – Bandeja de entrada**
- 🔴 Badge con pendientes
- Filtros: fecha / supervisor / CC / estado
- Lista de rendiciones con Folio, Supervisor, Total, Estado
- Indicador: ✅ OK / ⚠️ Discrepancia / 🕐 Procesando IA

**Pantalla 2 – Detalle de Rendición**
- Encabezado: Folio, Supervisor, Fecha, CC
- Por cada línea: vista **lado a lado**

| Datos Finales (Declarados) | Alertas de Auditoría |
|---|---|
| Tipo de gasto | 📍 Match GPS: ¿Coherente? |
| Descripción | 📝 ¿Editado Manualmente?: Sí/No |
| N° Boleta | 🛑 Posible duplicado: Sí/No |
| Fecha | Comercio devuelto por IA |
| **Monto final** | **Monto sugerido IA** |
| — | Estado: ✅ Limpio / ⚠️ Discrepancia |

- Botón: "Ver foto boleta" (abre imagen original)
- Botón por línea: "Marcar revisada manualmente"
- Botón final: "Aprobar Rendición" → dispara Flow 2 (SignRequest)
- Botón: "Rechazar con comentario"

---

## Flujo Automatizado

```
[Supervisor saca foto de boleta (Pantalla 2)]
       │
       ▼
Flow 1 – Extracción IA (On-the-fly)
  → Llama a IA (OpenAI con detail:high)
  → Devuelve datos a la App para autocompletar y revisión del usuario
       │
       ▼
[Supervisor revisa, corrige y presiona Enviar (Pantalla 3)]
       │
       ▼
Flow 1.1 – Procesamiento y Reglas de Negocio
  → Guarda transacción en Dataverse
  → Evalúa banderas de fraude (GPS, Duplicados, Fechas)
  → Si "Editado_Manualmente" es true: marca como "Discrepancia / ⚠️"
  → Notifica a Angélica (Teams/Email)
       │
       ▼
[Angélica revisa y aprueba]
       │
       ▼
Flow 2 – Firma Digital
  → Genera PDF resumen de rendición
  → Envía automáticamente a SignRequest con el gerente indicado
  → Angélica NO necesita abrir SignRequest manualmente
       │
       ▼
[Gerente firma en SignRequest]  ← recibe email, no necesita la app
       │
       ▼
Flow 3 – Cierre
  → Estado → "Cerrado"
  → Notifica a Don Javier con PDF firmado adjunto
  → Archivo histórico en Dataverse
```

---

## Carta Gantt

```
FASE 1 – VALIDACIÓN TÉCNICA (Semana 1-2)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ W1 ][ W2 ]
[████]        Verificar conector SignRequest en entorno Dev
[████]        Prueba OpenAI GPT-4o Vision con boleta real
      [████]  Crear modelo Dataverse (tablas y relaciones)
      [████]  Flujo básico: foto → OpenAI → resultado pantalla

FASE 2 – DESARROLLO CORE (Semana 3-5)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ W3 ][ W4 ][ W5 ]
[████]              Vista Digitador (3 pantallas)
      [████]        Vista Validador (2 pantallas)
      [████][████]  Power Automate: Flows 1, 2 y 3
            [████]  Integración SignRequest real

FASE 3 – PRUEBAS Y AJUSTE (Semana 6-7)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ W6 ][ W7 ]
[████]        Prueba piloto con Antonio y Angélica
[████]        Ajustes según feedback
      [████]  Prueba de firma con gerente real (SignRequest)
      [████]  Revisión de seguridad y permisos

FASE 4 – DESPLIEGUE (Semana 8)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[ W8 ]
[████]  Publicación app producción
[████]  Capacitación usuarios (Antonio, Angélica, Yessenia)
[████]  Documentación y entrega
```

---

## Plan Prototipo Rápido (esta semana)

Para validar que los conectores funcionan antes de comprometerse con el desarrollo completo:

1. **[ ]** Crear flow en Power Automate con conector SignRequest → enviar documento de prueba
2. **[ ]** Crear flow con HTTP + OpenAI API → mandar foto de boleta → ver qué extrae
3. **[ ]** Pantalla mínima en Power Apps: cámara → foto → botón enviar → ver resultado IA

> Con eso en 1-2 días tienes evidencia concreta de que la solución funciona antes de presentar.

---

## Beneficios Esperados

| Indicador | Antes | Después |
|---|---|---|
| Tiempo Angélica por rendición | ~40 min | ~5 min (solo discrepancias) |
| Tiempo Yessenia en SignRequest | ~15 min | 0 min (automático) |
| Riesgo pérdida boletas físicas | Alto | Eliminado |
| Trazabilidad / auditoría | Manual | 100% digital |
| Notificaciones | Cadena de emails | Automáticas por estado |

---

*Preparado por: [Tu nombre] | Marzo 2026*
