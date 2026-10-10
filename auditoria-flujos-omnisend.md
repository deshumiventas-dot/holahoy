# Auditoría de Flujos (Automations) de Omnisend — Casatec Global

- **Marca:** Casatec Global (`brandID: 69553f6a665bfca6230dbcbb`)
- **Plataforma:** Omnisend + Shopify
- **Fecha de auditoría:** 2026-10-10
- **Alcance:** 8 flujos de automatización (automations) de la cuenta
- **Método:** Lectura vía API de Omnisend de `trigger`, `exitConditions`, `settings`
  (frequency/overlap limiters, sending thresholds) y árbol de `blocks`
  (emails, delays, tags) de cada flujo.

> Nota de estado: ningún flujo fue modificado durante esta auditoría. Es un
> diagnóstico de solo lectura. Las correcciones propuestas requieren tu
> confirmación explícita antes de aplicarse sobre los flujos en vivo.

---

## 1. Resumen ejecutivo

La cuenta tiene **8 flujos**, de los cuales **6 están activos** y 2 desactivados.
La cobertura de casos de uso es buena (bienvenida, carrito, checkout, producto,
back-in-stock, reactivación), pero hay **problemas de configuración de alto
impacto en la bienvenida** y varias **inconsistencias de marca e idioma** que
conviene corregir.

Los 3 hallazgos más importantes:

1. **La única bienvenida activa ("[Restored] Welcome No.6") está mal segmentada:**
   su disparador es solo `canal = Email`, sin el filtro de "primera suscripción"
   ni "formulario de registro". Puede dispararse para resuscripciones y para
   contactos que no son suscriptores nuevos genuinos.
2. **Esa misma bienvenida no tiene `overlapLimiter`**, por lo que puede
   solaparse con los flujos de carrito/checkout/reactivación (la bienvenida
   original sí los excluía).
3. **La bienvenida activa envía un solo email** (la guía gratis) y **no tiene
   seguimiento ni incentivo de primera compra**, a diferencia de las versiones
   desactivadas que sí tenían una secuencia con descuento.

---

## 2. Inventario de flujos

| # | Flujo | Estado | Disparador | Nº emails | Limitador frecuencia | Overlap | Idioma |
|---|-------|--------|------------|-----------|----------------------|---------|--------|
| 1 | Welcome | ❌ Desactivado | Suscrito (Email + form registro + 1ª suscripción) | 2 | once | ✅ sí | es_ES |
| 2 | Welcome No.2 | ❌ Desactivado | Suscrito (Email + form registro + 1ª suscripción + form.id) | 2 | once | ❌ no | es_LA |
| 3 | [Restored] Welcome No.6 | ✅ **Activo** | Suscrito (**solo** Email) | 1 | once | ❌ **no** | es_LA |
| 4 | Abandoned Cart | ✅ Activo | Añadió producto al carrito (30 min inact.) | 3 | 1/día | ✅ sí | es_ES |
| 5 | Abandoned Checkout | ✅ Activo | Inició checkout (30 min inact.) | 3 | 1/día | ✅ sí | es_ES |
| 6 | Product Abandonment | ✅ Activo | Vio producto (en stock) | 1 | 1/semana | ✅ sí | es_ES |
| 7 | Back in Stock | ✅ Activo | Producto vuelve a stock | 1 | 1/semana | ❌ no | es_ES |
| 8 | Customer Reactivation | ✅ Activo | Realizó pedido (Shopify) | 3 | 1/180 días | ✅ sí | es_ES |

Todos los flujos usan `exitConditions` con `placed order` (y el carrito también
con `started checkout`), lo cual es correcto: el contacto sale del flujo al
convertir.

---

## 3. Hallazgos priorizados

### 🔴 CRÍTICO

**C1 — La bienvenida activa dispara para cualquier suscripción de Email, no solo nuevos suscriptores**
- Flujo: `[Restored] Welcome No.6` (`6ac5ab23866034cbe8b9850c`)
- El disparador solo filtra `channel_subscribed_to = Email`. Le faltan los
  filtros que sí tienen las versiones desactivadas:
  `first_subscription = true` y `subscription_method = Signup form`.
- **Riesgo:** envía "bienvenida / guía gratis" a contactos que se resuscriben,
  que son reactivados, o que entran por otros caminos (import, checkout, etc.),
  no solo a quienes se registran por primera vez. Experiencia pobre para
  clientes existentes y métricas de bienvenida distorsionadas.
- **Recomendación:** añadir al disparador `first_subscription = true` (y, si se
  quiere acotar a la captación por formulario, `subscription_method = Signup form`).

**C2 — La bienvenida activa no excluye a quienes están en flujos de e-commerce (sin `overlapLimiter`)**
- Flujo: `[Restored] Welcome No.6`.
- La bienvenida original (`Welcome`) excluía a contactos que hubieran estado en
  los 7 días previos en carrito, checkout, producto y reactivación. La versión
  restaurada y activa **no tiene ese `overlapLimiter`**.
- **Riesgo:** un nuevo suscriptor que además navega/abandona carrito recibe
  bienvenida + abandono apilados el mismo día.
- **Recomendación:** replicar el `overlapLimiter` de la bienvenida original
  (`mode: recentlyIn`, `withinDays: 7`) contra los flujos de carrito, checkout,
  producto y reactivación.

### 🟠 ALTO

**A1 — La bienvenida activa es de un solo email, sin nurture ni incentivo de compra**
- `[Restored] Welcome No.6` solo envía la guía gratuita (más un `addTag: "guia"`).
  No hay segundo correo ni incentivo de primera compra.
- Las versiones desactivadas sí tenían una secuencia de 2 emails (`Welcome No.2`
  incluía un **10% de descuento** en la primera compra).
- **Recomendación:** extender la bienvenida a 2–3 correos
  (guía → beneficio/propuesta de valor → incentivo de primera compra),
  reutilizando el contenido ya redactado en las versiones desactivadas.

**A2 — Tres flujos de bienvenida conviviendo generan confusión y riesgo operativo**
- `Welcome`, `Welcome No.2` y `[Restored] Welcome No.6` cubren el mismo caso de
  uso. Solo uno está activo, pero mantener tres aumenta el riesgo de activar el
  equivocado y dificulta el mantenimiento.
- **Recomendación:** consolidar en **una** bienvenida canónica (la correctamente
  segmentada y con secuencia completa) y archivar/eliminar las demás, o
  renombrarlas claramente (p. ej. `[OBSOLETO] ...`).

### 🟡 MEDIO

**M1 — Contenido en inglés dentro de un flujo en español (`Welcome No.2`)**
- El 2.º email tiene asunto `"Welcome to [[account.name]]"` (inglés) y preheader
  en inglés (`"Here's a 10% discount off your first order..."`), en un flujo
  `es_LA`. Además el asunto deja una etiqueta de fusión sin renderizar visible.
- **Riesgo:** si este flujo se activa, enviaría inglés a una audiencia
  hispanohablante. **No activar sin traducir primero.**

**M2 — Inconsistencia de idioma/locale entre flujos**
- Las bienvenidas usan `es_LA`; todos los flujos de e-commerce usan `es_ES`
  (español de España). Casatec apunta a Chile/LatAm (incluso un remitente dice
  "Equipo Casatec Chile").
- **Recomendación:** estandarizar en `es_LA` (o `es_CL`) en todos los flujos
  para coherencia de tono y formato con la audiencia.

**M3 — Nombre del remitente inconsistente (incluso dentro del mismo flujo)**
- Variantes detectadas: `"Casatec"`, `"Casatec Global, envíos internacionales."`,
  `"Equipo Casatec Global"`, `"Equipo Casatec Chile"`. En `Welcome No.2` los dos
  correos usan remitentes distintos entre sí.
- **Riesgo:** menor reconocimiento de marca y posible impacto en entregabilidad/
  reputación del remitente.
- **Recomendación:** definir **un** nombre de remitente estándar y aplicarlo a
  todos los flujos y correos.

### 🟢 BAJO / Observaciones

- **B1 — `Product Abandonment` (vio producto) es de alto volumen.** Dispara ante
  cualquier vista de producto en stock. Está acotado por frecuencia 1/semana y
  overlap, lo cual es correcto; considerar si el volumen/presión es el deseado.
- **B2 — `Back in Stock` sin `overlapLimiter`.** Aceptable por ser aviso de tipo
  transaccional, pero puede apilarse con otros flujos. Prioridad baja.
- **B3 — Umbral de envío `nonSubscribed`** en carrito, checkout, producto,
  back-in-stock y reactivación. Es el comportamiento estándar de Omnisend para
  alcanzar a contactos no suscritos que generaron un evento comercial; conviene
  validar que cumple el consentimiento/normativa aplicable a tu audiencia.
- **B4 — Descuentos.** Carrito/checkout ofrecen 5%; la bienvenida (desactivada)
  ofrecía 10%. Es una decisión de estrategia, no un defecto; revisar coherencia
  de la escalera de incentivos.

---

## 4. Lo que está bien (mantener)

- **Secuencias de abandono bien escalonadas:** carrito (0h → +12h → +12h) y
  checkout (0h → +12h → +24h), con descuento del 5% en el último paso.
- **`exitConditions` correctas:** salida por `placed order` en todos; el carrito
  también sale al `started checkout`, cediendo el relevo al flujo de checkout.
- **`overlapLimiter` entre los flujos de e-commerce** evita apilar carrito +
  checkout + producto + reactivación en la misma ventana de 7 días.
- **Reactivación** con cadencia razonable (día 30 → 40 → 45) y límite de
  frecuencia de 180 días.

---

## 5. Plan de acción recomendado (en orden)

1. **Arreglar la bienvenida activa** (`[Restored] Welcome No.6`):
   - Añadir `first_subscription = true` (y, opcional, `subscription_method = Signup form`) al disparador. *(C1)*
   - Añadir `overlapLimiter` contra carrito/checkout/producto/reactivación. *(C2)*
   - Extenderla a una secuencia de 2–3 correos con incentivo de primera compra. *(A1)*
2. **Consolidar bienvenidas:** dejar una canónica; archivar/renombrar las otras dos. *(A2)*
3. **No activar `Welcome No.2`** hasta traducir el 2.º email al español y corregir la etiqueta de fusión del asunto. *(M1)*
4. **Estandarizar idioma** (`es_LA`/`es_CL`) y **nombre de remitente** en todos los flujos. *(M2, M3)*
5. **Revisiones opcionales:** presión de `Product Abandonment`, overlap de
   `Back in Stock`, cumplimiento de `nonSubscribed` y escalera de descuentos. *(B1–B4)*

---

## Anexo — IDs de flujos

| Flujo | ID |
|-------|----|
| Welcome | `6956e8f5a48f8eff8c6bc233` |
| Welcome No.2 | `6ac5ad90653f43ddf35c9b6c` |
| [Restored] Welcome No.6 | `6ac5ab23866034cbe8b9850c` |
| Abandoned Cart | `6957d659b5c9f095fff0481c` |
| Abandoned Checkout | `6957d65c6c365a01b98b57fa` |
| Product Abandonment | `69585d4728691b890f4828b8` |
| Back in Stock | `6a64e0ae10cee6c28fdaa9b9` |
| Customer Reactivation | `6958600c6c365a01b98b593e` |
