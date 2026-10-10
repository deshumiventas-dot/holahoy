# Auditoría de Flujos (Automations) de Omnisend — Casatec Spa (casatec.cl)

- **Marca:** Casatec Spa (`brandID: 67df2297f8deedeb6f9ca25c`)
- **Sitio / Plataforma:** https://casatec.cl · WooCommerce
- **Remitente:** `ventas@casatec.cl` · Moneda: CLP · Zona horaria: America/Santiago
- **Fecha de auditoría:** 2026-10-10
- **Alcance:** 12 flujos de automatización (6 activos, 6 desactivados/borradores)
- **Método:** Lectura vía API de `trigger`, `exitConditions`, `settings`
  (limitadores de frecuencia/solape, umbrales de envío) y árbol de `blocks`
  (emails, delays, tags) de cada flujo.

> Diagnóstico de solo lectura. **No se modificó ningún flujo.** Las
> correcciones propuestas requieren tu confirmación antes de aplicarse en vivo.
> (Nota: esta auditoría es de la cuenta **casatec.cl**; es distinta de la de
> Casatec Global / Shopify, documentada aparte.)

---

## 1. Resumen ejecutivo

La cuenta está **más madura** que un setup básico: cubre bienvenida, carrito,
checkout, abandono de producto, reactivación, confirmación de compra, y hay
**borradores nuevos bien construidos** (bienvenida de 4 correos, post-compra,
preventa GTD-250, etc.) que parecen una **migración a medio terminar**: las
versiones mejoradas están **desactivadas** mientras siguen activas las antiguas
(o más pobres).

Los 3 puntos más importantes:

1. **Texto corrupto en un correo que se está enviando ahora:** el 3.er email del
   flujo activo *"Checkout abandonado"* tiene el preheader roto (frases pegadas y
   con errores: *"…los artículos que te gustarones de que se acaben"*).
2. **La bienvenida activa es de 1 solo correo**, mientras una **secuencia de 4
   correos ya construida** (*"Bienvenida 4 correos (borrador nuevo)"*) está
   desactivada. Se está dejando conversión sobre la mesa.
3. **Los limitadores de solape (`overlapLimiter`) apuntan a la bienvenida
   ANTIGUA desactivada** (`6963ef22`), no a la bienvenida activa (`6ac5b57b`):
   la bienvenida activa no protege ni está protegida frente a los flujos de
   e-commerce.

---

## 2. Inventario de flujos

### Activos (6)

| Flujo | ID | Disparador | Nº emails | Frecuencia | Overlap | Notas |
|-------|----|-----------|-----------|------------|---------|-------|
| Welcome No.2 | `6ac5b57b…` | Suscrito (Email + form + 1ª susc.) ✅ | 1 | once | ❌ **no** | Solo envía la guía |
| Carro abandonado 🛒 | `67e227d5…` | Añadió al carrito (30 min) | 3 | 1/día | ✅ sí | 0h→+12h→+12h (5%) |
| Checkout abandonado | `67df2ec5…` | Inició checkout (20 min) | 3 | 1/día | ✅ sí | 0h→+6h→+24h (5%) · **preheader roto en email 3** |
| Product Abandonment | `69642bda…` | Vio producto (en stock) | 1 | 1/día | ⚠️ apunta a welcome desactivada | +30 min |
| Customer Reactivation | `69642e0a…` | Realizó pedido | 3 | 1/180 d | ⚠️ apunta a welcome desactivada | 30d→10d→5d |
| Confirmación de compra 💰 | `67e1c28e…` | Pagó pedido | 1 | — | — | Transaccional (email=all) ✅ |

### Desactivados / borradores (6)

| Flujo | ID | Disparador | Nº emails | Overlap | Observación |
|-------|----|-----------|-----------|---------|-------------|
| Bienvenida 4 correos (borrador nuevo) | `6ac815c6…` | Suscrito (Email + form + 1ª) ✅ | 4 | ❌ no | **Secuencia mejor que la activa** (guía→elección→prueba social→5% vence 48h) |
| Bienvenida (antigua) | `6963ef22…` | Suscrito (Email + form + 1ª) | 1 | ✅ sí | Vieja; es la referida por los overlap de otros flujos |
| Product Abandonment 2 correos (borrador nuevo) | `6ac8185a…` | Vio producto | 2 | ✅ sí | +30 min → +24h (mejor que la activa de 1 correo) |
| Reactivación 180 días (borrador nuevo) | `6ac818e8…` | Realizó pedido | 3 | ❌ no | 180d→6d→7d, 7% final |
| Preventa GTD-250 (borrador nuevo) | `6ac819fc…` | Pagó pedido con permalink ~ `gtd-250` | 1 | — | Flujo de preventa; capacidad nueva |
| Post-compra (consejos, reseña, venta cruzada) | `6abff418…` | Pedido preparado (`order fulfilled`) | 2 | — | +3d→+16d; capacidad nueva, hoy sin equivalente activo |

---

## 3. Hallazgos priorizados

### 🔴 CRÍTICO

**C1 — Texto corrupto en el 3.er correo de "Checkout abandonado" (ACTIVO, enviándose ahora)**
- Flujo `67df2ec5…`, bloque `mZmWpRwy`.
- Preheader actual (roto): *"No olvides conseguirlos an¡Última llamada! Te damos
  un 5% de descuento en los artículos que te gustarones de que se acaben⌛"*.
  Mezcla dos frases y tiene errores ("gustarones").
- **Impacto:** se muestra a clientes reales en la bandeja de entrada; daña
  credibilidad y CTR.
- **Recomendación:** reemplazar por un preheader limpio, p. ej.
  *"¡Última llamada! Un 5% de descuento en los artículos que te gustaron, antes
  de que se agoten ⌛"*.

### 🟠 ALTO

**A1 — La bienvenida activa es de 1 correo; la secuencia de 4 correos ya lista está desactivada**
- Activo: *"Welcome No.2"* (`6ac5b57b…`) → solo la guía gratis (1 email).
- Desactivado: *"Bienvenida 4 correos (borrador nuevo)"* (`6ac815c6…`) →
  guía → cómo elegir equipo → prueba social/garantía → 5% que vence en 48 h.
- **Impacto:** la bienvenida es el flujo de mayor conversión; quedarse en 1
  correo (sin incentivo de compra) pierde ventas de nuevos suscriptores.
- **Recomendación:** activar la de 4 correos y **desactivar** la de 1 correo
  (evitar dos bienvenidas simultáneas), tras revisar remitente y overlap (ver A2/M1).

**A2 — Los `overlapLimiter` apuntan a la bienvenida ANTIGUA desactivada, no a la activa**
- Varios flujos (Product Abandonment, Customer Reactivation) excluyen contactos
  recientes de `6963ef22` (*"Bienvenida"* antigua, **desactivada**), por lo que
  esa exclusión no hace nada; y **ningún** flujo referencia la bienvenida activa
  (`6ac5b57b`). Además, la bienvenida activa **no tiene** `overlapLimiter`.
- **Impacto:** un nuevo suscriptor que además navega/abandona carrito puede
  recibir bienvenida + abandono apilados.
- **Recomendación:** al dejar una bienvenida canónica (ver A1), actualizar los
  `overlapLimiter` de carrito/checkout/producto/reactivación para que apunten a
  **esa** bienvenida activa, y darle a la bienvenida su propio `overlapLimiter`
  contra los flujos de e-commerce (`recentlyIn`, 7 días).

**A3 — Migración a medio terminar: borradores mejores desactivados y versiones antiguas activas**
- *Product Abandonment* activo = 1 correo; borrador *"2 correos"* (`6ac8185a…`) = 2 correos (mejor).
- *Preventa GTD-250* (`6ac819fc…`) y *Post-compra* (`6abff418…`) son **capacidades
  nuevas** sin equivalente activo hoy (preventa y nutrición post-compra).
- *Reactivación 180 días (borrador)* (`6ac818e8…`) duplica a la *Customer
  Reactivation* activa, pero **sin overlap**.
- **Recomendación:** decidir la versión canónica de cada caso de uso, activar la
  buena, y **archivar/renombrar** las obsoletas (`[OBSOLETO] …`) para quitar
  ambigüedad. Sugerencia: activar Post-compra y Preventa (aportan sin pisar nada);
  sustituir Product Abandonment por la de 2 correos; mantener **una** reactivación.

### 🟡 MEDIO

**M1 — Nombre de remitente inconsistente (incluso dentro del mismo flujo)**
- Variantes: `"Casatec Spa"`, `"Casatec Chile 🇨🇱"`, `"Casatec Chile"`, `"Casatec"`.
  En *"Bienvenida 4 correos"* alterna `"Casatec Spa"` y `"Casatec"`; en *"Carro
  abandonado"* alterna `"Casatec Chile 🇨🇱"` y `"Casatec Chile"`.
- **Recomendación:** definir **un** nombre de remitente estándar (p. ej.
  `Casatec Chile`) y aplicarlo a todos los correos de todos los flujos.

**M2 — `overlapLimiter` referenciando IDs de flujos desactivados**
- Más allá de la bienvenida (A2), conviene depurar las listas de
  `overlapLimiter` para que solo contengan flujos **activos y vigentes**; las
  referencias a IDs desactivados no aportan y confunden el mantenimiento.

### 🟢 BAJO / Observaciones

- **B1 — Secuencia de abandono bien escalonada** (carrito 0/+12/+12h; checkout
  0/+6/+24h) y checkout con inactividad 20 min < carrito 30 min (correcto: mayor
  intención → espera más corta). Mantener.
- **B2 — Confirmación de compra** usa `email=all`/`sms=all` (transaccional):
  correcto.
- **B3 — Product Abandonment ("vio producto")** es de alto volumen; acotado por
  frecuencia 1/día y overlap. Aceptable; vigilar presión de envío.
- **B4 — Umbral `nonSubscribed`** en abandono/reactivación/post-compra: estándar
  de Omnisend para alcanzar a quienes dispararon un evento comercial; validar que
  cumple el consentimiento aplicable en Chile.

---

## 4. Lo que está bien (mantener)

- Disparador de bienvenida correctamente filtrado (`first_subscription` + form).
- Secuencias de carrito y checkout bien escalonadas, con `exitConditions`
  correctas (`placed order`, y el carrito también sale al `started checkout`).
- Confirmación de compra transaccional con `email=all`.
- Borradores nuevos con buena estructura (bienvenida de 4 pasos, post-compra,
  preventa): buena base, solo falta activarlos y limpiar duplicados.

---

## 5. Plan de acción recomendado (en orden)

1. **Arreglar YA el preheader roto** del 3.er correo de *Checkout abandonado*. *(C1)*
2. **Bienvenida:** activar *"Bienvenida 4 correos"* y desactivar *"Welcome No.2"*;
   unificar remitente; añadirle `overlapLimiter`. *(A1, A2, M1)*
3. **Actualizar los `overlapLimiter`** de carrito/checkout/producto/reactivación
   para apuntar a la bienvenida activa y quitar IDs desactivados. *(A2, M2)*
4. **Completar la migración:** activar Post-compra y Preventa GTD-250; sustituir
   Product Abandonment por la versión de 2 correos; dejar una sola reactivación;
   archivar/renombrar las obsoletas. *(A3)*
5. **Estandarizar remitente** en todos los flujos. *(M1)*
6. **Revisiones opcionales:** presión de Product Abandonment y cumplimiento de
   `nonSubscribed`. *(B3, B4)*

---

## Anexo — IDs de flujos

| Flujo | ID | Estado |
|-------|----|--------|
| Welcome No.2 | `6ac5b57b866034cbe8b98586` | Activo |
| Carro abandonado 🛒 | `67e227d55484c48eb8cc7742` | Activo |
| Checkout abandonado | `67df2ec5414d1dc8d28ea7ff` | Activo |
| Product Abandonment | `69642bdae3e82127156fabf0` | Activo |
| Customer Reactivation | `69642e0a324f040149189fed` | Activo |
| Confirmación de compra 💰 | `67e1c28e3d78a08502c46b7d` | Activo |
| Bienvenida 4 correos (borrador nuevo) | `6ac815c6866034cbe8b9d653` | Desactivado |
| Bienvenida (antigua) | `6963ef22324f040149189fb3` | Desactivado |
| Product Abandonment 2 correos (borrador nuevo) | `6ac8185a12da75235968e893` | Desactivado |
| Reactivación 180 días (borrador nuevo) | `6ac818e8866034cbe8b9d66e` | Desactivado |
| Preventa GTD-250 (borrador nuevo) | `6ac819fc12da75235968e8b0` | Desactivado |
| Post-compra (consejos, reseña, venta cruzada) | `6abff4182c2ab323fbfa4b95` | Desactivado |
