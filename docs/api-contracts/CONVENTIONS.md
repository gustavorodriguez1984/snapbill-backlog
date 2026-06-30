# Convenciones de contrato API — SnapBill

Contratos compartidos entre **snapbill-backend** y **snapbill-frontend**. La verdad humana vive aquí y en el issue **BE-##**; la verdad automática es **OpenAPI/Scalar** en el backend.

## URLs

| Entorno | API base | Documentación interactiva |
|---------|----------|---------------------------|
| Dev (AppHost) | `https://localhost:7200` | `https://localhost:7200/scalar/v1` |
| OpenAPI JSON | `https://localhost:7200/openapi/v1.json` | — |

## Autenticación

```http
Authorization: Bearer <access_token>
```

- Token vía `POST /api/identity/login` o `POST /api/identity/refresh`.
- Endpoints de negocio requieren claim `tenant_id` en el JWT (salvo PlatformAdmin sin tenant).
- Rutas identity anónimas: register, login, refresh, onboarding público según endpoint.

## JSON

| Regla | Detalle |
|-------|---------|
| Nombres de propiedad | **camelCase** en request/response |
| Enums en JSON | **string** con nombre del enum (`"Credito"`, `"Contado"`) — no enteros |
| Fechas | `date-time` ISO 8601 (`DateTimeOffset`) |
| GUIDs | string UUID |
| Decimales | número JSON (no string) |
| Nullable | campo omitido o `null` según endpoint |

## Paginación (`GET` listados)

**Query:** `page` (default 1), `pageSize` (default 20), `searchText`, `sortBy`, `sortDirection`

**Response 200:**

```json
{
  "items": [ ],
  "totalCount": 0,
  "page": 1,
  "pageSize": 20,
  "totalPages": 0
}
```

## Errores (`Result` → ProblemDetails)

Content-Type: `application/problem+json`

```json
{
  "type": "https://tools.ietf.org/html/rfc9110#section-15.5.5",
  "title": "Conflict",
  "status": 409,
  "detail": "Human-readable message.",
  "errorCode": "Product.SkuAlreadyExists"
}
```

| `ErrorType` | HTTP |
|-------------|------|
| Validation | 400 |
| NotFound | 404 |
| Forbidden | 403 |
| Conflict | 409 |
| ServiceUnavailable | 503 |
| TooManyRequests | 429 |

El frontend debe leer **`errorCode`** para mensajes de UI; `detail` es fallback.

## Validación FluentValidation (400)

Errores de body/query inválido antes del handler — mismo formato ProblemDetails con `errorCode` de validación cuando aplique.

## Multipart

Onboarding, vouchers: `multipart/form-data`; campos documentados en el contrato **BE-##** (límites: 1 MB, PDF/JPEG/PNG).

## Versionado

- Rutas de negocio: `/api/v1/...`
- Identity: `/api/identity/...`
- Cambios breaking → nuevo grupo `/api/v2/...`
- Campos opcionales nuevos en misma versión → compatible

## Flujo de trabajo

1. **BE** implementa → actualiza sección **Contrato API** en issue + archivo `docs/api-contracts/BE-XX.md` → estado **implementado**.
2. **FE** implementa leyendo **FE-##** (integración) + **BE-XX.md** — sin copiar contrato en el chat.
3. FE lleva label `blocked` hasta que contrato BE esté **implementado**.

## OperationId

Cada endpoint en backend debe tener `.WithName("StableOperationId")` — usar ese nombre en la columna Scalar del contrato.
