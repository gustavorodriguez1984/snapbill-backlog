# Plantilla — sección Integración API (issue FE-##)

Pegar en el cuerpo del issue **frontend** debajo de **Historia**:

```markdown
---

## Integración API

**Backend:** BE-XX · [#N](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/N)
**Contrato:** [docs/api-contracts/BE-XX.md](https://github.com/gustavorodriguez1984/snapbill-backlog/blob/master/docs/api-contracts/BE-XX.md)
**Estado contrato:** borrador | implementado
**Bloqueado hasta:** BE cerrado + contrato `implementado`

### Endpoints que consume esta pantalla

| Uso en UI | Método | Ruta | OperationId |
|-----------|--------|------|-------------|
| Listado | GET | `/api/v1/...` | `GetAll...` |
| Acción | PATCH | `/api/v1/.../{id}/...` | `...` |

### Contrato JSON

→ Ver **BE-XX.md** (no duplicar schemas completos aquí).

### Notas solo-frontend

- Rutas React: `/catalog/products`
- Rol: `TenantAdmin` para acciones de escritura
- Toast / guard por `errorCode` (ver tabla en BE-XX.md)
- Scalar: probar con JWT de `admin@snapbill.dev` / tenant owner según CA

### Dependencias GitHub

- Bloqueado por: #N (issue BE)
```
