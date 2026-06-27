# Convención FE-XX / BE-XX
- **FE-XX**: historias frontend (issues con label `frontend`)
- **BE-XX**: historias backend (issues con label `backend`)
- El ID va en el **título** del issue: `[FE-42] CRUD bodegas`
- Milestone activo: **MVP-2**
- Backlog canónico: issues de este repo (no duplicar en snapbill-frontend ni backend)

# Template de historia
## Historia
Como **admin del tenant**, quiero **…**, para **…**.
## Criterios de aceptación
- [ ] CA-01: …
- [ ] CA-02: …
## UI / rutas
- Ruta: `/settings/warehouses`
- Menú: Configuración → Bodegas
- Patrón: tabla paginada + modal (TailAdmin)
## API
- `GET /api/v1/warehouses?page=&pageSize=`
- `POST /api/v1/warehouses`
- Issue backend: #XX
## Notas técnicas FE
- Feature: `src/features/.../warehouses/`
- Guards: tenant admin, etc.
## Dependencias
- Bloqueado por: #XX
