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

---

## 5 bis. Actualización 2026-10-10 (segunda tanda aplicada)

Decisiones que aprobaste (2, 3, 4) — **hechas en vivo**:

- **#2 Reactivación → Winback real (6 meses).** La "Customer Reactivation"
  disparaba a **30 días** del pedido (demasiado pronto para un equipo durable, y
  además el *Post-compra* ya cubre satisfacción/accesorios a +3d/+16d). La
  **retimé a 180 días** y la renombré **"Winback / Reactivación (6 meses)"**. Su
  copy ("no hemos sabido de ti…") ahora sí calza con el momento. *(No creé un 3.er
  flujo de satisfacción para no pisar el Post-compra: cada flujo con un solo trabajo.)*
- **#3 Preventa GTD-250 → ACTIVADA.**
- **#4 Back in Stock → CREADO y ACTIVADO:** "Vuelve a estar disponible". Disparador
  `product back in stock`, envío inmediato, sin supresión por solape (un aviso de
  "volvió" no debe bloquearse), copy nuevo (*"¡Volvió! Lo que esperabas está
  disponible otra vez"*) + bloque dinámico de producto + pie traducido al español.
- **#5 Descuento:** se mantiene en **5%** (bienvenida, carro y checkout).

**Estado final: 9 flujos ACTIVOS** → Bienvenida (4), Carro abandonado, Checkout
abandonado, Abandono de producto (2), Winback (6 meses), Confirmación de compra,
Post-compra, Preventa GTD-250, Back in Stock.

---

## 6. Captar correos SIN el popup que tapa la pantalla (tu pregunta)

Problema: el popup a pantalla completa en móvil estorba mientras corres ads. No
hay que apagar la captación: hay que **cambiar el formato**, no eliminarlo. Opciones
(de más fácil a más estructural), todas compatibles con ads:

1. **Teaser + disparador inteligente (mismo popup, sin tapar).** Omnisend permite
   mostrar un **teaser** (una pestañita/burbuja en una esquina) en vez del popup a
   pantalla completa. Configuración recomendada:
   - **Móvil:** solo teaser ("🎁 Guía gratis"), se expande **solo al tocarlo**. Nunca tapa.
   - **Escritorio:** *exit-intent* (cuando el cursor va a salir) o al **50% de scroll**.
   - **Excluir** las landing de ads o mostrar recién al 2.º pageview / tras 15–20 s,
     con tope 1 vez por visitante.
   - Psicología: mere-exposure (presencia constante sin fricción) + loss aversion
     (exit-intent capta al que se va) sin bloquear la entrada del anuncio.
   - **Lo puedo dejar configurado yo** en tu form de la Guía si me dices "dale".

2. **Formulario embebido (inline) — cero overlay.** Un bloque de suscripción fijo en:
   - **Footer** del sitio (todas las páginas), **final de cada blog**, y
   - **Ficha de producto**: *"Avísame cuando vuelva el stock / baje de precio"*
     (además alimenta directo el Back in Stock y un futuro flujo de price-drop).
   - Requiere pegar el snippet en el sitio WooCommerce (tu web/dev).

3. **Landing page de captación (URL propia).** Un formulario tipo *landing page* con
   la guía/descuento. Úsalo como **destino del anuncio** o en el *link in bio* de
   Instagram/TikTok: el anuncio capta el mail, sin popup en el sitio.

4. **Opt-in en el checkout de WooCommerce.** Un checkbox de consentimiento de
   marketing en el checkout → los compradores entran a los flujos. Cero popup.
   Psicología: default effect.

5. **Meta Lead Ads (recomendado si ya inviertes en ads).** Formulario **nativo dentro
   de Facebook/Instagram**: el usuario deja su correo sin salir del anuncio y sin
   tocar tu web. Se sincroniza a Omnisend. Es la vía con menos fricción para tu caso.

**Prioridad sugerida:** (1) teaser/exit-intent ya mismo + (4) opt-in en checkout →
(2) embebido en footer y ficha → (5) Meta Lead Ads / (3) landing como destino de ads.

> Nota: mientras no haya **ninguna** vía de captación activa, la Bienvenida (4
> correos) no recibe entradas.

### ✅ Hecho (2026-10-10): formulario de la Guía en modo no intrusivo, PUBLICADO

Dejé **activo** el formulario de la Guía (`6a81dce3…`) así:
- **Formato `flyout`** (tarjeta que entra por la esquina, **no tapa la pantalla**;
  antes era popup central).
- **Aparece solo tras interacción:** scroll 40% **o** 20 s en página **o**
  exit-intent — **nunca al entrar** (no estorba el aterrizaje del anuncio).
- **Frecuencia:** 1 vez al día por visitante. No se muestra a quien llega desde un
  correo de Omnisend.
- **Resultado:** la captación vuelve a funcionar y la **Bienvenida (4 correos)**
  ya recibe entradas, sin bloquear tus ads.

**Pendiente (manual, 1 clic en el editor):** el **teaser** (burbuja mínima tipo
"🎁 Guía gratis") no es editable por API sin reenviar todo el diseño, así que lo
dejé sin tocar. Si lo quieres, en el editor del form → pestaña **Teaser** →
activarlo y cambiar el texto "GET 10% OFF" por "🎁 Guía gratis". Con flyout ya es
no intrusivo; el teaser lo haría aún más sutil.

**Siguientes pasos de captación recomendados** (tu web/dev): opt-in en el checkout
de WooCommerce, formulario embebido en footer y ficha de producto, y Meta Lead Ads
como destino de tus campañas.

---

---

## 7. Actualización 2026-10-10 (recomendaciones de la IA de Omnisend aplicadas)

Revisé tus capturas. Todo lo aplicado está **confirmado en vivo** (13 workflows,
form en flyout, y ya hay señal de ingresos: *Checkout abandonado* con revenue
atribuido). Apliqué yo las 2 sugerencias que mostraba Omnisend:

- **Abandono de producto (2 correos):** añadí **condiciones de salida** —el flujo
  ahora **sale** si la persona agrega al carrito, inicia checkout o compra—, para
  que no siga mandando recordatorios de navegación cuando los flujos de carrito y
  checkout ya la están contactando. *(Era la recomendación "Add an exit condition".)*
- **Formulario de la Guía:** pasé los textos de **MAYÚSCULAS a sentence case**
  ("¿Preparado para el calor de este verano?", "Sí, quiero la guía gratis",
  "Guía gratis para el calor", "Quiero mi guía gratis", "¡Listo!") — la sugerencia
  "+10% signups" — y **activé el teaser** (burbuja mínima "🎁 Guía gratis"), que
  junto con el flyout lo hace aún menos intrusivo.

**Lo único que te recomiendo hacer a ti:** abrir casatec.cl en el **celular** y
mirar cómo se ve el flyout/teaser una vez (es lo que no puedo previsualizar yo).
Si algo no te gusta, lo revierto en segundos.

**Lo que queda para tu web/dev (requiere acceso al sitio, no a Omnisend):** opt-in
de marketing en el checkout de WooCommerce, formulario embebido en footer y ficha
de producto, y Meta Lead Ads como destino de campañas.

---

## Anexo — IDs y enlaces (editor Omnisend)

**Activos (9):**
- Bienvenida (4 correos): `6ac815c6866034cbe8b9d653` — https://app.omnisend.com/automation/edit/6ac815c6866034cbe8b9d653
- Carro abandonado 🛒: `67e227d55484c48eb8cc7742` — https://app.omnisend.com/automation/edit/67e227d55484c48eb8cc7742
- Checkout abandonado: `67df2ec5414d1dc8d28ea7ff` — https://app.omnisend.com/automation/edit/67df2ec5414d1dc8d28ea7ff
- Abandono de producto (2 correos): `6ac8185a12da75235968e893` — https://app.omnisend.com/automation/edit/6ac8185a12da75235968e893
- Winback / Reactivación (6 meses): `69642e0a324f040149189fed` — https://app.omnisend.com/automation/edit/69642e0a324f040149189fed
- Confirmación de compra 💰: `67e1c28e3d78a08502c46b7d` — https://app.omnisend.com/automation/edit/67e1c28e3d78a08502c46b7d
- Post-compra: `6abff4182c2ab323fbfa4b95` — https://app.omnisend.com/automation/edit/6abff4182c2ab323fbfa4b95
- Preventa GTD-250: `6ac819fc12da75235968e8b0` — https://app.omnisend.com/automation/edit/6ac819fc12da75235968e8b0
- Vuelve a estar disponible (Back in Stock): `6ac99e10653f43ddf35ce858` — https://app.omnisend.com/automation/edit/6ac99e10653f43ddf35ce858

**Archivados/desactivados:**
- `[OBSOLETO] Welcome No.2 (1 correo)`: `6ac5b57b866034cbe8b98586`
- `[OBSOLETO] Bienvenida (1 correo)`: `6963ef22324f040149189fb3`
- `[OBSOLETO] Product Abandonment (1 correo)`: `69642bdae3e82127156fabf0`
- `[OBSOLETO] Reactivación 180 días`: `6ac818e8866034cbe8b9d66e`
- `Preventa GTD-250`: `6ac819fc12da75235968e8b0`

**Formularios (desactivados):**
- Guía gratis: `6a81dce3e080d6c53735c833`
- 5% welcome: `69642ab177cbba1bd7812f50`
