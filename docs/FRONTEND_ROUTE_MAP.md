# Zentto - Mapa Completo de Rutas Frontend

> Documento de referencia para agentes IA y desarrolladores.
> Ultima actualizacion: 2026-03-30

---

## Tabla de Contenidos

1. [Arquitectura General](#arquitectura-general)
2. [Apps y Puertos](#apps-y-puertos)
3. [Paquetes Compartidos](#paquetes-compartidos)
4. [Shell (3000) - Hub Principal](#shell-3000---hub-principal)
5. [Contabilidad (3001)](#contabilidad-3001)
6. [POS (3002)](#pos-3002)
7. [Nomina (3003)](#nomina-3003)
8. [Bancos (3004)](#bancos-3004)
9. [Inventario (3005)](#inventario-3005)
10. [Ventas (3006)](#ventas-3006)
11. [Compras (3007)](#compras-3007)
12. [Restaurante (3008)](#restaurante-3008)
13. [Ecommerce (3009)](#ecommerce-3009)
14. [Auditoria (3010)](#auditoria-3010)
15. [Logistica (3011)](#logistica-3011)
16. [CRM (3012)](#crm-3012)
17. [Manufactura (3013)](#manufactura-3013)
18. [Flota (3014)](#flota-3014)
19. [Shipping (3015)](#shipping-3015)
20. [Report Studio (3017)](#report-studio-3017)
21. [Patrones de Navegacion y Sidebar](#patrones-de-navegacion-y-sidebar)
22. [Convenciones de Rutas](#convenciones-de-rutas)

---

## Arquitectura General

### Stack Tecnologico

| Componente | Tecnologia |
| --- | --- |
| Framework | Next.js 16+ (App Router, file-based routing) |
| Monorepo | npm workspaces |
| UI | MUI v6 + branding personalizado Zentto |
| Estado servidor | React Query (TanStack Query) |
| Estado cliente | Zustand |
| Grids | `@zentto/datagrid` (web component) |
| Reportes | `@zentto/shared-reports` (viewer, designer, core) |

### Estructura del Monorepo

```
web/modular-frontend/
  apps/
    shell/          # 3000 - Hub principal
    contabilidad/   # 3001
    pos/            # 3002
    nomina/         # 3003
    bancos/         # 3004
    inventario/     # 3005
    ventas/         # 3006
    compras/        # 3007
    restaurante/    # 3008
    ecommerce/      # 3009
    auditoria/      # 3010
    logistica/      # 3011
    crm/            # 3012
    manufactura/    # 3013
    flota/          # 3014
    shipping/       # 3015
    report-studio/  # 3017
  packages/
    shared-ui/
    shared-auth/
    shared-api/
    shared-reports/
    module-admin/
    module-contabilidad/
    module-bancos/
    module-inventario/
    module-nomina/
    module-compras/
    module-crm/
    module-ecommerce/
    module-auditoria/
    module-flota/
    module-logistica/
    module-manufactura/
    module-shipping/
```

### Modelo de Despliegue

Cada app puede ejecutarse de forma independiente durante desarrollo. En produccion, todas corren via PM2 dentro de Docker (ver `docker/pm2.config.cjs`). El shell (puerto 3000) actua como hub principal y puede cargar las rutas de los demas modulos internamente.

---

## Apps y Puertos

| App | Puerto | Descripcion |
| --- | --- | --- |
| **Shell** | 3000 | Hub principal, autenticacion, dashboard, configuracion, onboarding |
| **Contabilidad** | 3001 | Asientos, plan de cuentas, activos fijos, fiscal, cierre |
| **POS** | 3002 | Punto de venta, facturacion, caja |
| **Nomina** | 3003 | Empleados, nominas, vacaciones, liquidaciones, salud ocupacional |
| **Bancos** | 3004 | Entidades bancarias, cuentas, movimientos, caja chica |
| **Inventario** | 3005 | Articulos, almacenes, ajustes, traslados, categorias |
| **Ventas** | 3006 | Facturas, clientes, CxC, pedidos ecommerce |
| **Compras** | 3007 | Compras, proveedores, CxP, pagos |
| **Restaurante** | 3008 | Ambientes, recetas, cocina, fiscal |
| **Ecommerce** | 3009 | Tienda publica, carrito, checkout, afiliados |
| **Auditoria** | 3010 | Bitacora, sesiones, alertas, analytics |
| **Logistica** | 3011 | Albaranes, recepciones, transportistas, conductores |
| **CRM** | 3012 | Leads, pipeline, actividades, automatizaciones |
| **Manufactura** | 3013 | BOM, ordenes de produccion, centros de trabajo, rutas |
| **Flota** | 3014 | Vehiculos, combustible, mantenimiento, viajes |
| **Shipping** | 3015 | Portal de paqueteria, envios, rastreo |
| **Report Studio** | 3017 | Disenador de reportes, previsualizacion, wizard |

---

## Paquetes Compartidos

### @zentto/shared-ui

Tema MUI, layouts (DashboardLayout, AuthLayout), componentes toast, payment, branding.

### @zentto/shared-auth

AuthContext, NextAuth adapter, autenticacion biometrica. Provee el contexto de sesion a todas las apps.

### @zentto/shared-api

Funciones HTTP (`apiGet`, `apiPost`, `apiPut`, `apiDelete`), formatters de datos, QueryProvider (React Query), stores Zustand compartidos.

### @zentto/shared-reports

Viewer de reportes, designer visual, motor core del engine de reportes Zentto.

### Modulos de Dominio

| Paquete | Contenido |
| --- | --- |
| `@zentto/module-admin` | EditableDataGrid, CRUD inventario generico |
| `@zentto/module-contabilidad` | Hooks para asientos, cuentas, presupuestos, activos fijos, reportes |
| `@zentto/module-bancos` | Hooks para cuentas bancarias, conciliacion |
| `@zentto/module-inventario` | Articulos, almacenes, ajustes, traslados |
| `@zentto/module-nomina` | Empleados, procesamiento nomina, vacaciones, salud ocupacional |
| `@zentto/module-compras` | Compras, proveedores, cuentas por pagar |
| `@zentto/module-crm` | Leads, pipeline, actividades |
| `@zentto/module-ecommerce` | Tienda, carrito, checkout |
| `@zentto/module-auditoria` | Logs de auditoria, alertas |
| `@zentto/module-flota` | Vehiculos, combustible, mantenimiento |
| `@zentto/module-logistica` | Envios, recepciones |
| `@zentto/module-manufactura` | BOM, ordenes de produccion |
| `@zentto/module-shipping` | Integracion con servicios de paqueteria |

---

## Shell (3000) - Hub Principal

El shell es el punto de entrada principal. Contiene autenticacion, onboarding, dashboard, configuracion global y las rutas embebidas de los modulos principales.

### Autenticacion (publicas, sin layout dashboard)

| Ruta | Descripcion |
| --- | --- |
| `/authentication/login` | Inicio de sesion |
| `/register` | Registro de nueva empresa/usuario |
| `/forgot-password` | Recuperacion de contrasena |
| `/reset-password` | Restablecer contrasena (con token) |
| `/verify-email` | Verificacion de correo electronico |

### Paginas Publicas

| Ruta | Descripcion |
| --- | --- |
| `/pricing` | Planes y precios |
| `/billing/success` | Confirmacion de pago exitoso |
| `/subscription-expired` | Suscripcion expirada |

### Dashboard - Route Group `(dashboard)`

Todas las rutas dentro del route group `(dashboard)` comparten el DashboardLayout (sidebar + topbar).

#### Navegacion Principal

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)` | Home del dashboard (pagina principal post-login) |
| `/(dashboard)/aplicaciones` | Selector de aplicaciones / modulos disponibles |
| `/(dashboard)/addons` | Gestion de addons y extensiones |
| `/(dashboard)/backoffice` | Panel de administracion backoffice |
| `/(dashboard)/pagos` | Historial y gestion de pagos |
| `/(dashboard)/perfil` | Perfil del usuario |
| `/(dashboard)/notificaciones` | Centro de notificaciones |
| `/(dashboard)/docs` | Documentacion integrada |
| `/(dashboard)/info` | Acerca de Zentto |

#### Soporte

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/soporte` | Lista de tickets de soporte |
| `/(dashboard)/soporte/[number]` | Detalle de ticket (ruta dinamica por numero) |
| `/(dashboard)/soporte/nuevo` | Crear nuevo ticket de soporte |

### Contabilidad (dentro del Shell)

Las rutas de contabilidad estan disponibles tanto en el shell como en la app standalone (puerto 3001).

#### Dashboard y Asientos

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/contabilidad` | Dashboard de contabilidad |
| `/(dashboard)/contabilidad/asientos` | Lista de asientos contables |
| `/(dashboard)/contabilidad/asientos/nuevo` | Crear nuevo asiento |
| `/(dashboard)/contabilidad/recurrentes` | Asientos recurrentes programados |

#### Plan de Cuentas y Centros

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/contabilidad/cuentas` | Plan de cuentas (chart of accounts) |
| `/(dashboard)/contabilidad/centros-costo` | Centros de costo |
| `/(dashboard)/contabilidad/presupuestos` | Presupuestos |

#### Activos Fijos

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/contabilidad/activos-fijos` | Lista de activos fijos |
| `/(dashboard)/contabilidad/activos-fijos/categorias` | Categorias de activos |
| `/(dashboard)/contabilidad/activos-fijos/depreciacion` | Calculo y registro de depreciacion |
| `/(dashboard)/contabilidad/activos-fijos/reportes` | Reportes de activos fijos |

#### Fiscal

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/contabilidad/fiscal/libros` | Libros fiscales (compras, ventas, resumen) |
| `/(dashboard)/contabilidad/fiscal/declaraciones` | Declaraciones fiscales |
| `/(dashboard)/contabilidad/fiscal/retenciones` | Retenciones (IVA, ISLR, etc.) |

#### Otros

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/contabilidad/conciliacion` | Conciliacion bancaria-contable |
| `/(dashboard)/contabilidad/cierre` | Cierre contable de periodo |
| `/(dashboard)/contabilidad/reportes` | Reportes contables (balance, PyG, mayor, etc.) |

### Bancos (dentro del Shell)

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/bancos` | Dashboard de bancos |
| `/(dashboard)/bancos/entidades` | Entidades bancarias |
| `/(dashboard)/bancos/cuentas` | Cuentas bancarias |
| `/(dashboard)/bancos/movimientos/generar` | Generar movimientos bancarios |
| `/(dashboard)/bancos/caja-chica` | Caja chica |
| `/(dashboard)/bancos/conciliacion` | Conciliacion bancaria |
| `/(dashboard)/bancos/reportes` | Reportes bancarios |

### Compras (dentro del Shell)

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/compras` | Dashboard de compras |
| `/(dashboard)/compras/lista` | Lista de facturas de compra |
| `/(dashboard)/compras/new` | Nueva factura de compra |
| `/(dashboard)/compras/[numFact]` | Detalle de factura (ruta dinamica) |
| `/(dashboard)/cuentas-por-pagar` | Cuentas por pagar |
| `/(dashboard)/cuentas-por-pagar/new` | Nuevo registro CxP |
| `/(dashboard)/pagos` | Pagos a proveedores |
| `/(dashboard)/proveedores` | Lista de proveedores |
| `/(dashboard)/proveedores/new` | Nuevo proveedor |
| `/(dashboard)/proveedores/[codigo]` | Detalle de proveedor (ruta dinamica) |

### Inventario (dentro del Shell)

#### Articulos

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/inventario` | Dashboard de inventario |
| `/(dashboard)/inventario/articulos` | Lista de articulos |
| `/(dashboard)/inventario/articulos/new` | Nuevo articulo |
| `/(dashboard)/inventario/articulos/[codigo]` | Detalle de articulo (vista) |
| `/(dashboard)/inventario/articulos/[codigo]/edit` | Editar articulo |

#### Operaciones

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/inventario/ajuste` | Ajustes de inventario |
| `/(dashboard)/inventario/movimientos` | Movimientos de inventario |
| `/(dashboard)/inventario/traslados` | Traslados entre almacenes |
| `/(dashboard)/inventario/etiquetas` | Impresion de etiquetas |
| `/(dashboard)/inventario/almacenes` | Gestion de almacenes |

#### Clasificaciones

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/inventario/categorias` | Categorias de articulos |
| `/(dashboard)/inventario/marcas` | Marcas |
| `/(dashboard)/inventario/clases` | Clases |
| `/(dashboard)/inventario/tipos` | Tipos |
| `/(dashboard)/inventario/lineas` | Lineas de producto |
| `/(dashboard)/inventario/unidades` | Unidades de medida |

#### Reportes

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/inventario/reportes/libro` | Libro de inventario |

### Nomina (dentro del Shell)

#### Principal

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/nomina` | Dashboard de nomina |
| `/(dashboard)/nomina/empleados` | Lista de empleados |
| `/(dashboard)/nomina/nominas` | Nominas procesadas |
| `/(dashboard)/nomina/conceptos` | Conceptos de nomina (asignaciones/deducciones) |
| `/(dashboard)/nomina/constantes` | Constantes de calculo |
| `/(dashboard)/nomina/feriados` | Calendario de feriados |

#### Documentos y Procesamiento

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/nomina/documentos` | Documentos de nomina |
| `/(dashboard)/nomina/procesar` | Procesar nomina |

#### Liquidaciones

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/nomina/liquidaciones` | Lista de liquidaciones |
| `/(dashboard)/nomina/liquidaciones/nueva` | Nueva liquidacion |

#### Vacaciones

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/nomina/vacaciones` | Panel de vacaciones |
| `/(dashboard)/nomina/vacaciones/solicitar` | Solicitar vacaciones |
| `/(dashboard)/nomina/vacaciones/solicitudes` | Lista de solicitudes |
| `/(dashboard)/nomina/vacaciones/procesar` | Procesar solicitudes |

#### Beneficios Sociales

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/nomina/caja-ahorro` | Caja de ahorro |
| `/(dashboard)/nomina/fideicomiso` | Fideicomiso / prestaciones |
| `/(dashboard)/nomina/utilidades` | Utilidades |

#### Salud y Capacitacion

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/nomina/salud-ocupacional` | Salud ocupacional |
| `/(dashboard)/nomina/examenes-medicos` | Examenes medicos |
| `/(dashboard)/nomina/ordenes-medicas` | Ordenes medicas |
| `/(dashboard)/nomina/capacitacion` | Planes de capacitacion |
| `/(dashboard)/nomina/comites` | Comites (SST, etc.) |
| `/(dashboard)/nomina/obligaciones` | Obligaciones legales |

#### Reportes

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/nomina/reportes` | Reportes de nomina |

### Configuracion (dentro del Shell)

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/configuracion` | Configuracion global de la empresa |
| `/(dashboard)/configuracion/formas-pago` | Formas de pago |
| `/(dashboard)/configuracion/usuarios` | Gestion de usuarios |
| `/(dashboard)/configuracion/empleados` | Configuracion de empleados |
| `/(dashboard)/maestros/[slug]` | Tablas maestras genericas (ruta dinamica por slug) |

### Reportes y Report Studio (dentro del Shell)

| Ruta | Descripcion |
| --- | --- |
| `/(dashboard)/reportes` | Catalogo de reportes (mis reportes + publicos) |
| `/(dashboard)/reportes/designer` | Disenador de reportes |
| `/(dashboard)/reportes/wizard` | Asistente para crear reportes |
| `/(dashboard)/reportes/preview` | Previsualizacion de reporte |
| `/(dashboard)/report-studio` | Report Studio integrado |
| `/(dashboard)/report-studio/designer` | Disenador dentro de Report Studio |
| `/(dashboard)/report-studio/wizard` | Wizard dentro de Report Studio |
| `/(dashboard)/report-studio/preview` | Preview dentro de Report Studio |
| `/(dashboard)/studio-designer` | Disenador standalone |
| `/(dashboard)/studio-designer/preview` | Preview del disenador standalone |
| `/(dashboard)/studio-designer/wizard` | Wizard del disenador standalone |

### Onboarding - Route Group `(onboarding)`

Flujo de aprovisionamiento de nuevo tenant. Rutas secuenciales.

| Ruta | Descripcion |
| --- | --- |
| `/(onboarding)/onboarding/[token]` | Inicio de onboarding (token unico) |
| `/(onboarding)/onboarding/provider` | Seleccion de proveedor/plan |
| `/(onboarding)/onboarding/credentials` | Configurar credenciales |
| `/(onboarding)/onboarding/configure` | Configuracion inicial de empresa |
| `/(onboarding)/onboarding/deploy` | Deploy del entorno |
| `/(onboarding)/onboarding/complete` | Onboarding completado |

---

## Contabilidad (3001)

App standalone de contabilidad. Mismas rutas que en el shell pero montadas en la raiz (sin prefijo `/(dashboard)/contabilidad`).

| Ruta standalone | Equivalencia en Shell |
| --- | --- |
| `/` | `/(dashboard)/contabilidad` |
| `/asientos` | `/(dashboard)/contabilidad/asientos` |
| `/asientos/nuevo` | `/(dashboard)/contabilidad/asientos/nuevo` |
| `/recurrentes` | `/(dashboard)/contabilidad/recurrentes` |
| `/cuentas` | `/(dashboard)/contabilidad/cuentas` |
| `/centros-costo` | `/(dashboard)/contabilidad/centros-costo` |
| `/presupuestos` | `/(dashboard)/contabilidad/presupuestos` |
| `/activos-fijos` | `/(dashboard)/contabilidad/activos-fijos` |
| `/activos-fijos/categorias` | `/(dashboard)/contabilidad/activos-fijos/categorias` |
| `/activos-fijos/depreciacion` | `/(dashboard)/contabilidad/activos-fijos/depreciacion` |
| `/activos-fijos/reportes` | `/(dashboard)/contabilidad/activos-fijos/reportes` |
| `/fiscal/libros` | `/(dashboard)/contabilidad/fiscal/libros` |
| `/fiscal/declaraciones` | `/(dashboard)/contabilidad/fiscal/declaraciones` |
| `/fiscal/retenciones` | `/(dashboard)/contabilidad/fiscal/retenciones` |
| `/conciliacion` | `/(dashboard)/contabilidad/conciliacion` |
| `/cierre` | `/(dashboard)/contabilidad/cierre` |
| `/reportes` | `/(dashboard)/contabilidad/reportes` |

---

## POS (3002)

Punto de venta. Interfaz optimizada para operacion rapida en mostrador.

| Ruta | Descripcion |
| --- | --- |
| `/` | Pantalla principal del punto de venta |
| `/facturacion` | Modulo de facturacion |
| `/reportes` | Reportes del POS |
| `/configuracion` | Configuracion del punto de venta |
| `/cierre-caja` | Cierre de caja / arqueo |
| `/correlativos-fiscales` | Gestion de correlativos fiscales |
| `/fiscal` | Configuracion fiscal (impresora fiscal, etc.) |

---

## Nomina (3003)

App standalone de nomina. Mismas rutas que en el shell montadas en la raiz.

| Ruta standalone | Equivalencia en Shell |
| --- | --- |
| `/` | `/(dashboard)/nomina` |
| `/empleados` | `/(dashboard)/nomina/empleados` |
| `/nominas` | `/(dashboard)/nomina/nominas` |
| `/conceptos` | `/(dashboard)/nomina/conceptos` |
| `/constantes` | `/(dashboard)/nomina/constantes` |
| `/feriados` | `/(dashboard)/nomina/feriados` |
| `/documentos` | `/(dashboard)/nomina/documentos` |
| `/procesar` | `/(dashboard)/nomina/procesar` |
| `/liquidaciones` | `/(dashboard)/nomina/liquidaciones` |
| `/liquidaciones/nueva` | `/(dashboard)/nomina/liquidaciones/nueva` |
| `/vacaciones` | `/(dashboard)/nomina/vacaciones` |
| `/vacaciones/solicitar` | `/(dashboard)/nomina/vacaciones/solicitar` |
| `/vacaciones/solicitudes` | `/(dashboard)/nomina/vacaciones/solicitudes` |
| `/vacaciones/procesar` | `/(dashboard)/nomina/vacaciones/procesar` |
| `/caja-ahorro` | `/(dashboard)/nomina/caja-ahorro` |
| `/fideicomiso` | `/(dashboard)/nomina/fideicomiso` |
| `/utilidades` | `/(dashboard)/nomina/utilidades` |
| `/salud-ocupacional` | `/(dashboard)/nomina/salud-ocupacional` |
| `/examenes-medicos` | `/(dashboard)/nomina/examenes-medicos` |
| `/ordenes-medicas` | `/(dashboard)/nomina/ordenes-medicas` |
| `/capacitacion` | `/(dashboard)/nomina/capacitacion` |
| `/comites` | `/(dashboard)/nomina/comites` |
| `/obligaciones` | `/(dashboard)/nomina/obligaciones` |
| `/reportes` | `/(dashboard)/nomina/reportes` |

---

## Bancos (3004)

App standalone de bancos. Mismas rutas que en el shell montadas en la raiz.

| Ruta standalone | Equivalencia en Shell |
| --- | --- |
| `/` | `/(dashboard)/bancos` |
| `/entidades` | `/(dashboard)/bancos/entidades` |
| `/cuentas` | `/(dashboard)/bancos/cuentas` |
| `/movimientos/generar` | `/(dashboard)/bancos/movimientos/generar` |
| `/caja-chica` | `/(dashboard)/bancos/caja-chica` |
| `/conciliacion` | `/(dashboard)/bancos/conciliacion` |
| `/reportes` | `/(dashboard)/bancos/reportes` |

---

## Inventario (3005)

App standalone de inventario. Incluye rutas adicionales (lotes, seriales, WMS) no disponibles en el shell.

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de inventario |
| `/articulos` | Lista de articulos |
| `/articulos/new` | Nuevo articulo |
| `/articulos/[codigo]` | Detalle de articulo |
| `/articulos/[codigo]/edit` | Editar articulo |
| `/ajuste` | Ajustes de inventario |
| `/movimientos` | Movimientos |
| `/traslados` | Traslados entre almacenes |
| `/etiquetas` | Impresion de etiquetas |
| `/almacenes` | Gestion de almacenes |
| `/categorias` | Categorias |
| `/marcas` | Marcas |
| `/clases` | Clases |
| `/tipos` | Tipos |
| `/lineas` | Lineas de producto |
| `/unidades` | Unidades de medida |
| `/reportes/libro` | Libro de inventario |
| `/lotes` | Gestion de lotes (solo standalone) |
| `/seriales` | Gestion de numeros seriales (solo standalone) |
| `/almacenes-wms` | Warehouse Management System avanzado (solo standalone) |

---

## Ventas (3006)

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de ventas |
| `/facturas` | Lista de facturas de venta |
| `/abonos` | Notas de credito / abonos |
| `/articulos` | Articulos (vista desde ventas) |
| `/clientes` | Lista de clientes |
| `/cxc` | Cuentas por cobrar |
| `/cxp` | Cuentas por pagar (vista cruzada) |
| `/inventario` | Inventario (vista desde ventas) |
| `/pedidos-ecommerce` | Pedidos recibidos desde ecommerce |
| `/proveedores` | Proveedores (vista cruzada) |
| `/reportes` | Reportes de ventas |

---

## Compras (3007)

App standalone de compras. Mismas rutas que en el shell montadas en la raiz.

| Ruta standalone | Equivalencia en Shell |
| --- | --- |
| `/` | `/(dashboard)/compras` |
| `/lista` | `/(dashboard)/compras/lista` |
| `/new` | `/(dashboard)/compras/new` |
| `/[numFact]` | `/(dashboard)/compras/[numFact]` |
| `/cuentas-por-pagar` | `/(dashboard)/cuentas-por-pagar` |
| `/cuentas-por-pagar/new` | `/(dashboard)/cuentas-por-pagar/new` |
| `/pagos` | `/(dashboard)/pagos` |
| `/proveedores` | `/(dashboard)/proveedores` |
| `/proveedores/new` | `/(dashboard)/proveedores/new` |
| `/proveedores/[codigo]` | `/(dashboard)/proveedores/[codigo]` |

---

## Restaurante (3008)

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de restaurante / vista operativa |
| `/admin/ambientes` | Gestion de ambientes (salones, terrazas, barras) |
| `/admin/compras` | Compras del restaurante |
| `/admin/configuracion` | Configuracion del restaurante |
| `/admin/insumos` | Gestion de insumos |
| `/admin/productos` | Productos del menu |
| `/admin/recetas` | Recetas y fichas tecnicas |
| `/cocina` | Pantalla de cocina (KDS) |
| `/fiscal` | Configuracion fiscal |
| `/reportes` | Reportes del restaurante |

---

## Ecommerce (3009)

Tienda publica orientada al consumidor final. Incluye rutas de autenticacion propias.

| Ruta | Descripcion |
| --- | --- |
| `/` | Home de la tienda |
| `/productos` | Catalogo de productos |
| `/productos/[code]` | Detalle de producto (ruta dinamica por codigo) |
| `/carrito` | Carrito de compras |
| `/checkout` | Proceso de pago |
| `/confirmacion/[token]` | Confirmacion de pedido (ruta dinamica) |
| `/login` | Login de cliente |
| `/registro` | Registro de cliente |
| `/pedidos` | Mis pedidos |
| `/devoluciones` | Devoluciones |
| `/afiliados` | Programa de afiliados |
| `/acerca` | Acerca de la tienda |
| `/contacto` | Formulario de contacto |
| `/prensa` | Sala de prensa |
| `/trabaja-con-nosotros` | Ofertas de empleo |
| `/vende` | Portal para vendedores / marketplace |
| `/reportes` | Reportes del ecommerce |

---

## Auditoria (3010)

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de auditoria |
| `/bitacora` | Bitacora de eventos del sistema |
| `/sesiones` | Sesiones activas e historicas |
| `/usuarios` | Actividad por usuario |
| `/alertas` | Alertas de seguridad |
| `/alertas/configuracion` | Configuracion de reglas de alertas |
| `/analytics` | Analitica de uso y seguridad |
| `/fiscal-records` | Registros fiscales auditables |
| `/fiscal` | Auditoria fiscal |
| `/reportes` | Reportes de auditoria |

---

## Logistica (3011)

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de logistica |
| `/albaranes` | Gestion de albaranes (notas de entrega) |
| `/recepciones` | Recepciones de mercancia |
| `/conductores` | Gestion de conductores |
| `/transportistas` | Empresas transportistas |
| `/devoluciones` | Devoluciones logisticas |
| `/reportes` | Reportes de logistica |

---

## CRM (3012)

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de CRM |
| `/leads` | Lista de leads / prospectos |
| `/leads/[id]` | Detalle de lead (ruta dinamica) |
| `/pipeline` | Pipeline visual de ventas (Kanban) |
| `/actividades` | Actividades programadas (llamadas, visitas, etc.) |
| `/automatizaciones` | Reglas de automatizacion |
| `/timeline` | Linea de tiempo de interacciones |
| `/configuracion` | Configuracion del CRM |
| `/reportes` | Reportes de CRM |

---

## Manufactura (3013)

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de manufactura |
| `/bom` | Bills of Materials (listas de materiales) |
| `/ordenes` | Ordenes de produccion |
| `/centros-trabajo` | Centros de trabajo |
| `/rutas` | Rutas de produccion |
| `/reportes` | Reportes de manufactura |

---

## Flota (3014)

| Ruta | Descripcion |
| --- | --- |
| `/` | Dashboard de flota |
| `/vehiculos` | Gestion de vehiculos |
| `/combustible` | Registro de cargas de combustible |
| `/mantenimiento` | Mantenimiento preventivo y correctivo |
| `/viajes` | Control de viajes |
| `/reportes` | Reportes de flota |

---

## Shipping (3015)

Portal de paqueteria con autenticacion propia. Schema `logistics` en base de datos.

| Ruta | Descripcion |
| --- | --- |
| `/` | Landing del portal de shipping |
| `/login` | Login del portal |
| `/registro` | Registro de nuevo usuario shipping |
| `/dashboard` | Dashboard post-login |
| `/envios` | Lista de envios |
| `/envios/nuevo` | Crear nuevo envio |
| `/envios/[id]` | Detalle de envio (ruta dinamica) |
| `/rastreo` | Busqueda de rastreo |
| `/rastreo/[tracking]` | Rastreo por numero de guia (ruta dinamica) |
| `/perfil` | Perfil del usuario shipping |
| `/reportes` | Reportes de envios |

---

## Report Studio (3017)

App standalone del disenador de reportes. Usa `@zentto/shared-reports` como motor.

| Ruta | Descripcion |
| --- | --- |
| `/` | Catalogo de reportes (guardados + publicos) |
| `/designer` | Disenador visual de reportes |
| `/wizard` | Asistente guiado para crear reportes |
| `/preview` | Previsualizacion de reportes |

---

## Patrones de Navegacion y Sidebar

### Estructura del Sidebar (DashboardLayout)

El sidebar del shell se organiza en secciones colapsables:

```
Inicio
  - Dashboard
  - Aplicaciones
  - Addons

Modulos
  - Contabilidad  (expandible: asientos, cuentas, fiscal, ...)
  - Bancos         (expandible: entidades, cuentas, caja chica, ...)
  - Compras        (expandible: lista, proveedores, CxP, ...)
  - Inventario     (expandible: articulos, almacenes, ajustes, ...)
  - Nomina         (expandible: empleados, nominas, vacaciones, ...)
  - Ventas
  - POS
  - Restaurante
  - Ecommerce
  - CRM
  - Logistica
  - Manufactura
  - Flota
  - Shipping

Herramientas
  - Reportes
  - Report Studio
  - Auditoria

Administracion
  - Configuracion
  - Maestros
  - Backoffice
  - Usuarios

Cuenta
  - Perfil
  - Pagos
  - Notificaciones
  - Soporte
  - Docs
  - Info
```

### Sidebar en Apps Standalone

Cada app standalone tiene su propio sidebar reducido, mostrando solo las rutas relevantes a ese modulo. Ejemplo para Contabilidad (3001):

```
Dashboard
Asientos
  - Lista
  - Nuevo
  - Recurrentes
Plan de Cuentas
Centros de Costo
Presupuestos
Activos Fijos
  - Categorias
  - Depreciacion
  - Reportes
Fiscal
  - Libros
  - Declaraciones
  - Retenciones
Conciliacion
Cierre
Reportes
```

---

## Convenciones de Rutas

### Route Groups de Next.js

Los route groups (entre parentesis) no generan segmentos de URL:

| Route Group | Proposito |
| --- | --- |
| `(dashboard)` | Layout con sidebar, topbar, autenticacion requerida |
| `(onboarding)` | Flujo de onboarding sin sidebar |

La URL final de `/(dashboard)/contabilidad/asientos` es simplemente `/contabilidad/asientos`.

### Rutas Dinamicas

| Patron | Uso | Ejemplo |
| --- | --- | --- |
| `[id]` | ID numerico o UUID | `/leads/[id]`, `/envios/[id]` |
| `[codigo]` | Codigo alfanumerico de entidad | `/articulos/[codigo]`, `/proveedores/[codigo]` |
| `[numFact]` | Numero de factura | `/compras/[numFact]` |
| `[token]` | Token de sesion/verificacion | `/onboarding/[token]`, `/confirmacion/[token]` |
| `[number]` | Numero de ticket | `/soporte/[number]` |
| `[tracking]` | Numero de guia de rastreo | `/rastreo/[tracking]` |
| `[slug]` | Slug generico para tablas maestras | `/maestros/[slug]` |

### Patron CRUD Estandar

La mayoria de las entidades siguen este patron:

```
/entidad           -> Lista (ZenttoDataGrid)
/entidad/new       -> Formulario de creacion
/entidad/[id]      -> Detalle / vista de lectura
/entidad/[id]/edit -> Formulario de edicion
```

### Patron de Reportes

Cada modulo expone su ruta `/reportes` que lista los reportes disponibles para ese contexto. Los reportes se renderizan via `@zentto/shared-reports`.

### Regla de Componentes de Tabla

**NUNCA** usar `<table>` HTML nativo. Siempre usar `<ZenttoDataGrid>` de `@zentto/shared-ui` para cualquier listado tabular.

---

## Referencia Rapida - Indice de Rutas por Modulo

| Modulo | Total rutas | Ruta raiz |
| --- | --- | --- |
| Shell (auth) | 5 | `/authentication/login` |
| Shell (publicas) | 3 | `/pricing` |
| Shell (dashboard) | 10 | `/(dashboard)` |
| Shell (soporte) | 3 | `/(dashboard)/soporte` |
| Contabilidad | 17 | `/(dashboard)/contabilidad` |
| Bancos | 7 | `/(dashboard)/bancos` |
| Compras | 10 | `/(dashboard)/compras` |
| Inventario | 17 | `/(dashboard)/inventario` |
| Nomina | 25 | `/(dashboard)/nomina` |
| Configuracion | 4 | `/(dashboard)/configuracion` |
| Reportes/Studio | 10 | `/(dashboard)/reportes` |
| Onboarding | 6 | `/(onboarding)/onboarding/[token]` |
| POS | 7 | `/` (3002) |
| Ventas | 11 | `/` (3006) |
| Restaurante | 10 | `/` (3008) |
| Ecommerce | 17 | `/` (3009) |
| Auditoria | 10 | `/` (3010) |
| Logistica | 7 | `/` (3011) |
| CRM | 9 | `/` (3012) |
| Manufactura | 6 | `/` (3013) |
| Flota | 6 | `/` (3014) |
| Shipping | 12 | `/` (3015) |
| Report Studio | 4 | `/` (3017) |

---

*Documento generado como referencia para desarrollo y agentes IA del proyecto Zentto.*
