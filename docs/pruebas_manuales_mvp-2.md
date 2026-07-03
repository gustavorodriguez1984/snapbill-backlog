# Pruebas manuales — SnapBill MVP-2

Documento para **testers funcionales**. Valida los procesos nuevos del **segundo MVP** (milestone **MVP-2**) sobre frontend + backend desplegados en entorno de prueba.

**Alcance:** historias **FE-01 … FE-25** del milestone MVP-2 (issues `#5`–`#7`, `#30`–`#54` en [snapbill-backlog](https://github.com/gustavorodriguez1984/snapbill-backlog/issues?q=milestone%3AMVP-2+label%3Afrontend)).

**Prerrequisito:** completar el escenario base del **MVP-1** (tenant operativo, catálogo, maestros, al menos una recepción y una factura). Referencia: `snapbill-frontend/docs/pruebas_manuales_mvp.md`.

---

## 1. Cómo usar este documento

1. Tener **backend** y **frontend** en el mismo entorno (local o staging).
2. Anotar en la **tabla de seguimiento (§10)** fecha, tester, entorno y resultado (`OK` / `FALLA` / `BLOQ`).
3. Ejecutar los casos **en el orden sugerido (§3)** cuando sea posible; algunos tracks son independientes.
4. Ante un fallo, capturar: pantalla, paso, mensaje UI, código HTTP (DevTools → Network) y número de issue FE/BE si aplica.
5. Los casos **Plataforma** requieren usuario `PlatformAdmin`; los de **TenantAdmin** requieren rol administrador del tenant.

### Convenciones

| Símbolo | Significado |
|---------|-------------|
| **TenantAdmin** | Usuario con rol `TenantAdmin` del tenant de prueba |
| **TenantUser** | Usuario operativo sin rol admin |
| **PlatformAdmin** | Administrador de la plataforma SnapBill |
| **Owner** | Suscriptor dueño del tenant (cuenta de facturación SaaS) |
| `{tenant}` | Nombre del tenant de prueba (ej. TechNova Equipos S.A.) |

### Entorno sugerido (desarrollo)

| Componente | URL típica |
|------------|------------|
| Frontend | `http://localhost:5173` |
| API | `https://localhost:7200` |
| OpenAPI | `https://localhost:7200/scalar/v1` |

---

## 2. Datos y roles de prueba

### 2.1 Roles mínimos

| Rol | Uso en MVP-2 |
|-----|----------------|
| **TenantAdmin** | Períodos, ajustes, reportes admin, pagos proveedor, límite crédito |
| **TenantUser** | Facturación, cotizaciones, transferencias, listados |
| **Owner** | Voucher renovación, upgrade trial, banner mora |
| **PlatformAdmin** | Aprobar renovaciones, simular suspensión (según seed) |

### 2.2 Datos transaccionales recomendados

Antes de empezar MVP-2, el tenant debe tener:

| Dato | Propósito |
|------|-----------|
| ≥ 2 **bodegas** (una principal) | Stock por bodega, transferencias, recepción/factura |
| ≥ 1 **cliente** con RIF | Crédito, estado de cuenta, cotizaciones |
| ≥ 1 **proveedor** | Facturas CxP, devoluciones |
| Productos con **stock > 0** en al menos una bodega | Factura, transferencia, barcode |
| Plan de suscripción con reportes MVP-2 habilitados | Cartera, CxP, margen (según plan SaaS) |

---

## 3. Orden de ejecución recomendado

```
Bloque A — Control operativo (períodos)
  PM2-01 → PM2-02 → PM2-03

Bloque B — SaaS / suscripción (opcional si hay datos de mora/renovación)
  PM2-04 → PM2-05 → PM2-06 → PM2-07

Bloque C — Ventas a crédito y cobranza
  PM2-08 → PM2-09 → PM2-10 → PM2-11 → PM2-12 → PM2-13

Bloque D — Inventario multi-bodega
  PM2-14 → PM2-15 → PM2-16 → PM2-17

Bloque E — Compras y CxP
  PM2-18 → PM2-19 → PM2-20

Bloque F — Comercial
  PM2-21 → PM2-22

Bloque G — Reportes analíticos
  PM2-23

Bloque H — Catálogo y facturación extendida
  PM2-24 → PM2-25 → PM2-26
```

> **Nota:** Bloques C–H pueden ejecutarse en paralelo por distintos testers si el escenario base ya existe.

---

## 4. Bloque A — Períodos de inventario (FE-01, FE-02, FE-03)

### PM2-01 · FE-01 — Vista de períodos por año fiscal

**Ruta:** `/inventory/periods`  
**Rol:** TenantAdmin  
**Issue:** [#5](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/5)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Menú **Inventario → Períodos** | Pantalla carga con selector de año fiscal |
| 2 | Revisar combo **Año fiscal** | Lista años disponibles; por defecto año actual o el más reciente |
| 3 | Cambiar año | Tabla recarga 12 meses con estados (Abierto / Cerrado) |
| 4 | Revisar columnas | Mes, estado, acciones coherentes con API |

**API de referencia:** `GET /api/v1/inventory/periods?year={año}`

---

### PM2-02 · FE-02 — Cierre mensual con confirmación

**Ruta:** `/inventory/periods`  
**Rol:** TenantAdmin  
**Issue:** [#6](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/6)  
**Precondición:** Al menos un mes en estado **Abierto**.

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Clic **Cerrar período** en fila Abierto | Modal de confirmación con advertencia de bloqueo de documentos |
| 2 | Clic **Cancelar** | Modal cierra; mes sigue Abierto |
| 3 | Repetir → **Confirmar cierre** | Toast éxito; fila pasa a **Cerrado** |
| 4 | Intentar emitir factura con fecha en ese mes cerrado | Error claro (período cerrado) |

**API de referencia:** `POST /api/v1/inventory/periods/{year}/{month}/close`

---

### PM2-03 · FE-03 — Cierre fiscal anual

**Ruta:** `/inventory/periods`  
**Rol:** TenantAdmin  
**Issue:** [#7](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/7)  
**Precondición:** Los 12 meses del año seleccionado en estado **Cerrado**.

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Con meses pendientes | Botón **Ejecutar cierre fiscal {Año}** deshabilitado + tooltip explicativo |
| 2 | Cerrar los 12 meses | Botón se habilita |
| 3 | Clic en cierre fiscal → confirmar | Año fiscal queda sellado; UI refleja `isClosed` |
| 4 | Verificar año siguiente | Períodos de apertura disponibles según reglas BE-04 |

**API de referencia:** `POST /api/v1/inventory/periods/{year}/fiscal-close`

---

## 5. Bloque B — Suscripción SaaS (FE-04, FE-08, FE-09, FE-10)

### PM2-04 · FE-04 — Upgrade trial a plan de pago

**Ruta:** `/account/upgrade-trial`  
**Rol:** Owner en trial activo  
**Issue:** [#30](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/30)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Login owner con suscripción trial | Acceso a pantalla upgrade o CTA en cuenta |
| 2 | Elegir plan de pago y completar flujo | Voucher / pago según plan; confirmación visible |
| 3 | Tras aprobación (si aplica) | Tenant conserva acceso; plan actualizado |

---

### PM2-05 · FE-08 — Subir voucher de renovación

**Ruta:** `/account/billing` o `/account/subscription-payments`  
**Rol:** Owner con prefactura `PendingPayment`  
**Issue:** [#34](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/34)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Abrir bandeja de pagos del owner | Fila prefactura sin voucher |
| 2 | Clic **Subir comprobante** | Selector archivo (PDF/JPEG/PNG ≤ 1 MB) |
| 3 | Subir voucher válido | Estado → `PendingApproval`; archivo visible |
| 4 | Segundo upload sobre mismo pago | **409** o mensaje de idempotencia |

**API de referencia:** `POST /api/v1/subscription-payments/{id}/voucher`

---

### PM2-06 · FE-09 — Aprobar renovación (PlatformAdmin)

**Ruta:** `/platform/subscription-payments`  
**Rol:** PlatformAdmin  
**Issue:** [#35](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/35)  
**Precondición:** Pago en `PendingApproval` marcado como renovación (PM2-05).

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Abrir bandeja plataforma | Columna o badge **Renovación** vs **Alta** |
| 2 | **Aprobar renovación** | Suscripción extendida; nueva `NextBillingDate` visible |
| 3 | Probar **Rechazar** con motivo | Estado rechazado; motivo registrado |

---

### PM2-07 · FE-10 — Cuenta suspendida por mora

**Ruta:** `/signin` → `/account/suspended`  
**Rol:** Owner suspendido  
**Issue:** [#36](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/36)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Login con cuenta `Suspended` por mora | Pantalla dedicada con instrucciones de regularización |
| 2 | Enlace a pago / voucher | Redirige a flujo FE-08 |
| 3 | Owner con prefactura próxima a vencer (no suspendido) | Banner preventivo en app |

---

## 6. Bloque C — Ventas a crédito y cobranza

### PM2-08 · FE-05 — Facturación a crédito

**Ruta:** `/sales/invoice` (pestaña **Emitir**)  
**Rol:** TenantUser o TenantAdmin  
**Issue:** [#31](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/31)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Elegir término **Crédito** | Se oculta sección de pagos al emitir |
| 2 | Opcional: fecha vencimiento | Campo `dueDate` disponible |
| 3 | Emitir factura | **201**; sin pagos registrados al crear |
| 4 | Pestaña **Consultar** | Badge **Pendiente** y saldo > 0 |
| 5 | Filtrar por estado de cobro | Listado filtra correctamente |

---

### PM2-09 · FE-06 — Abonos en factura a crédito

**Ruta:** `/sales/invoice` → **Consultar** → **Ver**  
**Rol:** TenantUser  
**Issue:** [#32](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/32)  
**Precondición:** Factura crédito con saldo (PM2-08).

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Abrir detalle factura crédito | Botón **Registrar abono** visible |
| 2 | Abono parcial (monto < saldo) | Saldo restante; badge **Parcial** |
| 3 | Segundo abono hasta cubrir total | Badge **Pagada**; saldo 0 |
| 4 | Intentar abono con monto > saldo | Error en campo o toast `Invoice.Overpayment` |
| 5 | Revisar historial | Tabla de abonos con fecha, método, monto |

**API de referencia:** `POST /api/v1/invoices/{id}/payments`

---

### PM2-10 · FE-07 — Notas de crédito

**Ruta:** `/sales/invoice` y `/sales/credit-notes`  
**Rol:** TenantUser / TenantAdmin  
**Issue:** [#33](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/33)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Detalle factura emitida → **Nota de crédito** | Formulario con líneas y cantidades |
| 2 | Devolución parcial (cantidad ≤ facturada) | NC creada; stock repuesto si aplica |
| 3 | Menú **Ventas → Notas de crédito** | Listado con NC recién creada |
| 4 | **Imprimir / Descargar PDF** | PDF abre correctamente |

---

### PM2-11 · FE-11 — Límite de crédito en cliente

**Ruta:** `/masters/customers`  
**Rol:** TenantAdmin  
**Issue:** [#37](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/37)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Editar cliente → **Límite de crédito** | Campo numérico guardable |
| 2 | Guardar con límite (ej. C$ 5000) | Indicador **Usado / Disponible** visible |
| 3 | Emitir factura crédito que exceda disponible | Toast `Customer.CreditLimitExceeded`; no emite |
| 4 | Emitir dentro del límite | Factura OK; crédito usado actualizado |

---

### PM2-12 · FE-16 — Estado de cuenta cliente (PDF)

**Ruta:** `/masters/customers` → **Ver**  
**Rol:** TenantUser / TenantAdmin  
**Issue:** [#42](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/42)  
**Precondición:** Cliente con facturas y/o abonos en el periodo.

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Clic **Ver** en cliente | Ficha con datos y sección **Estado de cuenta** |
| 2 | Rango **Desde / Hasta** con movimientos | Fechas válidas |
| 3 | Clic **Estado de cuenta** | Descarga PDF (blob) |
| 4 | Abrir PDF | Encabezado tenant + cliente; facturas, abonos, saldos del periodo |

**API de referencia:** `GET /api/v1/customers/{id}/account-statement?from=&to=&format=pdf`

---

### PM2-13 · FE-12 — Reporte cartera (CxC)

**Ruta:** `/reports/accounts-receivable`  
**Rol:** TenantAdmin (plan con `AccountsReceivableReport`)  
**Issue:** [#38](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/38)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Menú **Reportes → Cartera** | Formulario fecha de corte |
| 2 | **Generar reporte** | Tabla por cliente con buckets de antigüedad |
| 3 | **Exportar PDF** | Descarga PDF coherente con pantalla |
| 4 | TenantUser | Ítem de menú no visible o acceso denegado |

**API de referencia:** `GET /api/v1/reports/accounts-receivable?asOf=` · `GET .../print?format=pdf`

---

## 7. Bloque D — Inventario multi-bodega

### PM2-14 · FE-13 — Stock en detalle de bodega

**Ruta:** `/company/warehouses` → **Ver stocks**  
**Rol:** TenantUser  
**Issue:** [#39](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/39)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Listado bodegas → acción ver detalle | Ruta `/company/warehouses/{id}` |
| 2 | Pestaña **Stocks** | Tabla producto / cantidad en esa bodega |
| 3 | Comparar con recepción previa en esa bodega | Cantidades coherentes |

---

### PM2-15 · FE-13 — Bodega en recepción y factura

**Rutas:** `/purchases/receipt`, `/sales/invoice`  
**Rol:** TenantUser  
**Issue:** [#39](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/39)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | **Recepción:** registrar entrada | Campo **Bodega** obligatorio en cabecera |
| 2 | Tras guardar | Stock aumenta solo en bodega elegida |
| 3 | **Productos → Editar** | Desglose stock por bodega visible |
| 4 | **Factura** con línea producto | Selector **Bodega** por línea obligatorio |
| 5 | Stock insuficiente en bodega elegida | Validación antes de emitir |

---

### PM2-16 · FE-14 — Transferencias entre bodegas

**Ruta:** `/inventory/transfers`  
**Rol:** TenantUser  
**Issue:** [#40](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/40)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Crear transferencia origen ≠ destino | Formulario con líneas y cantidades |
| 2 | Cantidad > stock origen | Error de validación |
| 3 | Transferencia exitosa | Stock − origen, + destino |
| 4 | Listado | Transferencia visible con estado |

---

### PM2-17 · FE-15 — Ajustes de inventario

**Ruta:** `/inventory/adjustments`  
**Rol:** TenantAdmin  
**Issue:** [#41](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/41)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | TenantUser accede | Solo lectura o acceso denegado |
| 2 | TenantAdmin → nuevo ajuste | Tipo, motivo, bodega, líneas |
| 3 | Ajuste negativo | Confirmación; stock disminuye |
| 4 | Kardex | Movimiento de ajuste registrado |

---

## 8. Bloque E — Compras y cuentas por pagar

### PM2-18 · FE-17 — Facturas de proveedor

**Ruta:** `/purchases/supplier-invoices`  
**Rol:** TenantUser  
**Issue:** [#43](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/43)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | **Nueva factura** | Proveedor, moneda, número, fecha, monto |
| 2 | Opcional: vincular recepción | Búsqueda por número recepción |
| 3 | Guardar | Listado muestra factura **Pendiente** y saldo = total |
| 4 | **Ver** detalle | Total, pagado, saldo pendiente |

---

### PM2-19 · FE-18 — Pagos proveedor y reporte CxP

**Rutas:** `/purchases/supplier-invoices`, `/reports/accounts-payable`  
**Rol:** TenantAdmin  
**Issue:** [#44](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/44)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Detalle factura proveedor → **Registrar pago** | Modal monto, fecha, método |
| 2 | Pago total | Estado **Pagada**; saldo 0 |
| 3 | **Reportes → Cuentas por pagar** | Fecha corte → tabla antigüedad por proveedor |
| 4 | **Exportar PDF** | PDF descargado |

**API pagos:** `POST /api/v1/supplier-invoices/{id}/payments`

---

### PM2-20 · FE-19 — Devolución a proveedor

**Ruta:** `/purchases/supplier-returns`  
**Rol:** TenantUser  
**Issue:** [#45](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/45)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Registrar devolución | Proveedor, bodega, líneas producto |
| 2 | Cantidad > stock bodega | Error claro |
| 3 | Devolución OK | Stock bodega disminuye; resumen post-alta |
| 4 | Opcional: nota crédito proveedor + factura CxP | Ajuste saldo CxP si aplica |

---

## 9. Bloque F — Comercial

### PM2-21 · FE-20 — Listas de precios

**Rutas:** `/catalog/price-lists`, `/masters/customers`  
**Rol:** TenantAdmin  
**Issue:** [#46](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/46)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Crear lista con ítems por producto | CRUD listas |
| 2 | Asignar lista a cliente | Selector en ficha cliente |
| 3 | Emitir factura a ese cliente | Precio sugerido desde lista al agregar línea |

---

### PM2-22 · FE-21 — Cotizaciones y conversión

**Ruta:** `/sales/quotes`  
**Rol:** TenantUser  
**Issue:** [#47](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/47)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Pestaña **Crear** cotización | Cliente, validez, líneas |
| 2 | Guardar | Resultado con número cotización |
| 3 | **Consultar** → **Ver** | PDF proforma (Imprimir / Descargar) |
| 4 | **Convertir a factura** | Modal confirma; avisa stock insuficiente si aplica |
| 5 | Confirmar | Redirección a `/sales/invoice` con factura creada |

**API:** `POST /api/v1/quotes/{id}/convert-to-invoice`

---

## 10. Bloque G — Reportes analíticos

### PM2-23 · FE-22 — Reporte margen de rentabilidad

**Ruta:** `/reports/profit-margin`  
**Rol:** TenantAdmin (plan con `ProfitMarginReport`)  
**Issue:** [#48](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/48)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Rango fechas + **Agrupar por** (Producto / Categoría) | Formulario válido |
| 2 | **Generar reporte** | Tabla ingresos, costo, margen, % |
| 3 | Totales | Coherentes con suma de filas |
| 4 | **Exportar PDF** | Descarga tras generar en pantalla |

---

## 11. Bloque H — Catálogo y facturación extendida

### PM2-24 · FE-23 — Baja lógica de productos

**Ruta:** `/catalog/products`  
**Rol:** TenantAdmin  
**Issue:** [#52](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/52)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Filtro **Activos / Inactivos / Todos** | Listado filtra por `isActive` |
| 2 | **Dar de baja** producto activo | Confirmación; badge **Inactivo** |
| 3 | Producto con stock > 0 | Modal advierte que no se venderá |
| 4 | **Reactivar** | Producto vuelve a activos |
| 5 | Facturar producto inactivo (barcode o selector) | Toast `Product.Inactive` |

---

### PM2-25 · FE-24 — Unidad de medida en productos

**Ruta:** `/catalog/products`  
**Rol:** TenantUser  
**Issue:** [#53](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/53)

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Alta / edición producto | Select **Unidad de medida** (default UND) |
| 2 | Listado productos | Columna **U.M.** visible |
| 3 | Línea en factura / OC / recepción | Unidad junto a cantidad (ej. `x CAJ`) |
| 4 | Kardex / reportes | Unidad coherente donde aplique |

---

### PM2-26 · FE-25 — Escaneo código de barras en factura

**Ruta:** `/sales/invoice` (Emitir)  
**Rol:** TenantUser  
**Issue:** [#54](https://github.com/gustavorodriguez1984/snapbill-backlog/issues/54)  
**Precondición:** Producto activo con código de barras configurado.

| Paso | Acción | Resultado esperado |
|------|--------|-------------------|
| 1 | Campo **Código / barras** visible | Autofocus opcional al entrar |
| 2 | Escanear o escribir código válido + Enter | Línea agregada cantidad 1 |
| 3 | Repetir mismo código | Cantidad incrementa +1 |
| 4 | Código inexistente | Toast **Producto no encontrado** |
| 5 | Producto inactivo | Toast claro (`Product.Inactive`) |
| 6 | (Opcional móvil) Cámara | Escaneo agrega línea |

**API:** `GET /api/v1/products/by-barcode/{code}`

---

## 12. Matriz historias FE → casos PM2

| Historia | Issue backlog | Casos PM2 |
|----------|---------------|-----------|
| FE-01 Períodos — vista anual | #5 | PM2-01 |
| FE-02 Cierre mensual | #6 | PM2-02 |
| FE-03 Cierre fiscal | #7 | PM2-03 |
| FE-04 Upgrade trial | #30 | PM2-04 |
| FE-05 Factura crédito | #31 | PM2-08 |
| FE-06 Abonos | #32 | PM2-09 |
| FE-07 Notas de crédito | #33 | PM2-10 |
| FE-08 Voucher renovación | #34 | PM2-05 |
| FE-09 Aprobar renovación | #35 | PM2-06 |
| FE-10 Cuenta suspendida | #36 | PM2-07 |
| FE-11 Límite crédito cliente | #37 | PM2-11 |
| FE-12 Reporte cartera | #38 | PM2-13 |
| FE-13 Stock por bodega | #39 | PM2-14, PM2-15 |
| FE-14 Transferencias | #40 | PM2-16 |
| FE-15 Ajustes inventario | #41 | PM2-17 |
| FE-16 Estado cuenta PDF | #42 | PM2-12 |
| FE-17 Facturas proveedor | #43 | PM2-18 |
| FE-18 Pagos + CxP | #44 | PM2-19 |
| FE-19 Devolución proveedor | #45 | PM2-20 |
| FE-20 Listas de precios | #46 | PM2-21 |
| FE-21 Cotizaciones | #47 | PM2-22 |
| FE-22 Reporte margen | #48 | PM2-23 |
| FE-23 Baja lógica productos | #52 | PM2-24 |
| FE-24 Unidad de medida | #53 | PM2-25 |
| FE-25 Barcode factura | #54 | PM2-26 |

---

## 13. Tabla de seguimiento

Completar una fila por caso ejecutado.

| Caso | Tester | Fecha | Entorno | Resultado | Issue / notas |
|------|--------|-------|---------|-----------|---------------|
| PM2-01 | | | | | |
| PM2-02 | | | | | |
| PM2-03 | | | | | |
| PM2-04 | | | | | |
| PM2-05 | | | | | |
| PM2-06 | | | | | |
| PM2-07 | | | | | |
| PM2-08 | | | | | |
| PM2-09 | | | | | |
| PM2-10 | | | | | |
| PM2-11 | | | | | |
| PM2-12 | | | | | |
| PM2-13 | | | | | |
| PM2-14 | | | | | |
| PM2-15 | | | | | |
| PM2-16 | | | | | |
| PM2-17 | | | | | |
| PM2-18 | | | | | |
| PM2-19 | | | | | |
| PM2-20 | | | | | |
| PM2-21 | | | | | |
| PM2-22 | | | | | |
| PM2-23 | | | | | |
| PM2-24 | | | | | |
| PM2-25 | | | | | |
| PM2-26 | | | | | |

---

## 14. Referencias

| Recurso | Ubicación |
|---------|-----------|
| Issues MVP-2 | [snapbill-backlog/issues](https://github.com/gustavorodriguez1984/snapbill-backlog/issues?q=milestone%3AMVP-2) |
| Contratos API BE | [docs/api-contracts/](./api-contracts/README.md) |
| Pruebas MVP-1 (base) | `snapbill-frontend/docs/pruebas_manuales_mvp.md` |
| Mapa de rutas UI | `snapbill-frontend/docs/ui-shell.md` |

---

*Documento generado para QA manual MVP-2. Actualizar cuando cambien criterios de aceptación en issues FE.*
