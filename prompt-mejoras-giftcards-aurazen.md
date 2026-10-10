# Prompt para la otra sesión (Aura Zen — Gift Cards / Checkout)

> Copia todo lo que está dentro del bloque de abajo y pégalo en la otra sesión de Claude Code (la que tiene el código del sitio de Aura Zen).

---

Eres el desarrollador del sitio de **Spa Aura Zen** (React + Vite + Tailwind, sucursales en Concepción y Vitacura, con flujo de Gift Cards: `/gift-cards`, `/gift-cards/carrito`/checkout, más páginas `/sucursales` y `/nosotros`).

**Antes de escribir una sola línea de código**, explora el repo y ubica los componentes reales involucrados: la página de Gift Cards, el carrito/checkout, el modal de "Envía una tarjeta regalo", la sección de upsells ("Completa tu regalo"), la página de Sucursales y el componente de estadísticas de reseñas en "Nosotros". Trabaja respetando el design system actual (tipografías serif, paleta tierra/stone/espresso, estética zen minimalista). No rehagas lo que ya funciona: integra sobre lo existente.

El objetivo de TODOS estos cambios es **aumentar la conversión del flujo de Gift Cards** aplicando psicología de venta, sin romper la estética premium/relajada de la marca.

Implementa lo siguiente:

## 1. Personaje "observador" en el checkout (efecto de ser observado → más conversión)

**Por qué:** la gente completa más una acción cuando se siente observada (el clásico experimento de la caja de donaciones: con unos "ojos" dibujados arriba, las donaciones suben). Queremos esa calidez humana en el momento del pago.

**Qué hacer (versión elegante, NO agresiva):**
- Agrega un personaje humano en la zona del checkout/carrito: una **mujer en bata del spa Aura Zen**, en actitud relajada y amable, mirando hacia el usuario.
- **Micro-interacción sutil:** a medida que el usuario **rellena los datos** del formulario (nombre, correo, etc.), el personaje **va esbozando una sonrisa** de forma disimulada/progresiva. Nada caricaturesco: debe sentirse cálido y premium.
- Ideal que tenga **motion** (movimiento muy sutil de cabeza / parpadeo / sonrisa), tipo animación ligera. Puede ser un seguimiento muy leve del cursor con los ojos, PERO opcional y siempre discreto. **NO** hagas la versión exagerada (nada de enojarse, pulgares abajo, gestos bruscos ni que "regañe" al usuario si se aleja del botón pagar). La marca es zen: calidez, no presión evidente.
- **Enfoque familiar:** además, incluye en el checkout a la **familia Aura Zen** (papá, mamá y dos hijos, todos en bata del spa), puede ser **en miniatura**, para comunicar visualmente que el spa es un plan **familiar / para regalar a quien quieras**.
- Las imágenes/ilustraciones de los personajes se crearán con IA aparte. Por ahora deja **componentes y slots preparados** (placeholders con las dimensiones correctas y props para intercambiar la imagen/animación después, ej. soporte para PNG/WebP y para Lottie/sprite si luego se anima). Documenta en el código dónde se insertan los assets finales.
- Respeta accesibilidad y performance: la animación no debe trabar el scroll ni el input; respeta `prefers-reduced-motion`.

## 2. Términos y condiciones obligatorios de la Gift Card

- Agrega un **checkbox obligatorio** de aceptación de las **condiciones de la Gift Card** antes de poder pagar (mismo criterio que usamos en la preventa).
- El botón de pago debe estar **deshabilitado hasta que se acepten** las condiciones.
- Muestra las condiciones (vigencia, canje, sucursales válidas, no reembolsable, etc.) de forma accesible (link/acordeón/modal) junto al checkbox.

## 3. Completar / mejorar la página de Gift Card

- Falta terminar/pulir la **página de la Gift Card**. Revísala, complétala y mejora las partes inconsistentes de copy y layout.

## 4. Diseños por temática y época del año (selector "Elige tu imagen")

**Referencia:** otros spas ofrecen elegir la imagen de la gift card (ej. "Feliz Navidad", "San Valentín", etc.). Queremos lo mismo, con identidad Aura Zen.
- Implementa un **selector "Elige tu imagen"** donde el comprador elige el diseño de su Gift Card.
- Crea **varios diseños** para distintas **temáticas y épocas del año**, por ejemplo: **Navidad**, **San Valentín / amor**, **Despedida de soltero/a**, **Cumpleaños**, **Día de la Madre**, **Día del Padre**, **Agradecimiento**, y un diseño **neutro/clásico** por defecto.
- Que el diseño elegido se refleje en la previsualización de la tarjeta (la tarjeta oscura con logo, código y QR) y viaje hasta el comprobante/entrega.
- Deja la arquitectura lista para **agregar nuevas temáticas fácilmente** (ej. un array de temas con nombre, miniatura, colores/fondo).

## 5. Upsells inteligentes en "Completa tu regalo" (carrito)

**Problema actual:** la sección "Completa tu regalo" recomienda servicios sin considerar lo que la persona ya está comprando.
- Los upsells deben **recomendar solo servicios que NO están incluidos** en la Gift Card que se está comprando.
- **Ejemplo:** si compra un *Head Spa con potenciador de crecimiento capilar + Masaje Relajante + Limpieza Hidrofacial*, **NO** le recomiendes otra Limpieza Hidrofacial (ya la lleva). Recomiéndale servicios **distintos y que no se repitan** entre sí.
- Implementa la lógica de **de-duplicación**: cruza el contenido del carrito/gift card contra el catálogo de upsells y filtra lo ya incluido y los duplicados.

## 6. Oferta exclusiva + badges en el checkout (descuento adicional y "Más vendido")

- Al momento de **pagar** (checkout), genera una **oferta exclusiva**: un **descuento adicional** si agrega el/los servicio(s) recomendado(s) en ese momento.
- En cada cajón de servicio adicional muestra claramente:
  - El **monto y % de descuento** (ej. precio tachado → precio con descuento, y "-30%").
  - Un badge de **"MÁS VENDIDO"** en el/los servicio(s) estrella.
- **Diseño dinámico / motion** para los cajoncitos de los servicios adicionales (una animación sutil que llame la atención sin ser invasiva: hover, aparición escalonada, pulso leve en el badge de descuento). Respeta `prefers-reduced-motion`.

## 7. Consistencia de reseñas/estrellas entre sucursales (página "Nosotros")

**Problema actual:** se muestra **4,8★ Concepción** y **5,0★ Vitacura** por separado. Esto hace creer que el servicio en Concepción es **inferior** al de Vitacura.
- **Ajusta la presentación** para que **no exista esa percepción de inferioridad**, SIN inventar ni falsear reseñas (mantener honestidad).
- Opciones válidas (elige la mejor o propón una): mostrar una **valoración unificada de la marca Aura Zen** (promedio ponderado por nº de reseñas), o presentar ambas con un encuadre que destaque que **las dos son excelentes** (ej. "4,8–5,0★ en Google", "+242 reseñas", "comunidad que nos recomienda"), evitando la comparación lado a lado que invita a juzgar una como peor.
- Aplica el mismo criterio en cualquier otra parte del sitio donde aparezcan las estrellas por sucursal.

## 8. Fix móvil en la página de Sucursales

- En `/sucursales`, los enlaces **"Ver todo sobre Concepción"** y **"Ver todo sobre Vitacura"** no se ven bien en **móvil**.
- Ajusta el layout para que se muestren correctamente en móvil (prueba en breakpoints chicos; revisa recorte, overflow y legibilidad de las cards con los mapas).

## 9. Gift cards: ordenar por precio y resaltar el descuento

En `/gift-cards` (listado "Elige la experiencia" y/o el grid de gift cards):
- **Ordena las Gift Cards de mayor a menor precio** (las más caras primero).
- En cada una, muestra de forma **visiblemente llamativa**:
  - El **precio normal tachado** y el **precio con descuento**.
  - El **monto ahorrado** y el **% de descuento**.
  - Badges tipo **"MÁS VENDIDO"** donde corresponda.
- El objetivo es **inducir la compra del servicio más caro** destacando cuánto se ahorra. Mantén la estética premium (no saturar de rojos/ofertón barato; usa la paleta de la marca para el acento del descuento).

---

### Consideraciones generales
- **Mobile-first** y responsive en todo lo anterior.
- **Accesibilidad:** foco, contraste, `aria-*`, y `prefers-reduced-motion` en todas las animaciones.
- **Performance:** animaciones con GPU (transform/opacity), nada que bloquee el input del formulario.
- **Assets de IA (personajes):** deja placeholders y props listos; no bloquees la implementación esperando las imágenes finales.
- No inventes reseñas ni datos. No apliques presión agresiva en el personaje observador (versión cálida y sutil).
- Cuando termines, haz commit con mensajes claros por feature y deja una lista breve de qué assets de IA faltan por entregar (personajes: mujer en bata + familia en bata) y sus dimensiones.
