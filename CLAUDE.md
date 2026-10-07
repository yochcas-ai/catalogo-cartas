# Catálogo de cartas gradeadas · Ruma Store

Catálogo público (`index.html`) + admin privado (`admin.html`). Sitio estático en GitHub Pages, datos y fotos en Supabase.

## Cómo trabajamos
- Excelencia técnica: probar antes de publicar (simulación móvil + escritorio), verificar en la página en línea después.
- Explicar con el término técnico correcto junto a la explicación simple; Yorman está aprendiendo.
- Cada cambio: optimizar, automatizar, conectar; seguir buenas prácticas de empresas de referencia.
- Mantener este archivo limpio: solo lo que sigue siendo cierto y útil. Borrar lo obsoleto en cuanto lo sea.
- De vez en cuando revisar repos de referencia y traer lo mejor.
- Identidad Ruma: usar siempre los assets originales (moneda con corona, logo, Rey), nunca versiones dibujadas.
  Fuentes Luckiest Guy + Quicksand; morado `#4B2E83`, amarillo `#FFB727`, crema `#FFF8D8`. Siempre tema claro.

## Arquitectura
- Front: HTML autosuficiente por archivo, sin build. `index.html` lee por REST; `admin.html` usa supabase-js (jsdelivr).
- Supabase `ruma-catalogo-cartas`: tabla `cartas`, `ventas`, bucket público `cartas` (fotos y `marca/`).
- Fuente de verdad: el catálogo manda en precio, stock, nombre y fotos. Airtable queda para costos, lotes y precios de mercado.
- Etiqueta pública "Grading China" = valor `Chinas` en la base (solo cambia la etiqueta).

## Seguridad (no romper)
- RLS activo. El público solo ve cartas con `disponible`, `pv>0` y foto de frente, columnas limitadas.
- Escrituras solo por RPC `security definer` que validan `es_admin()` (correo del admin). `ventas` sin políticas.
- La clave publicable es pública por diseño; ninguna clave secreta va al repo ni al chat.
- Nada de `innerHTML` con datos; construir con `textContent`. Imágenes solo del origen de Supabase.

## Despliegue
- Push a `main` publica solo (Pages, ~1 min). Verificar el archivo en vivo con `fetch(..., {cache:'no-store'})`.
- Para ver cambios en el navegador: añadir `?v=N` o recarga fuerte.

## Trampas ya resueltas
- `[hidden]{display:none!important}` en el admin: `display:flex` anulaba el atributo.
- Móvil: usar `minmax(0,1fr)` en grids y `flex-wrap` en cabeceras para evitar desborde horizontal.
- Subida de fotos: `accept="image/*"`, redimensionar con `<img>` + canvas (sin opciones de `createImageBitmap`), mostrar errores en pantalla.
- Un `select` con `width:100%` dentro de una fila flex se sale de su caja: darle ancho fijo y `flex-basis` propio.

## Pendientes
- Cuadrar stock catálogo ↔ Airtable (hoy solo se descuenta en el catálogo).
- Cargar 4 cartas gradeadas con casas tipo Pyxis que no se detectaron como "Chinese".
- Confirmar casa gradeadora real de cada carta china (etiqueta del slab).
- Activar protección de contraseñas filtradas en Supabase Auth (desde el panel).
