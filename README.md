# 📚 API de Tacita de Plata - Documentación

## 📋 Descripción General

La API de Tacita de Plata proporciona acceso a información completa sobre contratos del programa "Tacita de Plata" de la Alcaldía de Medellín. Este endpoint está diseñado específicamente para integraciones con Power BI y otras herramientas de análisis de datos.

Además de los contratos, el endpoint entrega dos conjuntos de datos adicionales — **presupuesto** y **empleabilidad** — que no tienen relación (FK) con los contratos (su granularidad es distinta: proyecto+año y contrato-SAP+mes), por lo que se devuelven siempre completos, sin paginar, en arrays independientes.

## ℹ️ Información Base

- 🌐 **URL_BASE**: `https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net`
- 🔗 **URL_COMPLETA**: `{url_base}/api/power-bi/tacita-plata`
- 📡 **Método HTTP**: `GET`
- 🔐 **Tipo de Autenticación**: API Key
- 📄 **Formato de Respuesta**: JSON

---

## 🔐 Autenticación

El endpoint requiere autenticación mediante API Key. La API Key puede proporcionarse de dos formas:

### ✅ Opción 1: Header HTTP (Recomendado)

```http
GET url_base/api/power-bi/tacita-plata
X-API-Key: tu_api_key_aqui
```

### 🔄 Opción 2: Query Parameter

```http
GET url_base/api/power-bi/tacita-plata?apikey=tu_api_key_aqui
```

### ⚙️ Configuración de API Key

La API Key esperada se configura mediante la variable de entorno:
- 🔑 **Variable de entorno**: `TACITA_PLATA_API_KEY`
- ⚠️ **Sin valor por defecto**: si la variable no está configurada en el servidor, el endpoint falla al arrancar (no hay clave de respaldo tipo `CLAVE_SECRETA`)



## 🚀 Endpoint Principal

### 📊 Obtener Contratos Completos

```http
GET https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata
```

Retorna información completa de los contratos del programa Tacita de Plata (con su ubicación geográfica y su actividad asociada), más los arrays completos de presupuesto y empleabilidad.

#### 🔍 Parámetros de Consulta (Query Parameters)

| Parámetro | Tipo | Requerido | Descripción | Valor por defecto |
|-----------|------|-----------|-------------|-------------------|
| `apikey` | string | No* | API Key para autenticación (alternativa al header) | - |
| `page` | number | No | Número de página — solo afecta a `contratos` | `1` |
| `limit` | number | No | Cantidad de contratos por página (máximo 10,000) — solo afecta a `contratos` | `100` |
| `dependencia` | number | No | ID de la secretaría/dependencia — filtra `contratos`, `presupuesto` y `empleabilidad` | - |
| `comuna` | number | No | ID de la comuna/corregimiento — solo afecta a `contratos` | - |
| `cud` | string | No | Código Único de dependencia (ej: SIF1, SIF2) — solo afecta a `contratos` | - |

*Requerido si no se envía en el header `X-API-Key`

> ⚠️ **El filtro `proyecto` fue retirado.** Ya no existe como query param — presupuesto ahora se identifica por su propio catálogo de proyectos (`numero_proyecto`), no por el antiguo ID de proyecto estratégico.

#### 💡 Ejemplo de Solicitud

```bash
# Con API Key en header
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=1&limit=50" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Con API Key en query parameter
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?apikey=tacita_plata_visor_estrategico_2024&page=1&limit=50"

# Con filtros
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=1&limit=100&comuna=3&dependencia=6" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Filtrar por CUD específico
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?cud=SIF1" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```

## 📦 Estructura de Respuesta

### ✅ Respuesta Exitosa (200 OK)

```json
{
  "metadata": {
    "lastUpdated": "2026-09-01T10:30:00.000Z",
    "totalRecords": 250,
    "page": 1,
    "limit": 100,
    "pages": 3
  },
  "contratos": [
    {
      "id": 1,
      "cud": "SIF1",
      "fecha_actualizacion_reporte": "2025-01-15T00:00:00.000Z",
      "secretaria": "Secretaría de Infraestructura Física",
      "vigencia_contrato": 2025,
      "numero_contrato": "CO-2025-001",
      "objeto": "Construcción de vía peatonal",
      "identificador_simple": "A-001",
      "contratista": "Constructora XYZ S.A.S.",
      "estado": "EN EJECUCION",
      "fecha_inicio": "2025-02-01",
      "fecha_terminacion_actual": "2025-12-31",
      "actividad": "Construcción de obras civiles",
      "unidad_medida": "Metro lineal",
      "cantidad_ejecutada": 150.5,
      "ubicaciones": [
        {
          "comuna": "San Javier",
          "barrio": "Las Independencias",
          "latitud": 6.2389,
          "longitud": -75.609
        }
      ]
    }
  ],
  "presupuesto": [
    {
      "numero_proyecto": "240010",
      "nombre_proyecto": "Unidos por el Agua",
      "secretaria": "Secretaría de Infraestructura Física",
      "anio": 2026,
      "tipo": "ejecucion",
      "valor": 150000000
    }
  ],
  "empleabilidad": [
    {
      "numero_contrato": "4600104949",
      "identificador_simple": "Puntos Críticos",
      "secretaria": "Secretaría de Infraestructura Física",
      "anio": 2026,
      "mes": 7,
      "total_personas": 45,
      "total_hombres": 30,
      "total_mujeres": 15,
      "personas_vinculacion_vigente": 40
    }
  ]
}
```

### 📊 Estructura de Metadatos

`metadata` describe **únicamente** la paginación de `contratos` — `presupuesto` y `empleabilidad` se devuelven siempre completos (sin paginar).

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `lastUpdated` | string (ISO 8601) \| `null` | Fecha real de la última modificación de un contrato dentro del resultado filtrado; `null` si no hay contratos que coincidan |
| `totalRecords` | number | Número total de contratos que coinciden con los filtros |
| `page` | number | Página actual de resultados |
| `limit` | number | Cantidad de registros por página |
| `pages` | number | Número total de páginas disponibles |

### 📄 Estructura de Contrato

#### 📋 Campos Principales del Contrato

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | number | ID único del contrato |
| `cud` | string \| `null` | Código Único de dependencia (único en el sistema) |
| `fecha_actualizacion_reporte` | string \| `null` | Fecha de la última actualización del reporte |
| `secretaria` | string \| `null` | Nombre de la secretaría responsable |
| `vigencia_contrato` | number \| `null` | Año de vigencia del contrato |
| `numero_contrato` | string \| `null` | Número oficial del contrato |
| `objeto` | string \| `null` | Objeto del contrato |
| `identificador_simple` | string \| `null` | Identificador simplificado del contrato |
| `contratista` | string \| `null` | Nombre del contratista |
| `estado` | string \| `null` | Estado actual del contrato |
| `fecha_inicio` | string \| `null` | Fecha de inicio del contrato |
| `fecha_terminacion_actual` | string \| `null` | Fecha de terminación actual (con prórrogas) |
| `actividad` | string \| `null` | Nombre de la actividad asociada al contrato (una sola, no un array) |
| `unidad_medida` | string \| `null` | Unidad de medida de la actividad |
| `cantidad_ejecutada` | number \| `null` | Cantidad ejecutada de la actividad |
| `ubicaciones` | array | Ubicaciones geográficas del contrato (ver abajo) |

> ℹ️ **Cambios respecto a versiones anteriores:** se eliminaron del contrato los campos `supervisor_tecnico`, `proyecto_estrategico`, `tipo_contrato`, `porcentaje_programado`, `porcentaje_avance_ejecutado`, `valor_actual_contrato`, `facturado_contrato`, `indicador_plan_desarrollo` y `observaciones`. Los arrays `actividades`, `beneficiarios` y `pgirs` también se eliminaron: `actividad`/`unidad_medida`/`cantidad_ejecutada` ahora son campos planos del contrato (regla de negocio: un contrato tiene una sola actividad asociada, no varias).

#### 📍 Campos de Ubicaciones (Array)

Por regla de negocio, un contrato debe tener **máximo una** ubicación (si por datos corruptos llegara a tener más, el endpoint conserva solo una: la más reciente, o la que coincide con el filtro `comuna` si se usó ese filtro).

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `comuna` | string \| `null` | Nombre de la comuna o corregimiento |
| `barrio` | string \| `null` | Nombre del barrio |
| `latitud` | number | Coordenada de latitud |
| `longitud` | number | Coordenada de longitud |

> ℹ️ El campo `direccion_referencia` ya no existe en la respuesta.

### 💰 Estructura de Presupuesto (Array)

Ejecución y proyección de presupuesto por proyecto y año. **Sin relación (FK) con `contratos`** — se filtra solo por `dependencia`, se devuelve siempre completo.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `numero_proyecto` | string | Número del proyecto |
| `nombre_proyecto` | string \| `null` | Nombre del proyecto |
| `secretaria` | string \| `null` | Nombre de la secretaría responsable del proyecto |
| `anio` | number | Año del valor de presupuesto |
| `tipo` | `"ejecucion"` \| `"proyeccion"` | Tipo de valor: ejecución real o proyección |
| `valor` | number | Valor en pesos |

### 👷 Estructura de Empleabilidad (Array)

Personas contratadas por mes, por contrato (en formato SAP — usa su propio catálogo de contratos, independiente de `tbl_tacita_contratos`). **Sin relación (FK) con `contratos`** — se filtra solo por `dependencia`, se devuelve siempre completo.

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `numero_contrato` | string | Número de contrato (formato SAP) |
| `identificador_simple` | string \| `null` | Identificador simplificado del contrato |
| `secretaria` | string \| `null` | Nombre de la secretaría responsable del contrato |
| `anio` | number | Año del reporte |
| `mes` | number | Mes del reporte (1-12) |
| `total_personas` | number \| `null` | Total de personas contratadas acumuladas |
| `total_hombres` | number \| `null` | Total de hombres |
| `total_mujeres` | number \| `null` | Total de mujeres |
| `personas_vinculacion_vigente` | number \| `null` | Personas con vinculación vigente |

---

## ⚠️ Respuestas de Error

### ❌ 400 Bad Request

Parámetros inválidos en la solicitud.

```json
{
  "statusCode": 400,
  "message": "Error en los parámetros",
  "error": "Bad Request"
}
```

### 🔒 401 Unauthorized

API Key faltante o inválida.

```json
{
  "statusCode": 401,
  "message": "API Key requerida para Tacita de Plata. Envíe la API Key en el header X-API-Key o como parámetro ?apikey=",
  "error": "Unauthorized"
}
```

### 🚦 429 Too Many Requests

Se ha excedido el límite de rate limit.

```json
{
  "statusCode": 429,
  "message": "Rate limit excedido. Intente nuevamente en 45 segundos.",
  "error": "Too Many Requests"
}
```

### 💥 500 Internal Server Error

Error interno del servidor.

```json
{
  "statusCode": 500,
  "message": "Error al obtener los contratos completos",
  "error": "Internal Server Error"
}
```

---

## 🛡️ Seguridad y Middlewares

El endpoint está protegido por los siguientes middlewares en orden:

1. 🚦 **RateLimitMiddleware**: **30 solicitudes por minuto por IP**. Al exceder el límite, devuelve `429` e incluye los headers `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset` y `Retry-After` (segundos).
2. 🔐 **ApiKeyTacitaPlataMiddleware**: Valida la API Key específica para Tacita de Plata
3. 📊 **PowerBiMiddleware**: Agrega headers de CORS, cache y seguridad orientados a Power BI

---

## 📄 Paginación

La API implementa paginación estándar mediante los parámetros `page` y `limit` (por defecto `limit=100`). La paginación **solo aplica a `contratos`** — `presupuesto` y `empleabilidad` siempre vienen completos en cada respuesta.

### 🧭 Ejemplo de navegación

```bash
# Primera página (100 registros)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Segunda página (desde 101 - 200 registros)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=2" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Obtener más valores de los registros (usar con precaución)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=1&limit=1000" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```
## 🔍 Filtros Avanzados

### 🎯 Combinar múltiples filtros

Los filtros se pueden combinar para obtener resultados más específicos:

```bash
# Filtrar por comuna
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?comuna=3" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Filtrar por dependencia (afecta contratos, presupuesto y empleabilidad)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=6" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Combinar dependencia + comuna
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=6&comuna=14" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```

## 🗄️ Modelo de Datos

### 📊 Diagrama de Relaciones

```
TacitaContrato (1) ──── (0..1) TacitaUbicacion   [regla de negocio: máx. 1 activa]
       │
       ├──── (1) TacitaActividad   [N:1 — se aplana en la respuesta, ya no es array]
       │
       └──── (1) Dependencia (Secretaría)

TacitaProyecto (1) ──── (N) TacitaPresupuesto     [sin FK a TacitaContrato]
       │
       └──── (1) Dependencia (Secretaría)

TacitaEmpleabilidadContrato (1) ──── (N) TacitaEmpleabilidad   [catálogo propio, sin FK a TacitaContrato]
       │
       └──── (1) Dependencia (Secretaría)
```

> ⚠️ `presupuesto` y `empleabilidad` no cuelgan de `tbl_tacita_contratos`: tienen granularidad distinta (proyecto+año y contrato-SAP+mes respectivamente) y usan sus propios catálogos (`tbl_tacita_proyecto` y `tbl_tacita_empleabilidad_contrato`). Los antiguos arrays `beneficiarios` y `pgirs` ya no existen.

### 🗂️ Tablas de Base de Datos

- 📄 **tbl_tacita_contratos**: Tabla principal de contratos
- 📍 **tbl_tacita_ubicaciones**: Ubicaciones geográficas de los contratos (máx. 1 vigente por contrato)
- ⚡ **tbl_tacita_actividades**: Catálogo de actividades (relación N:1 con el contrato)
- 💰 **tbl_tacita_presupuesto**: Ejecución/proyección de presupuesto por proyecto y año
- 🎯 **tbl_tacita_proyecto**: Catálogo de proyectos asociados al presupuesto
- 👷 **tbl_tacita_empleabilidad**: Personas contratadas por mes (formato SAP)
- 📇 **tbl_tacita_empleabilidad_contrato**: Catálogo de contratos SAP usado por empleabilidad (independiente de `tbl_tacita_contratos`)
- 🏢 **tbl_dependencia**: Catálogo de dependencias/secretarías
- 🏘️ **tbl_barrio**: Catálogo de barrios
- 🗺️ **tbl_comuna_corregimiento**: Catálogo de comunas y corregimientos
- 📏 **tbl_unidades_medida**: Catálogo de unidades de medida

---

## ❓ FAQ (Preguntas Frecuentes)

### 📊 ¿Cuál es el límite máximo de registros por solicitud?

El límite máximo es de 10,000 registros por solicitud (aplica solo a `contratos`). Sin embargo, se recomienda usar paginación con límites más pequeños (100-1000) para un mejor rendimiento.

### 🔄 ¿Con qué frecuencia se actualizan los datos?

Los datos se actualizan en tiempo real conforme se modifican en el sistema. El campo `metadata.lastUpdated` indica la última modificación real de un contrato dentro del resultado filtrado (no la hora de la consulta), y es `null` si el filtro no devuelve contratos.

### 📜 ¿Puedo obtener información histórica de contratos?

Actualmente, el endpoint devuelve solo contratos activos (`activo = true`). Para información histórica, consulte con el administrador del sistema.

### 🔑 ¿Cómo obtengo una API Key?

Contacte al administrador del sistema o al equipo de TI de la Alcaldía de Medellín para obtener una API Key.

### 🚦 ¿Hay límites de tasa (rate limiting)?

Sí: **30 solicitudes por minuto por IP**. Al superarlo, el endpoint responde `429` con el header `Retry-After` indicando cuántos segundos esperar.

### 💥 ¿Qué hacer si recibo un error 500?

Los errores 500 indican problemas en el servidor. Verifique:
1. Que los parámetros sean válidos
2. Intente de nuevo después de unos segundos
3. Si persiste, contacte al soporte técnico

---

## 📋 Catálogos de Referencia

### 🗺️ Comunas y Corregimientos

Para utilizar el filtro `comuna`, estos son los IDs disponibles:

#### 🏙️ Comunas Urbanas (ID 1-16)

| ID | Nombre |
|----|--------|
| 1 | 01 - Popular |
| 2 | 02 - Santa Cruz |
| 3 | 03 - Manrique |
| 4 | 04 - Aranjuez |
| 5 | 05 - Castilla |
| 6 | 06 - Doce de Octubre |
| 7 | 07 - Robledo |
| 8 | 08 - Villa Hermosa |
| 9 | 09 - Buenos Aires |
| 10 | 10 - La Candelaria |
| 11 | 11 - Laureles - Estadio |
| 12 | 12 - La América |
| 13 | 13 - San Javier |
| 14 | 14 - El Poblado |
| 15 | 15 - Guayabal |
| 16 | 16 - Belén |

#### 🌳 Corregimientos Rurales (ID 17-21)

| ID | Nombre |
|----|--------|
| 17 | 50 - San Sebastián de Palmitas |
| 18 | 60 - San Cristóbal |
| 19 | 70 - Altavista |
| 20 | 80 - San Antonio de Prado |
| 21 | 90 - Santa Elena |

#### ⭐ Zona Especial (ID 22)

| ID | Nombre |
|----|--------|
| 22 | 99 - Varias |

**💡 Ejemplo de uso:**

```bash
# Filtrar por Comuna 3 - Manrique
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?comuna=3" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Filtrar por Corregimiento Santa Elena
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?comuna=21" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```

---

### 🏢 Secretarías y Dependencias

Para utilizar el filtro `dependencia`, estos son los IDs disponibles:

| ID | Nombre de la Dependencia |
|----|--------------------------|
| 2 | Secretaría de Seguridad y Convivencia |
| 6 | Secretaría de Infraestructura Física |
| 9 | Secretaría de Medio Ambiente |
| 38 | Instituto Social de Vivienda y Hábitat de Medellín (ISVIMED) |
| 49 | Secretaría de Gestión y Control Territorial |

**💡 Ejemplo de uso:**

```bash
# Filtrar por Secretaría de Infraestructura Física
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=6" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```

---

### 🔗 Filtros Combinados - Ejemplos Avanzados

```bash
# Ejemplo 1: Secretaría de Infraestructura Física en Comuna 3 - Manrique
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=6&comuna=3" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Ejemplo 2: Contratos de Secretaría de Infraestructura en El Poblado
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=6&comuna=14" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Ejemplo 3: ISVIMED en corregimientos rurales (San Cristóbal)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=38&comuna=18" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Ejemplo 4: Filtro por CUD específico
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?cud=SIF1" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```



## 📝 Changelog

### ✨ Versión 2.0.0 (Actual)
- ✅ Se agregan los arrays `presupuesto` y `empleabilidad` (sin FK a contratos, siempre completos, sin paginar)
- ✅ El filtro `proyecto` fue **retirado**
- ✅ `actividad`, `unidad_medida` y `cantidad_ejecutada` pasan de array (`actividades`) a campos planos del contrato
- ❌ Se eliminan del contrato: `tipo_contrato`, `supervisor_tecnico`, `proyecto_estrategico`, `porcentaje_programado`, `porcentaje_avance_ejecutado`, `valor_actual_contrato`, `facturado_contrato`, `indicador_plan_desarrollo`, `observaciones`
- ❌ Se eliminan los arrays `beneficiarios` y `pgirs`
- ❌ Se elimina `direccion_referencia` de `ubicaciones`
- ✅ `ubicaciones` ahora respeta la regla de negocio de máximo 1 ubicación activa por contrato
- ✅ `metadata.lastUpdated` ahora refleja la última modificación real de un contrato filtrado (y es `null` sin resultados), en vez de la hora de la consulta
- ✅ Ya no hay valor por defecto (`CLAVE_SECRETA`) para la API Key: debe configurarse `TACITA_PLATA_API_KEY` en el servidor
- ✅ Rate limit documentado explícitamente: 30 solicitudes/minuto por IP

### Versión 1.0.0
- ✅ Endpoint principal `GET /api/power-bi/tacita-plata`
- ✅ Autenticación mediante API Key
- ✅ Paginación y filtros
- ✅ Soporte para filtros por proyecto, dependencia, comuna y CUD
- ✅ Relaciones completas: ubicaciones, actividades, beneficiarios y PGIRS

---

## 📜 Licencia y Uso

Esta API es propiedad de la **Alcaldía de Medellín** y está destinada exclusivamente para uso oficial y autorizado. El uso no autorizado está prohibido.
