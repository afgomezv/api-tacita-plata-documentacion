# 📚 API de Tacita de Plata - Documentación

## 📋 Descripción General

La API de Tacita de Plata proporciona acceso a información completa sobre contratos del programa "Tacita de Plata" de la Alcaldía de Medellín. Este endpoint está diseñado específicamente para integraciones con Power BI y otras herramientas de análisis de datos.

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
- 🎯 **Valor por defecto**: `CLAVE_SECRETA`



## 🚀 Endpoint Principal

### 📊 Obtener Contratos Completos

```http
GET https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata
```

Retorna información completa de los contratos del programa Tacita de Plata, incluyendo todas sus relaciones: ubicaciones, actividades, beneficiarios y datos PGIRS.

#### 🔍 Parámetros de Consulta (Query Parameters)

| Parámetro | Tipo | Requerido | Descripción | Valor por defecto |
|-----------|------|-----------|-------------|-------------------|
| `apikey` | string | No* | API Key para autenticación (alternativa al header) | - |
| `page` | number | No | Número de página para paginación | `1` |
| `limit` | number | No | Cantidad de contratos por página (máximo 10,000) | `100` |
| `proyecto` | number | No | ID del proyecto estratégico para filtrar | - |
| `dependencia` | number | No | ID de la secretaría/dependencia para filtrar | - |
| `comuna` | number | No | ID de la comuna/corregimiento para filtrar | - |
| `cud` | string | No | Código Único de dependencia (ej: SIF1, SIF2) | - |

*Requerido si no se envía en el header `X-API-Key`

#### 💡 Ejemplo de Solicitud

```bash
# Con API Key en header
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=1&limit=50&proyecto=1" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Con API Key en query parameter
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?apikey=tacita_plata_visor_estrategico_2024&page=1&limit=50"

# Con filtros
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=1&limit=100&comuna=3&dependencia=5" \
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
    "lastUpdated": "2024-11-07T10:30:00.000Z",
    "totalRecords": 250,
    "page": 1,
    "limit": 100,
    "pages": 3
  },
  "contratos": [
    {
      "id": 1,
      "cud": "SIF1",
      "fecha_actualizacion_reporte": "2024-11-01",
      "secretaria": "Secretaría de Infraestructura Física",
      "vigencia_contrato": 2024,
      "numero_contrato": "4600099999",
      "tipo_contrato": "Obra Pública",
      "objeto": "Construcción de infraestructura comunitaria",
      "supervisor_tecnico": "Juan Pérez",
      "proyecto_estrategico": "Medellín Futuro",
      "identificador_simple": "Proyecto A",
      "contratista": "Constructora ABC S.A.S",
      "estado": "En Ejecución",
      "fecha_inicio": "2024-01-15",
      "fecha_terminacion_actual": "2024-12-31",
      "porcentaje_programado": "45.5",
      "porcentaje_avance_ejecutado": "42.3",
      "valor_actual_contrato": "1500000000",
      "facturado_contrato": "635000000",
      "indicador_plan_desarrollo": "Infraestructura para el desarrollo",
      "observaciones": "Avance normal del proyecto",
      "ubicaciones": [
        {
          "direccion_referencia": "Calle 50 # 45-30",
          "comuna": "Comuna 3 - Manrique",
          "barrio": "La Cruz",
          "latitud": "6.2742",
          "longitud": "-75.5656"
        }
      ],
      "actividades": [
        {
          "nombre": "Construcción de muros",
          "unidad_medida": "Metros cuadrados",
          "meta": "500",
          "cantidad_ejecutada": "210"
        },
      ],
      "beneficiarios": [
        {
          "cedula": "1234567890",
          "nombre_completo": "María García López"
        },
      ],
      "pgirs": [
        {
          "programa": "Gestión Integral de Residuos",
          "persona_enlace_pgirs": "Ana Rodríguez",
          "dependencia_responsable": "Área Metropolitana",
          "dificultades_identificadas": "Ninguna",
          "accion_correctiva": "N/A",
          "relacion_contrato": "Directo"
        }
      ]
    }
  ]
}
```

### 📊 Estructura de Metadatos

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `lastUpdated` | string (ISO 8601) | Fecha y hora de la última actualización de los datos |
| `totalRecords` | number | Número total de contratos que coinciden con los filtros |
| `page` | number | Página actual de resultados |
| `limit` | number | Cantidad de registros por página |
| `pages` | number | Número total de páginas disponibles |

### 📄 Estructura de Contrato

#### 📋 Campos Principales del Contrato

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `id` | number | ID único del contrato |
| `cud` | string | Código Único de dependencia (único en el sistema) |
| `fecha_actualizacion_reporte` | string | Fecha de la última actualización del reporte |
| `secretaria` | string | Nombre de la secretaría responsable |
| `vigencia_contrato` | number | Año de vigencia del contrato |
| `numero_contrato` | string | Número oficial del contrato |
| `tipo_contrato` | string | Tipo de contrato (ej: Obra Pública, Prestación de Servicios) |
| `objeto` | string | Descripción del objeto contractual |
| `supervisor_tecnico` | string | Nombre del supervisor técnico |
| `proyecto_estrategico` | string | Nombre del proyecto estratégico asociado |
| `identificador_simple` | string | Identificador simplificado del proyecto |
| `contratista` | string | Nombre del contratista |
| `estado` | string | Estado actual del contrato |
| `fecha_inicio` | string | Fecha de inicio del contrato |
| `fecha_terminacion_actual` | string | Fecha de terminación actual/proyectada |
| `porcentaje_programado` | string | Porcentaje de avance programado |
| `porcentaje_avance_ejecutado` | string | Porcentaje de avance ejecutado |
| `valor_actual_contrato` | string | Valor actual del contrato en pesos |
| `facturado_contrato` | string | Valor facturado a la fecha |
| `indicador_plan_desarrollo` | string | Indicador del plan de desarrollo asociado |
| `observaciones` | string | Observaciones generales del contrato |

#### 📍 Campos de Ubicaciones (Array)

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `direccion_referencia` | string | Dirección de referencia de la ubicación |
| `comuna` | string | Nombre de la comuna o corregimiento |
| `barrio` | string | Nombre del barrio |
| `latitud` | string | Coordenada de latitud |
| `longitud` | string | Coordenada de longitud |

#### ⚡ Campos de Actividades (Array)

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `nombre` | string | Nombre de la actividad |
| `unidad_medida` | string | Unidad de medida de la actividad |
| `meta` | string | Meta programada |
| `cantidad_ejecutada` | string | Cantidad ejecutada a la fecha |

#### 👥 Campos de Beneficiarios (Array)

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `cedula` | string | Número de cédula del beneficiario |
| `nombre_completo` | string | Nombre completo del beneficiario |

#### ♻️ Campos de PGIRS (Array)

PGIRS: Plan de Gestión Integral de Residuos Sólidos

| Campo | Tipo | Descripción |
|-------|------|-------------|
| `programa` | string | Nombre del programa PGIRS |
| `persona_enlace_pgirs` | string | Persona de enlace para PGIRS |
| `dependencia_responsable` | string | Dependencia responsable del PGIRS |
| `dificultades_identificadas` | string | Dificultades identificadas en la gestión |
| `accion_correctiva` | string | Acciones correctivas implementadas |
| `relacion_contrato` | string | Tipo de relación con el contrato |

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
  "message": "Rate limit excedido. Por favor, intente de nuevo más tarde.",
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

1. 🚦 **RateLimitMiddleware**: Limita la cantidad de solicitudes por IP
2. 🔐 **ApiKeyTacitaPlataMiddleware**: Valida la API Key específica para Tacita de Plata
3. 📊 **PowerBiMiddleware**: Middleware adicional para servicios Power BI

---

## 📄 Paginación

La API implementa paginación estándar mediante los parámetros `page` y `limit` por default limit tiene un valor de 100.

### 🧭 Ejemplo de navegación

```bash
# Primera página (100 registros)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Segunda página (desde 101 - 200 registros)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=2" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Obtener vas valores de los registros (usar con precaución)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?page=1&limit=1000" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```
## 🔍 Filtros Avanzados

### 🎯 Combinar múltiples filtros

Los filtros se pueden combinar para obtener resultados más específicos:

```bash
# Filtrar por proyecto y comuna
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?proyecto=1&comuna=3" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Filtrar por dependencia
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=5" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```

## 🗄️ Modelo de Datos

### 📊 Diagrama de Relaciones

```
TacitaContrato (1) ──── (1) TacitaUbicacion
       │
       ├──── (1) TacitaActividad
       │
       ├──── (1) TacitaBeneficiario
       │
       ├──── (1) TacitaPgirs
       │
       ├──── (1) Dependencia (Secretaría)
       │
       └──── (1) ProyectoAsociado
```

### 🗂️ Tablas de Base de Datos

- 📄 **tbl_tacita_contratos**: Tabla principal de contratos
- 📍 **tbl_tacita_ubicaciones**: Ubicaciones geográficas de los contratos
- ⚡ **tbl_tacita_actividades**: Actividades ejecutadas en cada contrato
- 👥 **tbl_tacita_beneficiarios**: Beneficiarios de cada contrato
- ♻️ **tbl_tacita_pgirs**: Información de gestión de residuos
- 🏢 **tbl_dependencia**: Catálogo de dependencias/secretarías
- 🎯 **tbl_proyecto_asociado**: Catálogo de proyectos estratégicos asociados
- 🏘️ **tbl_barrio**: Catálogo de barrios
- 🗺️ **tbl_comuna_corregimiento**: Catálogo de comunas y corregimientos
- 📏 **tbl_unidades_medida**: Catálogo de unidades de medida

---

## ❓ FAQ (Preguntas Frecuentes)

### 📊 ¿Cuál es el límite máximo de registros por solicitud?

El límite máximo es de 10,000 registros por solicitud. Sin embargo, se recomienda usar paginación con límites más pequeños (100-1000) para un mejor rendimiento.

### 🔄 ¿Con qué frecuencia se actualizan los datos?

Los datos se actualizan en tiempo real conforme se modifican en el sistema. El campo `metadata.lastUpdated` indica la fecha y hora de la última consulta.

### 📜 ¿Puedo obtener información histórica de contratos?

Actualmente, el endpoint devuelve solo contratos activos (`activo = true`). Para información histórica, consulte con el administrador del sistema.

### 🔑 ¿Cómo obtengo una API Key?

Contacte al administrador del sistema o al equipo de TI de la Alcaldía de Medellín para obtener una API Key.

### 🚦 ¿Hay límites de tasa (rate limiting)?

Sí, el endpoint tiene rate limiting implementado. El límite específico depende de la configuración del servidor.

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

### 🎯 Proyectos Estratégicos Asociados

Para utilizar el filtro `proyecto`, estos son los IDs disponibles:

| ID | Nombre del Proyecto |
|----|---------------------|
| 1 | Acciones de Mejoramiento Ambientales del Hábitat Medellín |
| 2 | Acciones que Posibiliten la Protección, Recuperación y Apropiación del Espacio Público |
| 3 | Espacio Público para el Disfrute Colectivo y la Sostenibilidad Territorial |
| 4 | Espacio Público para la Convivencia Ciudadana |
| 5 | Espacios que nos Unen (AEEP) |
| 6 | Fomentar la Sana Convivencia, el Encuentro Ciudadano, el Sentido de Identidad y Pertenencia por el Territorio |
| 7 | Lugares que Vibran (Planeacion) |
| 8 | Mejoramiento de Vivienda |
| 9 | Tacita de Plata |

**💡 Ejemplo de uso:**

```bash
# Filtrar por proyecto "Tacita de Plata"
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?proyecto=9" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Filtrar por proyecto "Mejoramiento de Vivienda"
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?proyecto=8" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Combinar filtros: proyecto + comuna
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?proyecto=9&comuna=3" \
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
# Ejemplo 1: Proyectos de "Tacita de Plata" en Comuna 3 - Manrique
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?proyecto=9&comuna=3" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Ejemplo 2: Contratos de Secretaría de Infraestructura en El Poblado
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?dependencia=6&comuna=14" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Ejemplo 3: Mejoramiento de Vivienda en corregimientos rurales (San Cristóbal)
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?proyecto=8&comuna=18" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"

# Ejemplo 4: Todos los filtros combinados
curl -X GET "https://visorestrategicobackend-gkejc4hthnace6b4.eastus2-01.azurewebsites.net/api/power-bi/tacita-plata?proyecto=9&dependencia=6&comuna=3" \
  -H "X-API-Key: tacita_plata_visor_estrategico_2024"
```



## 📝 Changelog

### ✨ Versión 1.0.0 (Actual)
- ✅ Endpoint principal `GET /api/power-bi/tacita-plata`
- ✅ Autenticación mediante API Key
- ✅ Paginación y filtros
- ✅ Soporte para filtros por proyecto, dependencia, comuna y CUD
- ✅ Relaciones completas: ubicaciones, actividades, beneficiarios y PGIRS

---

## 📜 Licencia y Uso

Esta API es propiedad de la **Alcaldía de Medellín** y está destinada exclusivamente para uso oficial y autorizado. El uso no autorizado está prohibido.
