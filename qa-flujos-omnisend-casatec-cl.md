# QA de flujos y correos — Omnisend Casatec Chile

**Fecha:** 2026-10-10
**Cuenta:** Casatec Spa · casatec.cl (WooCommerce, CLP) · brandID `67df2297f8deedeb6f9ca25c`
**Alcance:** revisión en vivo de los 9 flujos activos y los 20 correos que envían (configuración + diseño + enlaces + textos).

---

## 1. Configuración de flujos — OK

Los 9 flujos están **activos (ENABLED)**, con triggers, condiciones de salida y `overlapLimiter` correctos (ningún flujo promocional interrumpe a otro). Remitente **"Casatec Chile"** en los 9.

| Flujo | ID | Estado |
|---|---|---|
| Bienvenida (4 correos) | `6ac815c6866034cbe8b9d653` | ✅ |
| Carro abandonado 🛒 | `67e227d55484c48eb8cc7742` | ✅ |
| Checkout abandonado | `67df2ec5414d1dc8d28ea7ff` | ✅ |
| Abandono de producto (2 correos) | `6ac8185a12da75235968e893` | ✅ |
| Winback / Reactivación (6 meses) | `69642e0a324f040149189fed` | ✅ (primer delay 180 d) |
| Confirmación de compra 💰 | `67e1c28e3d78a08502c46b7d` | ✅ (transaccional) |
| Post-compra | `6abff4182c2ab323fbfa4b95` | ✅ |
| Preventa GTD-250 | `6ac819fc12da75235968e8b0` | ✅ |
| Vuelve a estar disponible | `6ac99e10653f43ddf35ce858` | ✅ |

---

## 2. Errores encontrados en los correos y CORREGIDOS (aplicados en vivo, 2026-10-10)

Se corrigieron los **20 correos** (todos guardados con statusCode 200):

1. **Pie de página en inglés → traducido a español** (17 correos). Aviso de permiso + "Update preferences | Unsubscribe" → "Recibes este correo porque te suscribiste en nuestro sitio o realizaste una compra." / "Actualizar preferencias | Cancelar suscripción". (Bienvenida #1 y Back-in-Stock ya estaban en español.)
2. **Firma "Equipo Casatec Global" → "Equipo Casatec"** (Winback #1, #2, #3).
3. **Enlaces sociales unificados** en todos los correos con bloque social:
   - Facebook: `https://www.facebook.com/profile.php?id=100069125349437`
   - Instagram: `https://www.instagram.com/casatec_cl/`
   - TikTok: `https://www.tiktok.com/@casatec.cl`
   - YouTube: `https://www.youtube.com/@casatec_cl`
   - Se **eliminó el ícono de Twitter** y los enlaces vacíos (`facebook.com/`, `instagram.com/`, etc.).
4. **Typo "sin intéres" → "sin interés"** (Abandono #2 y Bienvenida #3).
5. **Dato sin confirmar suavizado:** GTD-250 "3,5 L de tanque" → "gran capacidad de tanque" (Bienvenida #2).
6. **Etiqueta de Gmail de "$3.500 DE DESCUENTO" desactivada** en Carro #1 (el flujo entrega 5%, no $3.500).
7. **Legibilidad:** texto de cuerpo (preset `paragraph`) pasado de naranja `#E9A673` a gris oscuro `#212121` en los 20 correos. Titulares (presets heading) siguen en naranja de marca.

**Nota técnica:** 5 correos traían dos secciones con el mismo `id` interno (bug heredado de la plantilla) que bloqueaba el guardado con 400; se asignó un `id` único a la sección del pie, sin cambios visibles.

---

## 3. PENDIENTE — requiere confirmación del cliente

No se pudo verificar desde el entorno (DNS de casatec.cl bloqueado). **Revisar en el navegador y avisar si alguno da 404:**

- **Facebook oficial:** se unificó en `facebook.com/profile.php?id=100069125349437`. Si la página oficial es otra, cambiarla en los correos.
- `casatec.cl/que-equipo-necesito/` (Bienvenida #2)
- `casatec.cl/que-deshumidificador-comprar/` (Bienvenida #2)
- `casatec.cl/product-category/deshumidificadores-en-chile/` (Bienvenida #3) — ⚠️ en los blogs se usó `/product-category/productos-hogar/deshumidificadores-domesticos/`; confirmar cuál es la real.
- `casatec.cl/product/kit-closet-seco-2-drybox-tabletas/` (Post-compra #2)
- PDF de la guía: `casatec.cl/wp-content/uploads/2026/10/Guia-ultradry-popup.pdf` (Bienvenida #1)
