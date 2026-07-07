# Reportes por Suscripción y Roles

> Fuente de verdad para el catálogo de reportes PDF disponibles por plan de SnapBill.
> Mercado objetivo: Nicaragua.
> Alcance: facturación e inventario únicamente; **no contabilidad**.
> Formato de entrega: **PDF bajo demanda**.

---

## 1. Planes de suscripción

| Plan | Precio mensual | Perfil de usuario |
|------|----------------|-------------------|
| **Basic** | $25 | Pequeño comercio, facturación simple, poco inventario |
| **Intermedia** | $50 | PYME con inventario, compras y necesidad de controlar cartera |
| **Premium** | $75 | Negocio con múltiples bodegas, análisis comercial y requiere reportes para contador |

---

## 2. Matriz de reportes por plan

### Leyenda

| Símbolo | Significado |
|---------|-------------|
| ✅ | Incluido en el plan |
| ❌ | No incluido (upsell al plan superior) |

### 2.1 Plan Basic

| Código | Reporte | Rol que accede | Notas |
|--------|---------|----------------|-------|
| `customer-account-statement` | Estado de cuenta por cliente | TenantAdmin, TenantUser | PDF individual por cliente |
| `sales` | Ventas por período | TenantAdmin, TenantUser | Ya existe `/reports/sales/pdf` |
| `quotes-by-period` | Cotizaciones por período | TenantAdmin, TenantUser | |
| `kardex` | Kardex por producto | TenantAdmin, TenantUser | Ya existe `/reports/kardex/pdf` |
| `current-stock` | Stock actual por producto | TenantAdmin, TenantUser | |

**Mensaje de venta:** *Opera y cobra.*

---

### 2.2 Plan Intermedia

Incluye todos los de Basic, más:

| Código | Reporte | Rol que accede | Notas |
|--------|---------|----------------|-------|
| `accounts-receivable` | Cuentas por cobrar vencidas (antigüedad) | TenantAdmin | Ya existe `/reports/accounts-receivable/print` |
| `accounts-payable` | Cuentas por pagar vencidas (antigüedad) | TenantAdmin | Ya existe `/reports/accounts-payable/print` |
| `profit-margin` | Margen de utilidad por producto/categoría | TenantAdmin | Ya existe `/reports/profit-margin/print` |
| `inventory-low` | Productos con stock bajo | TenantAdmin, TenantUser | Ya existe `/reports/inventory-low/pdf` |
| `purchases-by-period` | Compras por período | TenantAdmin, TenantUser | Incluye OC + recepciones + facturas proveedor |
| `purchase-orders-vs-receipts` | Órdenes de compra pendientes vs recibidas | TenantAdmin, TenantUser | |
| `sales-by-customer` | Ventas por cliente | TenantAdmin | |
| `sales-by-product` | Ventas por producto | TenantAdmin | |

**Mensaje de venta:** *Controla tu rentabilidad y cartera.*

---

### 2.3 Plan Premium

Incluye todos los de Intermedia, más:

| Código | Reporte | Rol que accede | Notas |
|--------|---------|----------------|-------|
| `sales-book` | Libro de ventas (auxiliar para contador) | TenantAdmin | Listado detallado, **no formato DGI** |
| `purchases-book` | Libro de compras (auxiliar para contador) | TenantAdmin | Listado detallado, **no formato DGI** |
| `credit-notes-by-period` | Notas de crédito emitidas por período | TenantAdmin | |
| `supplier-credit-notes-by-period` | Notas de crédito de proveedor por período | TenantAdmin | |
| `supplier-account-statement` | Estado de cuenta por proveedor | TenantAdmin, TenantUser | |
| `inventory-valuation` | Valoración de inventario por bodega | TenantAdmin | Costo total al MCPP |
| `inventory-rotation` | Rotación de inventario | TenantAdmin | Productos estancados vs rápidos |
| `sales-by-category` | Ventas por categoría | TenantAdmin | |
| `quote-conversion` | Conversión de cotizaciones a facturas | TenantAdmin | |
| `sales-credit-vs-cash` | Ventas a crédito vs contado | TenantAdmin | |
| `purchase-price-history` | Histórico de precios de compra por producto | TenantAdmin | Negociación con proveedores |

**Mensaje de venta:** *Analiza y cumple.*

---

## 3. Resumen por área de negocio

| Área | Reportes | Planes |
|------|----------|--------|
| **Ventas** | Ventas por período, por cliente, por producto, por categoría, crédito vs contado, cotizaciones, conversión de cotizaciones, notas de crédito | Basic + Intermedia + Premium |
| **Compras** | Compras por período, OC vs recepciones, notas de crédito proveedor, histórico precios de compra | Intermedia + Premium |
| **CxC / CxP** | Estado de cuenta cliente/proveedor, antigüedad CxC/CxP | Basic + Intermedia + Premium |
| **Inventario** | Stock actual, stock bajo, kardex, valoración, rotación | Basic + Intermedia + Premium |
| **Fiscal auxiliar** | Libro de ventas, libro de compras | Premium |

---

## 4. Control de acceso por rol

| Rol | Acceso a reportes |
|-----|-------------------|
| **TenantAdmin** | Todos los reportes habilitados por su plan |
| **TenantUser** | Solo reportes operacionales marcados como accesibles en la matriz |
| **PlatformAdmin** | No accede a reportes de tenant (solo módulo Plataforma) |

> El backend debe validar tanto el plan de suscripción (`SubscriptionTypeReport`) como el rol del usuario (`TenantMember` vs `TenantAdminOnly`).

---

## 5. Implementación técnica

1. Extender `ReportDefinition` con los nuevos códigos.
2. Poblar `SubscriptionTypeReport` según la matriz de esta página.
3. Crear endpoints `GET /api/v1/reports/{code}/pdf` para cada reporte nuevo.
4. Actualizar `GET /api/v1/reports/available` para reflejar rol + plan.
5. Frontend: bloquear/ocular reportes no habilitados; mostrar badge de plan requerido para upsell.

---

## 6. Issues relacionados

- Backend: **BE-30** — Implementar reportes por suscripción y roles
- Frontend: **FE-28** — Pantalla de reportes con filtros por plan y rol
