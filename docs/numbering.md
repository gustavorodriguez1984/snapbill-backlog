# Convención FE-XX / BE-XX
- **BE-XX** — historias de backend
- **FE-XX** — historias de frontend
- El ID va en el título: `[BE-01] …` o `[FE-01] …`
- Milestone activo: **MVP-2**
- Backlog canónico: Issues de este repo (no duplicar en repos de código)
- Enlazar pares: en BE poner `Frontend: #N` / en FE poner `Backend: #N`

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
