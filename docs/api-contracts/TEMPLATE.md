# BE-XX — Título corto

| Campo | Valor |
|-------|--------|
| **Estado contrato** | `borrador` \| `implementado` |
| **Issue BE** | [#N](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/N) |
| **Issue FE** | [#N](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/N) |
| **Backend PR** | *(enlace al merge)* |
| **Última actualización** | YYYY-MM-DD |

> Copiar este archivo a `BE-XX.md` al cerrar la historia backend. Ver [CONVENTIONS.md](./CONVENTIONS.md).

---

## Endpoints

| Operación | Método | Ruta | Auth | OperationId |
|-----------|--------|------|------|-------------|
| Ejemplo | POST | `/api/v1/recurso` | TenantMember | `CreateRecurso` |

---

## POST `/api/v1/recurso`

**Content-Type:** `application/json`

### Request

```json
{
  "campo": "valor"
}
```

### Response 201

```json
{
  "id": "00000000-0000-0000-0000-000000000001"
}
```

**Headers:** `Location` vía `CreatedAtRoute` → `GET /api/v1/recurso/{id}`

---

## Errores

| HTTP | errorCode | Cuándo |
|------|-----------|--------|
| 400 | `Validation.*` | body inválido |
| 404 | `Recurso.NotFound` | id inexistente |
| 409 | `Recurso.Conflict` | regla de negocio |

---

## Notas de integración FE

- *(Completar en issue FE-##; no duplicar schema completo aquí si ya está arriba.)*
