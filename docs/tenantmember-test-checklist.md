# Checklist de Pruebas Manuales — TenantMember (Suscripción Intermedia)

> **Rol:** TenantMember  
> **Plan:** Intermedia  
> **Fecha:** 2026-09-28

---

## 🏢 EMPRESA

- [ ] Ver perfil de empresa (`/company/profile`)

---

## 📋 CATÁLOGOS

- [ ] Ver categorías (`/catalog/categories`)
- [ ] Ver servicios (`/catalog/services`)

---

## 🛒 VENTAS

### Facturar
- [ ] Crear factura (`/sales/invoice`)
- [ ] Ver facturas (`/sales/invoices`)
- [ ] Editar factura
- [ ] Imprimir factura PDF

### Cotizaciones
- [ ] Crear cotización (`/sales/quotes`)
- [ ] Ver cotizaciones
- [ ] Convertir cotización a factura

### Clientes
- [ ] Ver clientes (`/sales/customers`)

---

## 🚚 COMPRAS

### Órdenes de compra
- [ ] Crear orden de compra (`/purchases/orders`)
- [ ] Ver órdenes de compra
- [ ] Editar orden de compra
- [ ] Imprimir orden de compra PDF

### Recepción de compra
- [ ] Crear recibo de compra (`/purchases/receipt`)
- [ ] Ver recibos de compra (`/purchases/receipts`)
- [ ] Recibir mercadería

### Facturas proveedor
- [ ] Registrar factura de proveedor (`/purchases/supplier-invoices`)
- [ ] Ver facturas de proveedor
- [ ] Registrar pago a proveedor

### Devoluciones a proveedor
- [ ] Crear devolución a proveedor (`/purchases/supplier-returns`)
- [ ] Ver devoluciones a proveedor

---

## 📦 INVENTARIO

### Kardex
- [ ] Consultar kardex por producto (`/inventory/kardex`)

### Productos
- [ ] Ver productos (`/inventory/products`)
- [ ] Corregir movimientos de producto

### Transferencias
- [ ] Crear transferencia entre bodegas (`/inventory/transfers`)
- [ ] Ver transferencias
- [ ] Imprimir transferencia

### Ajustes
- [ ] Crear ajuste de inventario (`/inventory/adjustments`)
- [ ] Ver ajustes de inventario
- [ ] Imprimir ajuste PDF

### Bodegas
- [ ] Ver bodegas (`/inventory/warehouses`)
- [ ] Gestionar stock por bodega
- [ ] Transferir entre bodegas

---

## 💰 FINANZAS

### Caja
- [ ] Ver cajas (`/finance/cash-registers`)
- [ ] Editar caja
- [ ] Cerrar caja
- [ ] Imprimir cierre de caja

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
