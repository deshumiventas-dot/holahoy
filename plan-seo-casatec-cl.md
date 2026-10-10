# Plan SEO — Casatec (casatec.cl)

- **Sitio:** casatec.cl · WordPress + WooCommerce · AIOSEO (Free) · WP Fastest Cache
- **Negocio:** deshumidificadores, aire portátil, control de humedad · base Concepción, Chile
- **Fecha:** 2026-10-10
- **Estado estimado:** base ~55–65/100 → objetivo ~90 con este plan
- **Nota:** no se puede editar el sitio por API (Jetpack sin plan AI/Complete); los ajustes
  los aplica el equipo en wp-admin. El contenido sí lo generamos aquí.

---

## 1. Checklist AIOSEO — para llegar al 100% (en orden)

**🔴 Alta**
1. **Completar asistente de configuración:** tipo *Tienda online/WooCommerce*, *Organización* = Casatec, subir logo, añadir redes.
2. **Conectar Google Search Console** y **enviar sitemap** (`casatec.cl/sitemap.xml`). Repetir en Bing Webmaster Tools.
3. **Ejecutar auditoría de la home** y corregir lo que marque (title/meta/H1/imágenes).

**🟠 Medio**
4. **Activar llms.txt** (visibilidad en IA — ChatGPT/Claude).
5. **Información de la organización:** nombre, logo, dirección (Concepción), teléfono, redes → genera schema de Organización.
6. **Ajustes de entrada (blog):** título `#post_title #separator_sa #site_title`, meta `#post_excerpt`, indexable.

**🔵 Baja**
7. **Taxonomías:** categorías de producto **INDEXAR**; etiquetas/tags **NOINDEX**.
8. **Archivos:** autor y fecha **NOINDEX**.

**⚪ Opcional**
9. **Broken Link Checker:** saltar/mejor un escaneo puntual en la nube (el plugin residente carga el servidor).

## 2. Lo que más mueve ventas (prioridad real)
1. **Títulos y meta de páginas comerciales** (fichas de producto + categorías) con keywords de Chile.
2. **Google Business Profile** (Concepción) → SEO local + reseñas.
3. **Blog con intención de compra** (ver roadmap) enlazando a productos.
4. **Alt text** en imágenes de producto con el modelo + keyword.
5. **Product schema** (lo añade WooCommerce + AIOSEO; verificar en Rich Results Test).

## 3. Keywords prioritizadas (estimado — validar en Keyword Planner geo Chile)

| Keyword | Intención | Dificultad | Artículo |
|---|---|---|---|
| cómo eliminar la humedad de la casa | info→compra | Baja | ✅ Escrito (pilar) |
| deshumidificador para dormitorio | compra | Baja | Pendiente |
| deshumidificador para el sur de chile | compra | Muy baja | Pendiente |
| cómo eliminar el moho de las paredes | info→compra | Baja | Pendiente |
| mejor deshumidificador (comparativa) | comercial | Media-alta | Pendiente |
| cuánto consume un deshumidificador | investigación | Baja | Pendiente |
| aire acondicionado portátil (sin manguera) | compra | Media | Pendiente (verano) |
| deshumidificador industrial | compra B2B | Baja | Pendiente |

### Roadmap 3 meses (invierno = peak)
- **Mes 1:** humedad casa ✅ · dormitorio · guía "cuál elegir"
- **Mes 2:** moho paredes · comparativa mejor deshumidificador · sur de Chile
- **Mes 3:** consumo · industrial · aire portátil (mirando verano)

## 4. Primer artículo (listo para publicar)
`blog-como-eliminar-la-humedad-de-la-casa.md` — pilar, ~1.950 palabras, con meta title,
meta description, handle, H1/H2/H3, cuerpo en HTML y alt text. Enlaza a categoría
deshumidificadores y a GTD-250, SmartDry Pro, Drybox y ULTRADRY500.
> Confirmar los slugs reales de GTD-250, SmartDry Pro y Drybox antes de publicar
> (el de ULTRADRY500 ya está verificado).

## 5. Qué hace cada quién
- **Equipo Casatec (wp-admin):** checklist AIOSEO puntos 1–8, publicar el artículo, GBP.
- **Claude:** generar los siguientes artículos del roadmap, keyword research, meta/títulos
  de páginas comerciales, schema a pegar. (Publicación directa no disponible por el plan de Jetpack.)
