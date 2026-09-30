# AGENTS.md

## Resumen del proyecto

Landing page estática de **Shadows of Edo**, un videojuego ficticio de acción/RPG en el Japón feudal. Stack: HTML5 + Tailwind CSS (vía CDN) + JavaScript vanilla. No hay framework, bundler ni `package.json`.

## Estructura de carpetas

```
shadows-of-edo/
├── AGENTS.md
├── index.html          # Marcado, config de Tailwind y <script> de JS
├── css/
│   └── styles.css      # Solo CSS propio que Tailwind no cubre
└── img/                # Solo JPG
    ├── hero-ronin.jpg
    ├── tale-dusk.jpg
    ├── gallery-lake.jpg
    ├── gallery-armor.jpg
    ├── gallery-alley.jpg
    ├── gallery-torii.jpg
    └── gallery-blade.jpg
```

- `index.html`: todas las secciones (`#tale`, `#gameplay`, `#media`, `#preorder`, `#newsletter`), la configuración de Tailwind y el JS al final del `<body>`.
- `css/styles.css`: animaciones y utilidades propias (`.reveal`, `.glow`, `.leaf`, `prefers-reduced-motion`). No duplicar aquí lo que Tailwind ya resuelve.
- `img/`: imágenes del sitio, referenciadas como `img/nombre.jpg`.

## Comandos

No hay paso de build: abrir `index.html` o servirlo es suficiente. Los comandos de abajo usan `npx`, así que no hace falta instalar nada en el proyecto.

```bash
# Servidor local (http://localhost:8000)
python3 -m http.server 8000
# Alternativa: npx serve .

# Formato (comprobar / aplicar)
npx prettier --check "**/*.{html,css,md}"
npx prettier --write "**/*.{html,css,md}"

# Validar HTML
# Si pide configuración, crear .htmlvalidate.json con {"extends": ["html-validate:recommended"]}
npx html-validate index.html

# Enlaces rotos (con el servidor local en marcha)
npx linkinator http://localhost:8000

# Rendimiento, accesibilidad y SEO (requiere Chrome)
npx lighthouse http://localhost:8000 --view

# Comprobar que todas las imágenes referenciadas existen
grep -o 'img/[a-z0-9-]*\.jpg' index.html | sort -u | while read f; do [ -f "$f" ] || echo "FALTA: $f"; done

# Imágenes que superan el presupuesto de peso
find img -type f -size +350k
```

## Pruebas

No hay pruebas automatizadas. Antes de dar una tarea por terminada:

1. Ejecutar `prettier --check`, `html-validate` y el comando de imágenes existentes.
2. Abrir la página en el servidor local y revisarla a 375 px (móvil) y a 1440 px (escritorio).
3. Comprobar que el menú móvil abre y cierra, que los efectos hover y el reveal funcionan y que el formulario muestra sus mensajes.
4. Con `prefers-reduced-motion` activado, confirmar que no hay animaciones.
5. Consola del navegador sin errores.

## Convenciones

### HTML
- HTML semántico: `header`, `nav`, `main`, `section`, `article`, `figure`, `footer`.
- Un solo `<h1>`; los demás encabezados en orden (`h2` → `h3`).
- Los `id` de sección en minúsculas y sin espacios. Los enlaces del menú dependen de ellos.
- Toda `<img>` lleva `alt` descriptivo. Las imágenes bajo el primer pantallazo llevan `loading="lazy"`; la del hero lleva `fetchpriority="high"`.
- Las imágenes de fondo llevan `data-fallback` para que el JS oculte la imagen rota y quede el degradado de respaldo.

### Tailwind y CSS
- Clases utilitarias directamente en el HTML.
- Colores y fuentes desde la configuración de Tailwind (`ink`, `coal`, `blood`, `bloodDark`, `font-serif`, `font-sans`). No escribir hex sueltos si existe un token.
- CSS propio solo en `css/styles.css`, nunca en un `<style>` dentro de `index.html`.
- Sin `!important`.
- Mobile-first: estilos base para móvil y prefijos `md:` / `lg:` para pantallas mayores.

### JavaScript
- Vanilla ES6+, dentro de una IIFE, en un único `<script>` al final del `<body>`.
- Sin librerías (jQuery, etc.) ni `var`.
- Respetar `prefers-reduced-motion` en cualquier animación nueva.
- Los mensajes dinámicos usan `aria-live`.

### Imágenes
- Solo JPG, nombres en kebab-case. No usar SVG para ilustraciones ni fotos (los iconos vienen de Bootstrap Icons).
- Tamaños de referencia: hero 1920×1080; `tale-dusk` 1200×900; galería entre 1200×1200 y 1600×900.
- Peso máximo: 350 KB en hero y galería, 250 KB en el resto (calidad JPG 75–82).
- Si se renombra una imagen, actualizar su referencia en `index.html`.

### Estilo de código e idioma
- Indentación de 2 espacios, comillas dobles en atributos HTML, punto y coma en JS.
- Texto visible de la página en inglés; comentarios y documentación en español.

## Límites

### Siempre
- Mantener contraste legible del texto sobre imágenes (con velo o degradado).
- Conservar el degradado de respaldo de cada bloque con imagen.
- Probar en móvil y escritorio tras cada cambio visual.

### Preguntar antes
- Añadir dependencias, CDNs o fuentes nuevas.
- Introducir un paso de build (npm, Vite, Tailwind CLI) o un `package.json`.
- Cambiar la paleta, las tipografías o los `id` de las secciones.
- Mover, renombrar o eliminar carpetas o archivos existentes.
- Reemplazar o borrar archivos de `img/`.
- Crear páginas nuevas.

### Nunca
- Usar frameworks (React, Vue, Angular) ni jQuery.
- Guardar claves, tokens o datos personales en el repositorio.
- Conectar el formulario de la newsletter a un servicio real sin instrucción explícita (hoy solo valida en el navegador y no envía nada).
- Usar imágenes, personajes o logotipos protegidos de otros juegos, películas o series.
- Usar animaciones que parpadeen rápido o que no se puedan desactivar.

## Notas conocidas

- Tailwind por CDN sirve para prototipar. Para producción habría que compilarlo; eso requiere autorización (ver «Preguntar antes»).
- El formulario de la newsletter es solo demostración.
- Los nombres, el logo «KAGE» y todo el contenido son ficticios.
