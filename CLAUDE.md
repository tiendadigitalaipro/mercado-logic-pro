# CLAUDE.md — Agente Técnico: Mercado Logic Pro (A2K Digital Studio)

> Archivo de contexto del proyecto. Léelo antes de cualquier tarea.

---

## ROL DEL AGENTE

Eres el **Agente Técnico Senior** de Mercado Logic Pro, una app POS (Point of Sale) para mercados/tiendas de abarrotes, desarrollada por A2K Digital Studio. El archivo principal es `index.html` (monolítico, ~240 KB, ~3700 líneas). **No existen archivos externos de JS ni CSS separados** — todo está inline en un solo HTML, usando Tailwind CSS via CDN.

---

## ARQUITECTURA DEL PROYECTO

```
mercado-logic-pro/
├── index.html              — App principal (monolítico, ~3700 líneas)
├── admin-licencias.html    — Panel de administración de licencias
├── logo.jpg                — Logo del sistema
├── CLAUDE.md               — Este archivo
└── README.md               — Documentación pública
```

### Estructura interna de `index.html`

| Bloque | Líneas aprox. | Descripción |
|---|---|---|
| `<head>` + Tailwind config | 1 – 90 | Config de colores, Tailwind CDN |
| CSS personalizado | 91 – 270 | Dark mode, temas, mobile, scrollbars |
| HTML Body — Lock Screen | 251 – 262 | `#ironLockScreen` con spinner inicial |
| HTML Sidebar | 265 – 360 | Navegación lateral con nav-items |
| HTML Main / Header | 360 – 410 | Reloj, botones, header |
| Views (secciones) | 395 – 1040 | ventas, inventario, reportes, configuracion, proveedores |
| Modales HTML | 1040 – 1580 | modalPayment, modalReceipt, modalProduct, modalStock, modalUser, modalNotif, themePanel, calcPanel, modalProveedor, modalCompra |
| JAVASCRIPT | 1580+ | Todo el JS inline |
| IRON LOCK v2 | ~1600 – 1870 | Sistema de licencias |
| LICENSE_UI | ~1870 – 2100 | Pantallas de activación |
| DATA LAYER (DB) | ~2100 – 2155 | localStorage wrapper |
| POS MODULE | ~2200 – 2600 | Ventas, carrito, búsqueda |
| INV MODULE | ~2640 – 3020 | Inventario, productos |
| RPT MODULE | ~3020 – 3210 | Reportes, cierre de caja |
| CFG MODULE | ~3210 – 3390 | Configuración |
| PROV MODULE | ~3507 – 3650 | Proveedores y compras |
| KEYBOARD/EVENTS | ~3474 – 3505 | Atajos de teclado |
| INIT | ~3650+ | DOMContentLoaded |

---

## SISTEMA DE LICENCIAS — IRON LOCK v2

### Códigos DEMO disponibles (universales, sin Device ID)
- `MERCA-CWCJ-JH77-KTXX` — Demo original
- `MERCA-DEMO-2026-FREE` — Demo pública (mostrada en pantalla de login)
- `MERCA-PRBA-GRA1-2026` — Prueba gratuita alternativa

Todas expiran el **31 de Diciembre 2026**.

### Flujo de activación (CORREGIDO)
1. Usuario ingresa código → `LICENSE_UI.activate()`
2. Si exitoso, NO recarga la página (evita bug de pantalla estática)
3. Directamente muestra `showDemoScreen()` o `showProScreen()`
4. Usuario hace clic en "INICIAR SESIÓN" → `enterApp()`
5. Se elimina `#ironLockScreen` y se inicializa la app

### EMBEDDED_LICENSES
Array en `index.html` línea ~1638. Para agregar licencias PRO permanentes:
```javascript
{
    id: Date.now(),
    type: "PRO",         // o "DEMO"
    name: "NOMBRE NEGOCIO",
    phone: "",
    deviceId: "",        // vacío = universal (cualquier dispositivo)
    code: "MERCA-XXXX-XXXX-XXXX",
    createdAt: "2026-XX-XXTXX:XX:XX.000Z",
    expiresAt: null,     // null = sin expiración (PRO)
    status: "Activa"
}
```

---

## MÓDULOS DE LA APP

| ID de Vista | Módulo | JS Object |
|---|---|---|
| `#view-ventas` | Punto de Venta (F1) | `POS` |
| `#view-inventario` | Inventario (F2) | `INV` |
| `#view-reportes` | Reportes / Caja (F3) | `RPT` |
| `#view-configuracion` | Configuración (F4) | `CFG` |
| `#view-proveedores` | Proveedores (F5) | `PROV` |

---

## FUNCIONES CLAVE

| Función | Descripción |
|---|---|
| `navigateTo(view)` | Navega entre vistas |
| `POS.closeModal(id)` | Cierra modal (remueve clase .active) |
| `$('id').classList.add('active')` | Abre modal |
| `toast(msg, type)` | Muestra notificación (success/error/info) |
| `DB.get(key)` | Lee de localStorage |
| `DB.set(key, val)` | Escribe en localStorage |
| `formatMoney(n)` | Formatea a `$ X.XX` |
| `formatBs(n)` | Formatea a `Bs. X.XX` (usa tasa de cambio) |
| `getTasa()` | Lee tasa USD→Bs de configuración |

---

## REGLAS IMPORTANTES

1. **NO modificar la lógica de seguridad de IRON LOCK** (solo actualizar fechas de expiración y agregar licencias)
2. **Tailwind CSS via CDN** — no hay clases personalizadas salvo en el `<style>` del head
3. **Dark mode**: clase `dark` en `body`, sobrescrituras en CSS con `body.dark .clase`
4. **Mobile**: sidebar usa `#sidebar.mobile-open` + overlay `#sidebarOverlay`
5. **Módulos**: cada módulo es un objeto `const X = { render(), ... }` con métodos
6. **DB**: todo persiste en `localStorage` con prefijo `mercado_`

---

## REPOSITORIO GITHUB

- **Repo:** `tiendadigitalaipro/mercado-logic-pro`
- **URL:** `https://github.com/tiendadigitalaipro/mercado-logic-pro`
- **Branch:** `main`
- **Ruta local:** `C:\Users\ASUS\mercado-logic-pro\`

```bash
cd /c/Users/ASUS/mercado-logic-pro
git add index.html CLAUDE.md
git commit -m "descripción"
git push origin main
```

---

*Generado por Claude — Mercado Logic Pro — A2K Digital Studio — 2026*
