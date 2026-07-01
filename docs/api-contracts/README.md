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

## Índice de contratos (MVP-2)

| BE | FE | Estado | Archivo | Issues |
|----|-----|--------|---------|--------|
| BE-01 | FE-01 | implementado | [BE-01.md](./BE-01.md) | [#1](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/1) · [#5](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/5) |
| BE-02 | — | implementado | [BE-02.md](./BE-02.md) | [#2](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/2) |
| BE-03 | FE-02 | implementado | [BE-03.md](./BE-03.md) | [#3](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/3) · [#6](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/6) |
| BE-04 | FE-03 | implementado | [BE-04.md](./BE-04.md) | [#4](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/4) · [#7](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/7) |
| BE-05 | — | implementado | [BE-05.md](./BE-05.md) | [#8](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/8) |
| BE-06 | FE-04 | implementado | [BE-06.md](./BE-06.md) | [#11](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/11) · [#30](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/30) |
| BE-07 | FE-08 | implementado | [BE-07.md](./BE-07.md) | [#9](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/9) · [#34](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/34) |
| BE-08 | — | implementado | [BE-08.md](./BE-08.md) | [#10](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/10) |
| BE-09 | FE-05 | implementado | [BE-09.md](./BE-09.md) | [#12](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/12) · [#31](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/31) |
| BE-10 | FE-06 | implementado | [BE-10.md](./BE-10.md) | [#13](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/13) · [#32](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/32) |
| BE-11 | FE-07 | implementado | [BE-11.md](./BE-11.md) | [#14](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/14) · [#33](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/33) |
| BE-12 | FE-08 | implementado | [BE-12.md](./BE-12.md) | [#15](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/15) · [#34](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/34) |
| BE-13 | FE-09 | implementado | [BE-13.md](./BE-13.md) | [#16](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/16) · [#35](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/35) |
| BE-14 | FE-10 | implementado | [BE-14.md](./BE-14.md) | [#17](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/17) · [#36](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/36) |
| BE-15 | FE-16 | implementado | [BE-15.md](./BE-15.md) | [#18](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/18) · [#42](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/42) |
| BE-16 | FE-11 | implementado | [BE-16.md](./BE-16.md) | [#19](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/19) · [#37](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/37) |
| BE-17 | FE-12 | implementado | [BE-17.md](./BE-17.md) | [#20](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/20) · [#38](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/38) |
| BE-18 | FE-13 | implementado | [BE-18.md](./BE-18.md) | [#21](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/21) · [#39](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/39) |
| BE-19 | FE-14 | implementado | [BE-19.md](./BE-19.md) | [#22](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/22) · [#40](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/40) |
| BE-20 | FE-15 | implementado | [BE-20.md](./BE-20.md) | [#23](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/23) · [#41](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/41) |
| BE-21 | FE-17 | implementado | [BE-21.md](./BE-21.md) | [#24](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/24) · [#43](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/43) |
| BE-22 | FE-18 | implementado | [BE-22.md](./BE-22.md) | [#25](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/25) · [#44](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/44) |
| BE-23 | FE-19 | implementado | [BE-23.md](./BE-23.md) | [#26](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/26) · [#45](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/45) |
| BE-24 | FE-20 | implementado | [BE-24.md](./BE-24.md) | [#27](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/27) · [#46](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/46) |
| BE-25 | FE-21 | implementado | [BE-25.md](./BE-25.md) | [#28](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/28) · [#47](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/47) |
| BE-26 | FE-22 | implementado | [BE-26.md](./BE-26.md) | [#29](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/29) · [#48](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/48) |
| BE-27 | FE-23 | implementado | [BE-27.md](./BE-27.md) | [#49](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/49) · [#52](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/52) |
| BE-28 | FE-24 | implementado | [BE-28.md](./BE-28.md) | [#50](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/50) · [#53](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/53) |
| BE-29 | FE-25 | implementado | [BE-29.md](./BE-29.md) | [#51](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/51) · [#54](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/54) |

> Verificación automática: OpenAPI en `https://localhost:7200/openapi/v1.json` (Scalar: `/scalar/v1`).

## Labels en issues

| Label | Significado |
|-------|-------------|
| `api-contract` | BE define o actualiza contrato HTTP |
| `blocked` | FE esperando contrato BE |
