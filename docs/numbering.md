# Convención FE-XX / BE-XX

- **BE-XX** — historias de backend (`snapbill-backend`)
- **FE-XX** — historias de frontend (`snapbill-frontend`)
- El ID va en el título: `[BE-01] …` o `[FE-01] …`
- Milestone activo: **MVP-2**
- Backlog canónico: Issues de este repo (no duplicar historias en repos de código)

## Enlaces entre repos

| Enlace | Dónde |
|--------|--------|
| BE → FE | En issue BE: `Frontend: FE-##` · `#N` |
| FE → BE | En issue FE: sección **Integración API** → BE-## · `#N` |
| Contrato HTTP | [docs/api-contracts/BE-XX.md](./api-contracts/README.md) |

## Labels

| Label | Uso |
|-------|-----|
| `backend` / `frontend` | Tipo |
| `api-contract` | BE expone o cambia API |
| `blocked` | FE esperando BE o contrato |
| `prioridad-p0` … `p2` | Prioridad MVP-2 |

---

# Template issue BE

```markdown
## Historia
Como **rol**, quiero **…**, para **…**.

**Repositorio:** `snapbill-backend`
**Frontend:** FE-## · #N

## Contrato API
**Estado:** borrador | implementado
**Archivo:** docs/api-contracts/BE-XX.md

*(tabla endpoints, JSON, errores — o enlace al .md)*

## Criterios de aceptación
### CA-01 — …
- [ ] …

## Verificación manual
1. …
```

---

# Template issue FE

Ver [api-contracts/FE-INTEGRACION-TEMPLATE.md](./api-contracts/FE-INTEGRACION-TEMPLATE.md).

```markdown
## Historia
Como **rol**, quiero **…**, para **…**.

**Repositorio:** `snapbill-frontend`

## Integración API
**Backend:** BE-## · #N
**Contrato:** docs/api-contracts/BE-XX.md
**Bloqueado hasta:** contrato `implementado`

## UI / rutas
- Ruta: `/...`
- Menú: …

## Criterios de aceptación
- [ ] CA-01: …

## Dependencias
- Bloqueado por: #N
```
