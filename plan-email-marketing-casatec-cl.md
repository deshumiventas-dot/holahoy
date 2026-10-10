# Casatec Chile (casatec.cl) — Cambios aplicados + Plan de Email Marketing

- **Marca:** Casatec Spa · https://casatec.cl · WooCommerce · CLP · America/Santiago
- **Remitente estándar aplicado:** **Casatec Chile** (`ventas@casatec.cl`)
- **Fecha:** 2026-10-10
- **Enfoque:** skills de *email marketing* (secuencias) y *marketing psychology*
  (Jobs-to-be-Done, loss aversion, Zeigarnik, prueba social, escasez, reciprocidad).

---

## 1. Cambios aplicados en vivo (hechos ✅)

Todo esto ya está aplicado en la cuenta. Es reversible (basta reactivar/renombrar).

### 1.1 Consolidación de flujos — "dejar solo los que suman"

**Estado final ACTIVOS (7):**

| Flujo | Disparador | Correos | Overlap | Notas |
|-------|-----------|:------:|:------:|-------|
| **Bienvenida (4 correos)** | Suscripción (form + 1ª vez) | 4 | ✅ | Activado en reemplazo de la de 1 correo |
| **Carro abandonado 🛒** | Añadió al carrito (30 min) | 3 | ✅ | Remitente unificado |
| **Checkout abandonado** | Inició checkout (20 min) | 3 | ✅ | **Preheader roto corregido** |
| **Abandono de producto (2 correos)** | Vio producto (en stock) | 2 | ✅ | Activado en reemplazo de la de 1 correo |
| **Customer Reactivation** | Realizó pedido | 3 | ✅ | Overlap actualizado |
| **Confirmación de compra 💰** | Pagó pedido | 1 | — | Transaccional (sin cambios) |
| **Post-compra (consejos, reseña, venta cruzada)** | Pedido preparado | 2 | — | **Activado** (capacidad nueva) |

**Estado final ARCHIVADOS / desactivados (5):**

| Flujo | Motivo |
|-------|--------|
| `[OBSOLETO] Welcome No.2 (1 correo)` | Reemplazado por Bienvenida (4 correos) |
| `[OBSOLETO] Bienvenida (1 correo)` | Versión antigua |
| `[OBSOLETO] Product Abandonment (1 correo)` | Reemplazado por versión de 2 correos |
| `[OBSOLETO] Reactivación 180 días` | Duplicaba Customer Reactivation y sin overlap |
| `Preventa GTD-250` | Queda **desactivado**: actívalo solo si la preventa está vigente |

> Al desactivar, se usó "conservar contactos" para que nadie quede cortado a
> mitad de secuencia. Nada se eliminó: los OBSOLETO se pueden recuperar.

### 1.2 Que ninguno interrumpa a otro (overlap)

Reconfiguré el `overlapLimiter` (modo *recentlyIn*, 7 días) de los 5 flujos
promocionales para que cada uno **excluya a quien estuvo en los otros 4**:
bienvenida ⇄ carro ⇄ checkout ⇄ abandono de producto ⇄ reactivación. Antes los
limitadores apuntaban a la bienvenida **antigua desactivada**, así que no
protegían nada. Transaccionales (confirmación, post-compra) quedan fuera a
propósito (no deben suprimirse).

### 1.3 Marca y copy

- **Remitente unificado a "Casatec Chile"** en todos los correos de todos los
  flujos activos (antes había "Casatec Spa", "Casatec", "Casatec Chile 🇨🇱").
- **Preheader corregido** en el 3.er correo de *Checkout abandonado* (estaba roto:
  *"…los artículos que te gustarones de que se acaben"* → ahora *"¡Última llamada!
  Un 5% de descuento en los artículos que te gustaron, antes de que se agoten ⌛"*).

### 1.4 Revisión de diseño de los borradores de "ayer"

Revisé el contenido real (no solo la config). Veredicto: **bien construidos**.
Ejemplo (correo 2 de bienvenida): logo, colores de marca (durazno #E9A673 / azul
#94C6FF), y una **matriz de recomendación por m²** muy buena
(Drybox → SmartDry Pro WiFi → GTD-250 Plus WiFi → ULTRADRY500) con CTA a test y
comparativa. Copy claro y con criterio de Jobs-to-be-Done ("¿cuál me sirve?").

> Lo único que no puedo validar 100% por API es el render visual final (imágenes,
> espaciados en móvil). **Recomendación:** dale una pasada visual en el editor
> con "Vista previa" antes de abrir la llave de tráfico (el formulario). Links de
> cada flujo en la sección 4.

---

## 2. ⚠️ Tu pregunta del formulario — hallazgo CRÍTICO

**¿Hay que crear un formulario para activar la bienvenida?** → **No hace falta
crearlo: ya tienes DOS, pero los dos están DESACTIVADOS.** Hoy la bienvenida se
dispara con *"Signup form"*, así que **sin un formulario activo la bienvenida (por
muy buena que sea) no recibe entradas**. Es, literalmente, la puerta de entrada
de leads cerrada.

Formularios existentes (ambos popup, ambos desactivados):

| Form | Oferta / gancho | Tag | Estado |
|------|-----------------|-----|--------|
| `6a81dce3…` | **Guía gratis** del calor (multi-paso) | `guía` | ❌ desactivado (08-oct) |
| `69642ab1…` | **5% dto.** (código `welcome5`) | `welcome_discount` | ❌ desactivado (abr) |

**Recomendación (experto):** activar **uno solo** para no competir consigo mismo.
- La **Bienvenida (4 correos)** ya está pensada como *guía → cómo elegir → prueba
  social → 5% que vence en 48 h*. El par natural es el **form de la Guía**
  (`6a81dce3…`), que además etiqueta `guía` igual que el flujo.
- Psicología: el gancho de **valor primero** (guía) baja la fricción de entrega el
  email (reciprocidad + zero-price effect) mejor que pedir el dato "a cambio de
  5%"; el 5% llega después, como cierre con urgencia (loss aversion) en el correo 4.

**No lo activé yo** porque enciende un popup en tu web (decisión de UX/tráfico que
es tuya). Dime "enciende el form de la guía" y lo dejo activo en 1 paso.

---

## 3. Nuevos flujos para VENDER (priorizados)

Pensados para tu negocio: ticket medio-alto, producto **durable** (no consumible),
estacional (humedad en invierno / calor en verano), WooCommerce.

### 🥇 Prioridad 1 — "Vuelve a estar disponible" (Back in Stock)
- **No lo tienes** y es de los de mayor intención de compra.
- Disparador: `product back in stock` (WooCommerce lo soporta).
- Psicología: escasez + loss aversion. Quien lo pidió ya quería comprar.

### 🥈 Prioridad 2 — Recuperación de cupón sin usar
- Para quien entró por el form, recibió el 5% y **no compró**: recordatorio antes
  de que expire el cupón.
- Psicología: Zeigarnik (bucle abierto) + urgencia real + mental accounting
  ("ya tienes 5%, no lo pierdas").

### 🥉 Prioridad 3 — Recompra / Winback estacional (6–9 meses)
- Tu *Customer Reactivation* dispara a **30 días** de la compra, que para un equipo
  durable es **demasiado pronto** ("te extrañamos" a quien recién compró su
  deshumidificador suena raro). Propongo:
  - Reencuadrar la de 30 días como **satisfacción + accesorios** (no "winback"), y
  - Crear un **winback real a 6–9 meses**: mantención/filtros, Drybox para clósets,
    cambio de temporada. Psicología: peak-end + relevancia estacional + JTBD.

### Prioridad 4 — Serie de consideración para alto ticket (AC / GTD-250)
- Para quienes ven productos caros y no compran: secuencia que maneja objeciones
  (garantía, cuotas, despacho, comparativa, reseñas). Regla de 7 contactos.
- Psicología: authority + prueba social + regret aversion (garantía/devolución).

### Prioridad 5 — Cumpleaños / aniversario de compra con beneficio
- Disparador integrado `birthday` (o aniversario de 1.ª compra) con un detalle.
- Psicología: reciprocidad + peak-end; bajo esfuerzo, buen retorno.

### Extra (campañas, no flujos)
- **Newsletter estacional** (ola de calor / lluvias) y **abandono de navegación por
  categoría**. La clave para "vender de verdad" es combinar **flujos (automático)
  + campañas estacionales (manual)**, no solo flujos.

---

## 4. Decisiones abiertas para ti

1. **¿Enciendo el formulario de la Guía** (`6a81dce3…`) para que la bienvenida
   empiece a recibir leads? (o prefieres el de 5%, o ambos con reglas distintas).
2. **Reactivación a 30 días:** ¿la reencuadro a satisfacción/accesorios y creo un
   winback real a 6–9 meses?
3. **Preventa GTD-250:** ¿la preventa está vigente? Si sí, la activo.
4. **Nuevos flujos:** ¿arranco por el de **Back in Stock** (prioridad 1)?
5. **Escalera de descuento:** carrito/checkout dan 5% y bienvenida 5%. ¿Lo dejamos
   o subimos el de bienvenida a 10% como gancho de captación?

---

## Anexo — IDs y enlaces (editor Omnisend)

**Activos:**
- Bienvenida (4 correos): `6ac815c6866034cbe8b9d653` — https://app.omnisend.com/automation/edit/6ac815c6866034cbe8b9d653
- Carro abandonado 🛒: `67e227d55484c48eb8cc7742` — https://app.omnisend.com/automation/edit/67e227d55484c48eb8cc7742
- Checkout abandonado: `67df2ec5414d1dc8d28ea7ff` — https://app.omnisend.com/automation/edit/67df2ec5414d1dc8d28ea7ff
- Abandono de producto (2 correos): `6ac8185a12da75235968e893` — https://app.omnisend.com/automation/edit/6ac8185a12da75235968e893
- Customer Reactivation: `69642e0a324f040149189fed` — https://app.omnisend.com/automation/edit/69642e0a324f040149189fed
- Confirmación de compra 💰: `67e1c28e3d78a08502c46b7d` — https://app.omnisend.com/automation/edit/67e1c28e3d78a08502c46b7d
- Post-compra: `6abff4182c2ab323fbfa4b95` — https://app.omnisend.com/automation/edit/6abff4182c2ab323fbfa4b95

**Archivados/desactivados:**
- `[OBSOLETO] Welcome No.2 (1 correo)`: `6ac5b57b866034cbe8b98586`
- `[OBSOLETO] Bienvenida (1 correo)`: `6963ef22324f040149189fb3`
- `[OBSOLETO] Product Abandonment (1 correo)`: `69642bdae3e82127156fabf0`
- `[OBSOLETO] Reactivación 180 días`: `6ac818e8866034cbe8b9d66e`
- `Preventa GTD-250`: `6ac819fc12da75235968e8b0`

**Formularios (desactivados):**
- Guía gratis: `6a81dce3e080d6c53735c833`
- 5% welcome: `69642ab177cbba1bd7812f50`
