# Contratos API — SnapBill

Contratos HTTP para integración **backend ↔ frontend**. Ambos repos leen este directorio en [snapbill-backlog](https://github.com/gustavorodriguez1984/snapbill-backlog).

## Cómo usa el frontend una historia

1. Abrir issue **FE-##** → sección **Integración API** → enlace a **BE-##**.
2. Leer **`BE-XX.md`** en esta carpeta (ejemplos JSON y errores).
3. Confirmar en Scalar: `https://localhost:7200/scalar/v1` (AppHost).

No hace falta copiar contratos en el chat del agente: basta con `Implementa FE-23`.

## Cómo cierra el backend una historia

1. Implementar endpoints y tests.
2. Actualizar **`BE-XX.md`**: estado `implementado`, ejemplos reales, enlace al PR.
3. Añadir sección **Contrato API** en el issue BE (o sincronizar desde el `.md`).
4. Comentar en issue **FE-##**: «Contrato listo — ver `docs/api-contracts/BE-XX.md`».
5. Quitar label `blocked` del FE si aplicaba.

## Documentos

| Archivo | Uso |
|---------|-----|
| [CONVENTIONS.md](./CONVENTIONS.md) | Auth, JSON, enums, paginación, ProblemDetails |
| [TEMPLATE.md](./TEMPLATE.md) | Plantilla para nuevo `BE-XX.md` |
| [FE-INTEGRACION-TEMPLATE.md](./FE-INTEGRACION-TEMPLATE.md) | Bloque para issues frontend |

## Índice de contratos

| BE | FE | Estado | Archivo | Issues |
|----|-----|--------|---------|--------|
| BE-27 | FE-23 | borrador | [BE-27.md](./BE-27.md) | [BE #49](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/49) · [FE #52](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/52) |
| BE-28 | FE-24 | — | *(pendiente)* | [#50](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/50) · [#53](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/53) |
| BE-29 | FE-25 | — | *(pendiente)* | [#51](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/51) · [#54](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/54) |

> Ampliar esta tabla al cerrar cada BE. Contratos `implementado` solo después del merge en `snapbill-backend`.

## Labels en issues

| Label | Significado |
|-------|-------------|
| `api-contract` | BE define o actualiza contrato HTTP |
| `blocked` | FE esperando contrato BE |
