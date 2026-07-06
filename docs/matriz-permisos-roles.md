# Matriz de Permisos por Rol

> Fuente de verdad para el frontend: mostrar/ocultar menús, habilitar/deshabilitar botones según el rol del usuario autenticado.

## 1. Roles del Sistema

| Rol | Descripción | Acceso |
|-----|-------------|--------|
| **PlatformAdmin** | Super-admin de la plataforma SnapBill. Sin TenantId. | Solo módulo Plataforma |
| **TenantAdmin** | Administrador de una empresa (tenant). | Todos los módulos del tenant |
| **TenantUser** | Usuario operativo de una empresa (tenant). | Lectura en datos maestros + CRUD en operaciones |

### Claims JWT relevantes

| Claim | Tipo | Ejemplo | Uso |
|-------|------|---------|-----|
| `role` | string | `"TenantAdmin"` | Rol del usuario (puede ser array si tiene múltiples) |
| `tenant_id` | string (GUID) | `"a1b2c3d4-..."` | Tenant al que pertenece (null para PlatformAdmin) |
| `is_account_owner` | string | `"true"` / `"false"` | Si es el dueño de la suscripción |

---

## 2. Matriz de Acceso por Módulo

### Convenciones

| Símbolo | Significado |
|---------|-------------|
| ✅ | Acceso completo (CRUD) |
| 👁️ | Solo lectura (ver listado y detalle) |
| ❌ | Sin acceso |
| 🔒 | Acceso restringido (ver nota) |

---

### 2.1 PlatformAdmin — Solo Plataforma

| Módulo / Pantalla | PlatformAdmin |
|-------------------|:---:|
| **Plataforma** — Suscriptores | ✅ |
| **Plataforma** — Tipos de suscripción | ✅ |
| **Plataforma** — Definiciones de reportes | ✅ |
| **Plataforma** — Configuración global (claves) | ✅ |
| **Plataforma** — Aprobar/rechazar pagos suscripción | ✅ |
| Todos los demás módulos | ❌ |

---

### 2.2 TenantAdmin — Acceso completo al tenant

| Módulo / Pantalla | TenantAdmin | TenantUser |
|-------------------|:---:|:---:|
| **Dashboard** | ✅ | ✅ |
| **Productos** — Ver listado / detalle | ✅ | 👁️ |
| **Productos** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Productos** — Activar / Desactivar | ✅ | ❌ |
| **Productos** — Importar desde Excel | ✅ | ❌ |
| **Servicios** — Ver listado / detalle | ✅ | 👁️ |
| **Servicios** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Servicios** — Importar desde Excel | ✅ | ❌ |
| **Categorías** — Ver listado | ✅ | 👁️ |
| **Categorías** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Clientes** — Ver listado / detalle | ✅ | 👁️ |
| **Clientes** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Proveedores** — Ver listado / detalle | ✅ | 👁️ |
| **Proveedores** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Monedas** — Ver listado | ✅ | 👁️ |
| **Monedas** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Listas de precios** — Ver | ✅ | 👁️ |
| **Listas de precios** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Facturas** — CRUD completo + imprimir | ✅ | ✅ |
| **Facturas** — Registrar pagos | ✅ | ✅ |
| **Cotizaciones** — CRUD + convertir a factura | ✅ | ✅ |
| **Notas de crédito** — Crear + imprimir | ✅ | ✅ |
| **Órdenes de compra** — Crear / Editar | ✅ | ✅ |
| **Órdenes de compra** — Eliminar | ✅ | ❌ |
| **Órdenes de compra** — Aprobar | ✅ | ❌ |
| **Recepciones** — Crear | ✅ | ✅ |
| **Recepciones** — Confirmar recepción (stock) | ✅ | ❌ |
| **Facturas proveedor** — Crear + registrar pagos | ✅ | ✅ |
| **Devoluciones proveedor** — Crear | ✅ | ✅ |
| **Bodegas** — Ver listado | ✅ | 👁️ |
| **Bodegas** — Crear | ✅ | ❌ |
| **Transferencias entre bodegas** — Crear / Ver | ✅ | ✅ |
| **Ajustes de inventario** — Crear / imprimir | ✅ | ❌ |
| **Kardex** — Consultar | ✅ | ✅ |
| **Períodos** — Ver listado | ✅ | ✅ |
| **Períodos** — Cerrar mes / año fiscal | ✅ | ❌ |
| **Numeración** — Ver | ✅ | ✅ |
| **Numeración** — Configurar | ✅ | ❌ |
| **Configuración tenant** — Ver | ✅ | ✅ |
| **Configuración tenant** — Crear / Editar / Eliminar | ✅ | ❌ |
| **Reportes** — CxC, CxP, kardex, inventario bajo | ✅ | ✅ |
| **Reportes** — Margen de ganancia, PDFs sensibles | ✅ | ❌ |
| **Tenant** — Ver perfil | ✅ | ✅ |
| **Tenant** — Editar perfil | ✅ | ❌ |
| **Usuarios del tenant** — Listar / Crear | ✅ | ❌ |
| **Suscripción** — Ver plan actual | ✅ | ✅ |

---

## 3. Detalle de Endpoints API por Módulo

### 3.1 Productos (`/api/v1/products`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/by-sku/{sku}` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/by-barcode/{barcode}` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}/stocks-by-warehouse` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/unit-of-measures` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | **TenantAdminOnly** | ✅ | ❌ |
| PUT | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| DELETE | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| PATCH | `/{id}/deactivate` | TenantAdminOnly | ✅ | ❌ |
| PATCH | `/{id}/activate` | TenantAdminOnly | ✅ | ❌ |
| POST | `/import` | TenantAdminOnly | ✅ | ❌ |

### 3.2 Servicios (`/api/v1/services`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | **TenantAdminOnly** | ✅ | ❌ |
| PUT | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| DELETE | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| POST | `/import` | TenantAdminOnly | ✅ | ❌ |

### 3.3 Categorías (`/api/v1/categories`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | **TenantAdminOnly** | ✅ | ❌ |
| PUT | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| DELETE | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |

### 3.4 Clientes (`/api/v1/customers`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/by-tax-id/{taxId}` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}/account-statement` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | **TenantAdminOnly** | ✅ | ❌ |
| PUT | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| DELETE | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |

### 3.5 Proveedores (`/api/v1/suppliers`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | **TenantAdminOnly** | ✅ | ❌ |
| PUT | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| DELETE | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |

### 3.6 Facturas (`/api/v1/invoices`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| GET | `/{id}` | TenantMember | ✅ | ✅ |
| GET | `/by-number/{invoiceNumber}` | TenantMember | ✅ | ✅ |
| GET | `/{id}/print` | TenantMember | ✅ | ✅ |
| GET | `/{id}/payments` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantMember | ✅ | ✅ |
| POST | `/{id}/payments` | TenantMember | ✅ | ✅ |

### 3.7 Cotizaciones (`/api/v1/quotes`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| GET | `/{id}` | TenantMember | ✅ | ✅ |
| GET | `/{id}/print` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantMember | ✅ | ✅ |
| PATCH | `/{id}` | TenantMember | ✅ | ✅ |
| POST | `/{id}/convert-to-invoice` | TenantMember | ✅ | ✅ |

### 3.8 Notas de crédito (`/api/v1/credit-notes`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/{id}` | TenantMember | ✅ | ✅ |
| GET | `/{id}/print` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantMember | ✅ | ✅ |

### 3.9 Órdenes de compra (`/api/v1/purchase-orders`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| GET | `/{id}` | TenantMember | ✅ | ✅ |
| GET | `/by-number/{orderNumber}` | TenantMember | ✅ | ✅ |
| GET | `/{id}/print` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantMember | ✅ | ✅ |
| PUT | `/{id}` | TenantMember | ✅ | ✅ |
| DELETE | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| PATCH | `/{id}/approve` | TenantAdminOnly | ✅ | ❌ |

### 3.10 Recepciones (`/api/v1/purchase-receipts`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| GET | `/{id}` | TenantMember | ✅ | ✅ |
| GET | `/by-number/{receiptNumber}` | TenantMember | ✅ | ✅ |
| GET | `/{id}/print` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantMember | ✅ | ✅ |
| PATCH | `/{id}/receive` | **TenantAdminOnly** | ✅ | ❌ |

### 3.11 Facturas proveedor (`/api/v1/supplier-invoices`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantMember | ✅ | ✅ |
| POST | `/{id}/payments` | TenantMember | ✅ | ✅ |

### 3.12 Devoluciones proveedor (`/api/v1/supplier-returns`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| POST | `/` | TenantMember | ✅ | ✅ |

### 3.13 Bodegas (`/api/v1/warehouses`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}/stocks` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | TenantAdminOnly | ✅ | ❌ |

### 3.14 Transferencias (`/api/v1/warehouse-transfers`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantMember | ✅ | ✅ |

### 3.15 Ajustes de inventario (`/api/v1/inventory-adjustments`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| POST | `/` | TenantAdminOnly | ✅ | ❌ |
| GET | `/{id}/print` | TenantAdminOnly | ✅ | ❌ |

### 3.16 Kardex (`/api/v1/products/{id}/kardex`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |

### 3.17 Períodos (`/api/v1/inventory/periods`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| POST | `/{id}/close-month` | TenantAdminOnly | ✅ | ❌ |
| POST | `/fiscal-close` | TenantAdminOnly | ✅ | ❌ |

### 3.18 Listas de precios (`/api/v1/price-lists`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | TenantAdminOnly | ✅ | ❌ |
| PUT | `/{id}` | TenantAdminOnly | ✅ | ❌ |
| DELETE | `/{id}` | TenantAdminOnly | ✅ | ❌ |

### 3.19 Numeración (`/api/v1/document-numbering`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| GET | `/{documentType}` | TenantMember | ✅ | ✅ |
| PUT | `/{documentType}` | TenantAdminOnly | ✅ | ❌ |

### 3.20 Configuración tenant (`/api/v1/global-configurations`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ |
| GET | `/by-key/{configKey}` | TenantMember | ✅ | ✅ |
| GET | `/{id}` | TenantMember | ✅ | ✅ |
| POST | `/` | TenantAdminOnly | ✅ | ❌ |
| PUT | `/{id}` | TenantAdminOnly | ✅ | ❌ |
| DELETE | `/{id}` | TenantAdminOnly | ✅ | ❌ |

### 3.21 Reportes (`/api/v1/reports`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/available` | TenantMember | ✅ | ✅ |
| GET | `/accounts-receivable` | TenantMember | ✅ | ✅ |
| GET | `/accounts-receivable/print` | TenantAdminOnly | ✅ | ❌ |
| GET | `/accounts-payable` | TenantMember | ✅ | ✅ |
| GET | `/accounts-payable/print` | TenantAdminOnly | ✅ | ❌ |
| GET | `/sales/pdf` | TenantAdminOnly | ✅ | ❌ |
| GET | `/inventory-low/pdf` | TenantMember | ✅ | ✅ |
| GET | `/kardex/pdf` | TenantMember | ✅ | ✅ |
| GET | `/profit-margin` | TenantAdminOnly | ✅ | ❌ |
| GET | `/profit-margin/print` | TenantAdminOnly | ✅ | ❌ |

### 3.22 Tenant (`/api/v1/tenants`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/{id}` | TenantMember | ✅ | ✅ |
| PUT | `/{id}` | TenantAdminOnly | ✅ | ❌ |

### 3.23 Usuarios tenant (`/api/v1/tenants/users`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantAdminOnly | ✅ | ❌ |
| POST | `/` | TenantAdminOnly | ✅ | ❌ |

### 3.24 Dashboard (`/api/v1/dashboard`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/summary` | TenantMember | ✅ | ✅ |

### 3.25 Monedas (`/api/v1/coins`)

| Método | Ruta | Política | TenantAdmin | TenantUser |
|--------|------|----------|:---:|:---:|
| GET | `/` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/{id}` | TenantMember | ✅ | ✅ (lectura) |
| GET | `/by-symbol/{symbol}` | TenantMember | ✅ | ✅ (lectura) |
| POST | `/` | **TenantAdminOnly** | ✅ | ❌ |
| PUT | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |
| DELETE | `/{id}` | **TenantAdminOnly** | ✅ | ❌ |

---

## 4. Ejemplo de Implementación en Frontend

### 4.1 Leer el rol del JWT

```typescript
// Decodificar el JWT (sin librería externa)
function parseJwt(token: string): Record<string, unknown> {
  const base64Url = token.split('.')[1];
  const base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
  return JSON.parse(atob(base64));
}

const payload = parseJwt(accessToken);
const userRole = payload.role as string; // "PlatformAdmin" | "TenantAdmin" | "TenantUser"
const isAccountOwner = payload.is_account_owner === "true";
```

### 4.2 Definir menús según rol

```typescript
interface MenuItem {
  label: string;
  route: string;
  icon: string;
  visible: boolean;
  actions?: {
    create?: boolean;
    edit?: boolean;
    delete?: boolean;
    import?: boolean;
  };
}

function getMenuItems(role: string): MenuItem[] {
  const isAdmin = role === 'TenantAdmin';
  const isPlatformAdmin = role === 'PlatformAdmin';

  if (isPlatformAdmin) {
    return [
      { label: 'Plataforma', route: '/platform', icon: 'settings', visible: true },
      // Solo este menú para PlatformAdmin
    ];
  }

  return [
    { label: 'Dashboard', route: '/dashboard', icon: 'home', visible: true },
    {
      label: 'Productos', route: '/products', icon: 'box',
      visible: true,
      actions: { create: isAdmin, edit: isAdmin, delete: isAdmin, import: isAdmin }
    },
    {
      label: 'Servicios', route: '/services', icon: 'briefcase',
      visible: true,
      actions: { create: isAdmin, edit: isAdmin, delete: isAdmin, import: isAdmin }
    },
    {
      label: 'Categorías', route: '/categories', icon: 'tag',
      visible: true,
      actions: { create: isAdmin, edit: isAdmin, delete: isAdmin }
    },
    {
      label: 'Clientes', route: '/customers', icon: 'users',
      visible: true,
      actions: { create: isAdmin, edit: isAdmin, delete: isAdmin }
    },
    {
      label: 'Proveedores', route: '/suppliers', icon: 'truck',
      visible: true,
      actions: { create: isAdmin, edit: isAdmin, delete: isAdmin }
    },
    { label: 'Facturas', route: '/invoices', icon: 'file-text', visible: true,
      actions: { create: true, edit: true, delete: true } },
    { label: 'Cotizaciones', route: '/quotes', icon: 'clipboard', visible: true,
      actions: { create: true, edit: true, delete: true } },
    { label: 'Notas de crédito', route: '/credit-notes', icon: 'minus-circle', visible: true,
      actions: { create: true } },
    { label: 'Órdenes de compra', route: '/purchase-orders', icon: 'shopping-cart', visible: true,
      actions: { create: true, edit: true, delete: isAdmin } },
    { label: 'Recepciones', route: '/purchase-receipts', icon: 'package', visible: true,
      actions: { create: true } },
    { label: 'Facturas proveedor', route: '/supplier-invoices', icon: 'file', visible: true,
      actions: { create: true } },
    { label: 'Devoluciones proveedor', route: '/supplier-returns', icon: 'rotate-ccw', visible: true,
      actions: { create: true } },
    { label: 'Inventario', route: '/inventory', icon: 'warehouse', visible: true,
      actions: { transfer: true, adjust: isAdmin } },
    { label: 'Reportes', route: '/reports', icon: 'bar-chart', visible: true },
    {
      label: 'Configuración', route: '/settings', icon: 'settings',
      visible: true,
      actions: { edit: isAdmin }
    },
  ];
}
```

### 4.3 Habilitar/deshabilitar botones en componentes

```typescript
// En un componente de productos
const canCreate = userRole === 'TenantAdmin';
const canEdit = userRole === 'TenantAdmin';
const canDelete = userRole === 'TenantAdmin';
const canImport = userRole === 'TenantAdmin';

// Template
<button *ngIf="canCreate" (click)="openCreateModal()">Nuevo producto</button>
<button *ngIf="canImport" (click)="openImportModal()">Importar Excel</button>
<button *ngIf="canEdit" (click)="editProduct(product)">Editar</button>
<button *ngIf="canDelete" (click)="deleteProduct(product)">Eliminar</button>
```

### 4.4 Verificar acceso a endpoints protegidos

```typescript
// Si el frontend llama a un endpoint protegido con rol incorrecto,
// el backend retorna 403 Forbidden.
// El interceptor debe mostrar: "No tienes permisos para realizar esta acción"

// Ejemplo de manejo de error en interceptor:
if (error.status === 403) {
  toast.error('No tienes permisos para realizar esta acción');
}
```

---

## 5. Resumen Ejecutivo

### Para PlatformAdmin
- **Solo ve**: Módulo Plataforma (suscriptores, tipos de suscripción, config global, aprobación de pagos)
- **No ve**: Nada del tenant (productos, facturas, clientes, etc.)

### Para TenantAdmin
- **Ve todo**: Todos los módulos del tenant
- **Puede todo**: CRUD completo en todos los módulos + operaciones especiales (importar, aprobar, cerrar períodos)

### Para TenantUser
- **Lectura en datos maestros**: Productos, Servicios, Categorías, Clientes, Proveedores, Monedas, Listas de precios, Bodegas
- **CRUD en operaciones**: Facturas, Cotizaciones, Notas de crédito, OC (crear/editar), Recepciones (crear), Facturas proveedor, Devoluciones, Transferencias
- **No puede**: Importar, aprobar OC, confirmar recepciones, cerrar períodos, gestionar usuarios, editar configuración
