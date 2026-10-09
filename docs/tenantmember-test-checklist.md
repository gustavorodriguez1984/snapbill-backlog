# Checklist de Pruebas Manuales — TenantMember (Suscripción Intermedia)

> **Rol:** TenantMember  
> **Plan:** Intermedia  
> **Fecha:** 2026-09-28

---

## 🏢 EMPRESA

- [x] Ver perfil de empresa (`/company/profile`)

---

## 📋 CATÁLOGOS

- [x] Ver categorías (`/catalog/categories`)
- [x] Ver servicios (`/catalog/services`)

---

## 🛒 VENTAS

### Facturar
- [x] Crear factura (`/sales/invoice`)
- [x] Ver facturas (`/sales/invoices`)
- [x] Imprimir factura PDF

### Cotizaciones
- [x] Crear cotización (`/sales/quotes`)
- [x] Ver cotizaciones
- [x] Convertir cotización a factura
- [x] Imprimir cotización

### Clientes
- [x] Ver clientes (`/sales/customers`)

---

## 🚚 COMPRAS

### Órdenes de compra
- [x] Crear orden de compra (`/purchases/orders`)
- [x] Ver órdenes de compra
- [x] Editar orden de compra
- [x] Imprimir orden de compra PDF

### Recepción de compra
- [x] Crear recepcion de compra (`/purchases/receipt`)
- [x] Ver recepciones de compra (`/purchases/receipts`)
- [x] Recibir mercadería

### Facturas proveedor
- [x] Registrar factura de proveedor (`/purchases/supplier-invoices`)
- [x] Ver facturas de proveedor
- [x] Registrar pago a proveedor

### Devoluciones a proveedor
- [x] Crear devolución a proveedor (`/purchases/supplier-returns`)
- [x] Ver devoluciones a proveedor

---

## 📦 INVENTARIO

### Kardex
- [x] Consultar kardex por producto (`/inventory/kardex`)

### Productos
- [x] Ver productos (`/inventory/products`)
- [x] Corregir movimientos de producto

### Transferencias
- [x] Crear transferencia entre bodegas (`/inventory/transfers`)
- [x] Ver transferencias
- [x] Imprimir transferencia

### Ajustes
- [x] Crear ajuste de inventario (`/inventory/adjustments`)
- [x] Ver ajustes de inventario
- [x] Ver detalle de ajuste de inventario
- [x] Imprimir ajuste PDF

### Bodegas
- [x] Ver bodegas (`/inventory/warehouses`)
- [x] Gestionar stock por bodega
- [x] Transferir entre bodegas

---

## 💰 FINANZAS

### Caja
- [x] Ver cajas (`/finance/cash-registers`)
- [x] Editar caja
- [x] Cerrar caja
- [x] Imprimir cierre de caja

### Cuentas Bancarias
- [ ] Ver cuentas bancarias (`/finance/bank-accounts`)

---

## 📊 REPORTES

### Ventas
- [ ] Cotizaciones por periodo (`/reports/quotes`)
- [ ] Catálogo de servicios (`/reports/sales`)
- [ ] Impresión de cotización

### Compras
- [ ] Compras por periodo (`/reports/purchases`)
- [ ] Órdenes de compra (`/reports/purchases`)
- [ ] Facturas de proveedor (`/reports/purchases`)
- [ ] OC pendientes vs recibidas (`/reports/purchases`)

### Inventario
- [ ] Stock crítico (`/reports/inventory`)
- [ ] Stock actual (`/reports/inventory`)
- [ ] Kardex por producto (`/reports/inventory`)
- [ ] Soporte de transferencia entre bodegas
- [ ] Catálogo de productos (`/reports/inventory`)
- [ ] Cierre de inventario (`/reports/inventory`)

### Finanzas
- [ ] Estado de cuenta cliente (`/reports/financial`)
- [ ] Estado de cuenta proveedor (`/reports/financial`)
- [ ] Cierre de caja

### Auxiliares contables
- [ ] Impresión de nota de crédito

---

## ⚠️ NO ACCESIBLES (requieren TenantAdmin)

| Módulo | Proceso |
|--------|---------|
| Catálogos | Crear/Editar/Eliminar categorías |
| Catálogos | Crear/Editar/Importar servicios |
| Ventas | Crear/Editar clientes |
| Ventas | Anular factura |
| Ventas | Editar cotización |
| Ventas | Crear/Editar listas de precios |
| Compras | Aprobar orden de compra |
| Inventario | Crear/Editar/Importar productos |
| Inventario | Crear bodega |
| Finanzas | Crear/Eliminar caja |
| Finanzas | Crear/Editar/Eliminar cuenta bancaria |
| Reportes | Ventas por periodo, cliente, producto, categoría |
| Reportes | Crédito vs contado, conversión cotizaciones, margen rentabilidad |
| Reportes | Histórico precios de compra |
| Reportes | Valoración/rotación inventario, saldo inicial, cierre fiscal |
| Reportes | Cartera, cuentas por pagar |
| Reportes | Libro ventas/compras, notas crédito/débito |
