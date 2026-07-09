# Guía de Diseño — GAE Facultad de Minas

> Instrucciones de diseño para Antigravity CLI. Este documento es el **contexto obligatorio** que el asistente debe seguir al rediseñar cualquier sub-página del sitio de **Gestión de Asuntos Estudiantiles** (GAE) de la Facultad de Minas, Universidad Nacional de Colombia Sede Medellín.

---

## 0. Contexto de la migración — léelo primero

### Qué estamos haciendo

Se está **migrando y actualizando** el diseño de las páginas del GAE. Las páginas antiguas están construidas con la plantilla Joomla de la UNAL (HTML/CSS/JS heredado, estilos inline, scripts de ofuscación, jQuery, MooTools, etc.). El objetivo es rediseñarlas con un look moderno, limpio y profesional, manteniendo **exactamente la misma información** de la página antigua.

### Flujo de trabajo

1. **El usuario pega o referencia el código HTML de la página antigua** — este HTML es la **fuente de verdad en cuanto a información** (textos, links, normativa, requisitos, pasos, correos, etc.).
2. **El asistente rediseña la página** usando los componentes y estilos de esta guía, conservando toda la información del HTML antiguo.
3. Puedes **reorganizar, mejorar la redacción y agregar elementos** que mejoren la experiencia (por ejemplo: agrupar mejor la información, agregar iconos descriptivos, incluir notas aclaratorias), siempre que no se pierda ni se altere la información original.

### Público objetivo

Estudiantes universitarios de la Facultad de Minas. La interfaz debe ser:

- **Intuitiva**: un estudiante debe poder encontrar lo que busca en segundos.
- **Clara**: jerarquía visual fuerte, sin muros de texto.
- **Profesional**: transmitir seriedad institucional sin ser aburrida.
- **Coherente**: cada sub-página debe ser visualmente coherente con el **Home** del GAE.

### Página de referencia (Home)

El Home del GAE es la página de referencia maestra del diseño. Está en:

```
modules/facultad-de-minas/gestion-de-asuntos-estudiantiles/index.html
```

Todo diseño nuevo debe ser **coherente** con este Home en cuanto a paleta, tipografía, componentes, espaciado e interactividad.

### Páginas ya creadas

| Página | Archivo | Rol |
|---|---|---|
| **Home** | `index.html` | Landing principal con tiles de servicios, noticias e instructivos |
| **Solicitudes** | `nuevo-solicitudes-estudiantiles-2.html` | Listado de 25 solicitudes con buscador, filtros por categoría y FAB |
| **Sub-página ejemplo** | `admision-automatica-nuevo.html` | Página de detalle de una solicitud individual |

Las nuevas páginas son **sub-páginas** del GAE (de detalle de solicitudes, o de secciones como "Acerca de la oficina", "Gestión de calidad", etc.). Deben seguir los lineamientos de esta guía.

---

## 1. Stack tecnológico

```
HTML5 + TailwindCSS (CDN) + CSS custom + JavaScript vanilla
```

- **Tailwind**: cargar desde CDN (`https://cdn.tailwindcss.com`) con `important: true` en config.
- **Fuente principal**: Inter (Google Fonts), pesos 400, 500, 600, 700, 800.
- **Íconos**: Font Awesome 6.4+ (CDN).
- **Sin frameworks JS**. Todo interactivo (colapsar secciones, filtros, carruseles) es vanilla JS.

---

## 2. Paleta de colores

Usar estos tokens exactos en Tailwind config y en CSS custom:

| Token | Hex | Uso |
|---|---|---|
| `brand-blue` | `#1C4587` | Color institucional principal, navbar, links, acentos |
| `brand-blueDark` | `#163570` | Hover de elementos azules |
| `brand-blueLight` | `#2D62B5` | Botones secundarios, hover de links |
| `brand-yellow` | `#F5B01B` | Acento dorado, líneas decorativas, FAB, CTAs primarios |
| `brand-yellowDark` | `#D4960E` | Hover del amarillo |
| `page` | `#F7F8FA` | Fondo general de la página |
| `card` | `#FFFFFF` | Fondo de tarjetas |
| `border` | `#E8ECF0` | Bordes de tarjetas y separadores |
| `text-dark` | `#1A1D23` | Títulos, texto principal |
| `text-mid` | `#4B5563` | Texto de cuerpo, descripciones |
| `text-soft` | `#9CA3AF` | Texto terciario, placeholders, breadcrumb inactivo |

### Variables CSS complementarias

```css
:root {
  --brand-blue: #1C4587;
  --brand-yellow: #F5B01B;
  --r-sm: 6px;
  --r-md: 12px;
  --r-lg: 20px;
  --r-xl: 28px;
}
```

---

## 3. Escudo CSS contra la plantilla UNAL

> **OBLIGATORIO** en toda página. Estas reglas evitan que la plantilla Joomla de la UNAL sobreescriba los estilos. El sitio se publica **dentro** de la plantilla Joomla, que inyecta tipografías, subrayados, bordes, líneas verdes y otros estilos no deseados. El escudo los neutraliza.

### 3.1 Tipografía forzada (Escudo de tipografía)

Forzar la fuente Inter en **todos** los elementos, incluyendo `input`, `label`, `li`, `ol`, `ul` (que la guía original omitía en el Home pero sí incluía en las sub-páginas):

```css
#gae-landing {
  font-family: 'Inter', sans-serif !important;
}

#gae-landing h1, #gae-landing h2, #gae-landing h3, #gae-landing h4, #gae-landing h5, #gae-landing h6,
#gae-landing p, #gae-landing a, #gae-landing span, #gae-landing div, #gae-landing time, #gae-landing button,
#gae-landing input, #gae-landing label, #gae-landing li, #gae-landing ol, #gae-landing ul {
  font-family: 'Inter', sans-serif !important;
}
```

### 3.2 Eliminar decoraciones de títulos (línea verde, bordes, fondos)

```css
#gae-landing h1::after, #gae-landing h2::after, #gae-landing h3::after, #gae-landing h4::after,
#gae-landing h1::before, #gae-landing h2::before, #gae-landing h3::before, #gae-landing h4::before {
  display: none !important;
  content: none !important;
}

#gae-landing h1, #gae-landing h2, #gae-landing h3, #gae-landing h4 {
  border: none !important;
  background: none !important;
}
```

### 3.3 Bloquear subrayados globales al hover (Escudo de subrayado)

> **CRÍTICO.** Joomla inyecta `text-decoration: underline` en hover a muchos elementos. Debemos bloquearlo globalmente y **permitirlo solo donde sea explícitamente deseado** con la clase `enlace-subrayado`.

```css
/* Bloquear subrayados globales al pasar el mouse */
#gae-landing a:hover, #gae-landing p:hover, #gae-landing span:hover,
#gae-landing h1:hover, #gae-landing h2:hover, #gae-landing h3:hover,
#gae-landing h4:hover, #gae-landing div:hover, #gae-landing section:hover, #gae-landing button:hover {
  text-decoration: none !important;
}

/* Permitir subrayado únicamente en los textos con la clase explícita */
#gae-landing .enlace-subrayado:hover {
  text-decoration: underline !important;
}
```

**Cuándo usar `.enlace-subrayado`**: en links tipo "Ver todas las noticias →", "Ver todos los instructivos →" u otros enlaces donde el subrayado al hover mejora la UX. **No** usarlo en botones CTA, navbar, cards, tiles ni FAB.

### 3.4 El wrapper `#gae-landing`

**Todo** el contenido de la página debe estar envuelto en `<div id="gae-landing">`. Esto es lo que activa el escudo CSS.

```html
<body class="font-sans antialiased text-text-dark bg-page">
  <div id="gae-landing">
    <!-- Todo el contenido aquí -->
  </div>
</body>
```

---

## 4. Uso de `!important`

> **REGLA GENERAL:** Las clases de Tailwind ya llevan `!important` gracias a `important: true` en el config. Pero en **CSS custom** (clases propias como `.section-card`, `.cta-btn`, `.intro-alert`, etc.) **usa `!important`** en propiedades que la plantilla Joomla pueda sobreescribir, especialmente:
>
> - `font-family`
> - `text-decoration`
> - `color` (en links y botones)
> - `border`
> - `background`
> - `font-size`, `line-height` y `font-weight` cuando Joomla los pisotea (títulos, h4 de noticias, etc.)
>
> **Ejemplo real** del Home — los `h4` de las tarjetas de noticias necesitan `style` inline con `!important` para el tamaño de fuente porque Joomla los sobreescribe:
> ```html
> <h4 class="font-bold text-text-dark leading-snug mb-1.5 m-0 p-0"
>     style="font-size: 14px !important; line-height: 20px !important; font-weight: 600 !important;">
>   Título de la noticia
> </h4>
> ```
>
> Usa esta técnica **solo cuando sea necesario** (cuando las clases de Tailwind no sean suficientes para ganarle a Joomla). No abuses de `style` inline.

---

## 5. Tailwind Config

Copiar este bloque **exacto** en cada página:

```html
<script>
  tailwind.config = {
    important: true,
    theme: {
      extend: {
        fontFamily: { sans: ['Inter', 'sans-serif'] },
        colors: {
          brand: {
            blue:      '#1C4587',
            blueDark:  '#163570',
            blueLight: '#2D62B5',
            yellow:    '#F5B01B',
            yellowDark:'#D4960E',
          },
          page:   '#F7F8FA',
          card:   '#FFFFFF',
          border: '#E8ECF0',
          text: {
            dark: '#1A1D23',
            mid:  '#4B5563',
            soft: '#9CA3AF',
          },
        }
      }
    }
  }
</script>
```

---

## 6. Navbar

> **POSICIÓN:** El navbar se posiciona **sobre** (debajo en el DOM, pero visualmente encima) el header de Joomla. No reemplaza el header de la UNAL; se **superpone**. El header de Joomla sigue existiendo arriba y el navbar aparece inmediatamente después.

```html
<nav class="custom-navbar text-white shadow-md relative z-20 flex items-center">
  <div class="w-full flex justify-center items-center h-full">
    <ul class="nav-scroll flex items-center justify-center gap-0 text-sm font-medium whitespace-nowrap overflow-x-auto h-full px-4">
      <li><a href="index.html" class="block py-4 px-3 hover:text-brand-yellow transition-colors text-white">Inicio</a></li>
      <li class="text-white/30 select-none hidden sm:block">|</li>
      <li><a href="#" class="block py-4 px-3 hover:text-brand-yellow transition-colors text-white">Acerca de la oficina</a></li>
      <li class="text-white/30 select-none hidden sm:block">|</li>
      <li><a href="#" class="block py-4 px-3 hover:text-brand-yellow transition-colors text-white">Gestión de calidad</a></li>
      <li class="text-white/30 select-none hidden sm:block">|</li>
      <li><a href="#" class="block py-4 px-3 hover:text-brand-yellow transition-colors text-white">Gestión de la información</a></li>
      <li class="text-white/30 select-none hidden sm:block">|</li>
      <li><a href="#" class="block py-4 px-3 hover:text-brand-yellow transition-colors text-white">Flujo de atención</a></li>
      <li class="text-white/30 select-none hidden sm:block">|</li>
      <li><a href="#" class="block py-4 px-3 hover:text-brand-yellow transition-colors text-white">Preguntas Frecuentes</a></li>
    </ul>
  </div>
</nav>
```

CSS necesario:
```css
.custom-navbar { background-color: #1C4587; height: 55px; }
.nav-scroll { overflow-x: auto; -ms-overflow-style: none; scrollbar-width: none; }
.nav-scroll::-webkit-scrollbar { display: none; }
```

**Notas sobre el navbar:**
- En el **Home**: el navbar va justo debajo del hero (sección con imagen de fondo). No incluye "Inicio" porque ya estás en el Home.
- En las **sub-páginas**: el navbar va al inicio de `#gae-landing`, como primer elemento visible. **Sí incluye** "Inicio" como primer item.
- Los links del navbar son hover `text-brand-yellow`, sin subrayado.

---

## 7. Estructura de página

### 7.1 Home (`index.html`)

```
┌─ Hero (imagen de fondo + título + CTAs)
├─ Navbar (azul, 55px, centrado)
├─ Sección: Servicios en línea (tiles grid 4 columnas)
├─ Sección: Noticias (carrusel de tarjetas)
└─ Sección: Instructivos (grid de videos)
```

- **Ancho**: `max-w-7xl` (1280px)
- No tiene breadcrumb.

### 7.2 Sub-páginas de detalle (solicitudes)

```
┌─ Navbar (azul, 55px, centrado — con "Inicio")
├─ Main container (max-w-4xl, centered)
│  ├─ Breadcrumb
│  ├─ Page header (icono + badge categoría + título + línea amarilla)
│  ├─ Intro alert (fondo azul claro, borde izquierdo azul)
│  ├─ Sección: Normativa (section-card colapsable)
│  ├─ Sección: Requisitos Específicos (section-card colapsable)
│  ├─ Sección: Medios para realizar solicitud (section-card colapsable)
│  │  ├─ Sub-sección: SGS (pasos numerados + botón CTA primario)
│  │  └─ Sub-sección: Formato Sugerido (pasos + botón CTA secundario)
│  ├─ Sección: Notificación (section-card colapsable)
│  └─ Back link ("Volver a Solicitudes Estudiantiles")
└─ FAB flotante (esquina inferior derecha)
```

- **Ancho**: `max-w-4xl` (896px) — contenido de lectura debe ser más angosto.

### 7.3 Otras sub-páginas del GAE (no solicitudes)

Para páginas como "Acerca de la oficina", "Gestión de calidad", "Flujo de atención", etc., usar la misma lógica:

- Navbar con "Inicio".
- `max-w-4xl` o `max-w-5xl` según la densidad del contenido.
- Breadcrumb: `Inicio > Nombre de la sección`.
- Componentes reutilizables de esta guía según aplique.
- FAB obligatorio.
- Coherencia visual con el Home.

---

## 8. Componentes de página

### 8.1 Breadcrumb

```html
<nav class="flex items-center gap-1.5 text-sm text-text-soft mb-8 flex-wrap" aria-label="Breadcrumb">
  <a href="index.html" class="breadcrumb-link">Inicio</a>
  <i class="fa-solid fa-chevron-right text-[10px]"></i>
  <a href="nuevo-solicitudes-estudiantiles-2.html" class="breadcrumb-link">Solicitudes Estudiantiles</a>
  <i class="fa-solid fa-chevron-right text-[10px]"></i>
  <span class="text-text-dark font-medium">Nombre de la solicitud</span>
</nav>
```

CSS:
```css
.breadcrumb-link {
  color: var(--brand-blue) !important;
  text-decoration: none !important;
  transition: color .2s;
}
.breadcrumb-link:hover {
  text-decoration: underline !important;
}
```

### 8.2 Page Header

```html
<div class="mb-8">
  <div class="flex items-center gap-3 mb-3">
    <div class="w-10 h-10 rounded-xl bg-brand-blue/10 flex items-center justify-center">
      <i class="fa-solid fa-ICONO text-brand-blue text-lg"></i>
    </div>
    <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-blue-50 text-brand-blue tracking-wide uppercase">CATEGORÍA</span>
  </div>
  <h1 class="text-2xl sm:text-3xl font-extrabold text-text-dark m-0 p-0 leading-tight">TÍTULO DE LA SOLICITUD</h1>
  <div class="w-14 h-1.5 bg-brand-yellow mt-4 rounded-full"></div>
</div>
```

**Categorías y sus badges** (según la clasificación del listado de solicitudes):

| Categoría | Badge text | Ícono sugerido |
|---|---|---|
| Asignaturas | `Asignaturas` | `fa-book-open` |
| Grado | `Grado` | `fa-scroll` |
| Matrícula y Permanencia | `Matrícula y Permanencia` | `fa-clipboard-list` |
| Trámites Administrativos | `Trámites Administrativos` | `fa-file-signature` |
| Doble Titulación | `Doble Titulación` | `fa-certificate` |
| Movilidad | `Movilidad` | `fa-shuffle` |
| Posgrados | `Posgrados` | `fa-graduation-cap` |

### 8.3 Intro Alert

Siempre incluir el texto introductorio dentro de un alert azul:

```html
<div class="intro-alert mb-8">
  <div class="flex gap-3 items-start">
    <i class="fa-solid fa-circle-info text-brand-blue text-lg mt-0.5 flex-shrink-0"></i>
    <p class="text-sm text-text-mid leading-relaxed m-0">
      Lea atentamente todo el artículo antes de realizar su solicitud. En caso de duda,
      <a href="/tramitesestudiantiles/unidad-de-apoyo/contacto.html" class="inline-link">contacte al asesor estudiantil</a>
      de la Facultad de Minas. Verifique el
      <a href="/tramitesestudiantiles/calendarios/calendario-academico-sede-medellin.html" class="inline-link">Calendario Académico y de Solicitudes</a>
      y, en caso que la solicitud tenga fechas definidas, realice su solicitud de manera oportuna.
    </p>
  </div>
</div>
```

CSS:
```css
.intro-alert {
  background: linear-gradient(135deg, #EFF6FF 0%, #F0F4FF 100%);
  border: 1px solid #DBEAFE;
  border-left: 4px solid var(--brand-blue);
  border-radius: var(--r-md);
  padding: 1rem 1.25rem;
}
```

> **Nota:** Este texto es el mismo para TODAS las solicitudes (viene del viejo sitio, intro estándar). Solo cambia si la solicitud tiene un aviso especial.

---

## 9. Componentes reutilizables

### 9.1 Section Card (colapsable)

Cada bloque de contenido (Normativa, Requisitos, Medios, Notificación) es una tarjeta colapsable:

```html
<div class="section-card mb-5 reveal-on-scroll" id="sec-ID">
  <div class="section-card-header" aria-expanded="false" onclick="toggleSection('sec-ID')">
    <div class="section-card-icon bg-COLOR-50 text-COLOR-700">
      <i class="fa-solid fa-ICONO"></i>
    </div>
    <div>
      <h2 class="text-base font-bold text-text-dark m-0 p-0">TÍTULO</h2>
      <p class="text-xs text-text-soft m-0 mt-0.5">Subtítulo descriptivo</p>
    </div>
    <i class="fa-solid fa-chevron-down section-card-chevron"></i>
  </div>
  <div class="section-card-body collapsed">
    <!-- Contenido -->
  </div>
</div>
```

> **Nota:** Las tarjetas inician **colapsadas** (`aria-expanded="false"` + clase `collapsed` en el body) para que el usuario las abra una por una.
```

**Colores por sección:**

| Sección | Fondo ícono | Color ícono | Ícono |
|---|---|---|---|
| Normativa | `bg-purple-50` | `text-purple-700` | `fa-scale-balanced` |
| Requisitos | `bg-green-50` | `text-green-700` | `fa-clipboard-check` |
| Medios / Solicitud | `bg-blue-50` | `text-brand-blue` | `fa-paper-plane` |
| Notificación | `bg-amber-50` | `text-amber-700` | `fa-bell` |

CSS completo del section card:
```css
.section-card {
  background: #fff;
  border: 1px solid #E5E7EB;
  border-radius: var(--r-lg);
  box-shadow: 0 2px 8px rgba(0,0,0,0.03);
  overflow: hidden;
}

.section-card-header {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 1.25rem 1.5rem;
  border-bottom: 1px solid #F3F4F6;
  cursor: pointer;
  user-select: none;
  transition: background .15s;
}

.section-card-header:hover {
  background: #FAFBFC;
}

.section-card-icon {
  width: 40px; height: 40px; min-width: 40px;
  border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  font-size: 1rem;
}

.section-card-chevron {
  margin-left: auto;
  transition: transform .3s ease;
  color: #9CA3AF;
  font-size: 0.75rem;
}

.section-card-header[aria-expanded="true"] .section-card-chevron {
  transform: rotate(180deg);
}

.section-card-body {
  padding: 1.25rem 1.5rem;
  overflow: hidden;
  transition: max-height .4s ease, padding .3s ease;
}

.section-card-body.collapsed {
  max-height: 0 !important;
  padding-top: 0 !important;
  padding-bottom: 0 !important;
}

/* Hover lift en section cards */
.section-card {
  transition: transform .25s ease, box-shadow .25s ease;
}

.section-card:hover {
  transform: translateY(-3px);
  box-shadow: 0 6px 20px rgba(0,0,0,0.07);
}
```

### 9.2 Step Cards (pasos numerados)

```html
<div class="step-card">
  <div class="step-number">1</div>
  <p class="text-sm text-text-mid m-0 leading-relaxed">Texto del paso...</p>
</div>
```

CSS:
```css
.step-card {
  display: flex;
  gap: 1rem;
  align-items: flex-start;
  padding: 1rem 0;
}

.step-card + .step-card {
  border-top: 1px solid #F3F4F6;
}

.step-number {
  width: 32px; height: 32px; min-width: 32px;
  border-radius: 50%;
  background: var(--brand-blue);
  color: #fff;
  display: flex; align-items: center; justify-content: center;
  font-size: 0.8rem;
  font-weight: 700;
  flex-shrink: 0;
}
```

### 9.3 Requirement Items (requisitos con check)

```html
<div class="req-item">
  <div class="req-check"><i class="fa-solid fa-check"></i></div>
  <p class="text-sm text-text-mid m-0 leading-relaxed">Texto del requisito...</p>
</div>
```

CSS:
```css
.req-item {
  display: flex;
  gap: 0.75rem;
  align-items: flex-start;
  padding: 0.75rem 0;
}

.req-item + .req-item {
  border-top: 1px solid #F3F4F6;
}

.req-check {
  width: 24px; height: 24px; min-width: 24px;
  border-radius: 50%;
  background: #ECFDF5;
  color: #059669;
  display: flex; align-items: center; justify-content: center;
  font-size: 0.65rem;
  flex-shrink: 0;
  margin-top: 2px;
}
```

### 9.4 Normativa Link

```html
<a href="URL" target="_blank" rel="noopener noreferrer" class="normativa-link">
  <div class="normativa-link-icon">
    <i class="fa-solid fa-file-lines"></i>
  </div>
  <div>
    <p class="text-sm font-semibold text-text-dark m-0">Nombre del acuerdo / resolución</p>
    <p class="text-xs text-text-soft m-0 mt-1">Detalles · Abrir en el Sistema de Normatividad</p>
  </div>
  <i class="fa-solid fa-arrow-up-right-from-square text-text-soft text-xs ml-auto flex-shrink-0"></i>
</a>
```

Si hay múltiples normas, colocar varios `normativa-link` con `space-y-3` en el contenedor.

CSS:
```css
.normativa-link {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.85rem 1rem;
  border-radius: var(--r-md);
  background: #F9FAFB;
  border: 1px solid #E5E7EB;
  text-decoration: none !important;
  color: #1A1D23 !important;
  transition: all .25s ease;
}

.normativa-link:hover {
  border-color: #D1D5DB;
  box-shadow: 0 4px 12px rgba(0,0,0,0.05);
  transform: translateY(-2px);
}

.normativa-link-icon {
  width: 36px; height: 36px; min-width: 36px;
  border-radius: 8px;
  background: #EFF6FF;
  color: var(--brand-blue);
  display: flex; align-items: center; justify-content: center;
  font-size: 0.9rem;
}
```

### 9.5 Notes Box (notas importantes)

```html
<div class="notes-box mt-5">
  <p class="text-sm font-bold text-yellow-800 m-0 mb-2 flex items-center gap-2">
    <i class="fa-solid fa-triangle-exclamation text-yellow-600"></i> Notas importantes
  </p>
  <ul class="text-sm text-yellow-900/80 m-0 pl-5 space-y-1.5" style="list-style: disc;">
    <li>Nota 1</li>
    <li>Nota 2</li>
  </ul>
</div>
```

CSS:
```css
.notes-box {
  background: #FFFBEB;
  border: 1px solid #FDE68A;
  border-radius: var(--r-md);
  padding: 1rem 1.25rem;
}
```

### 9.6 CTA Buttons

**Primario** (acción principal, SGS):
```html
<a href="URL" target="_blank" class="cta-btn cta-primary">
  <i class="fa-solid fa-arrow-up-right-from-square"></i>
  Texto del botón — SGS
</a>
```

**Secundario** (Formato Sugerido, acciones alternativas):
```html
<a href="URL" target="_blank" class="cta-btn cta-secondary">
  <i class="fa-solid fa-file-arrow-down"></i>
  Texto del botón — Formato Sugerido
</a>
```

CSS:
```css
.cta-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 0.6rem;
  padding: 0.85rem 1.75rem;
  border-radius: var(--r-md);
  font-size: 0.9rem;
  font-weight: 700;
  text-decoration: none !important;
  cursor: pointer;
  transition: all .25s ease;
  border: none;
}

.cta-primary {
  background: var(--brand-yellow);
  color: #fff !important;
  box-shadow: 0 4px 14px rgba(245,176,27,0.3);
}
.cta-primary:hover {
  background: #D4960E;
  box-shadow: 0 6px 20px rgba(245,176,27,0.4);
  transform: translateY(-2px);
}

.cta-secondary {
  background: #fff;
  color: var(--brand-blue) !important;
  border: 1.5px solid #E5E7EB !important;
  box-shadow: 0 2px 8px rgba(0,0,0,0.04);
}
.cta-secondary:hover {
  border-color: var(--brand-blue) !important;
  box-shadow: 0 4px 14px rgba(28,69,135,0.1);
  transform: translateY(-2px);
}
```

### 9.7 Inline Links

Para links dentro de texto corrido:
```html
<a href="URL" class="inline-link">texto del link</a>
```

Para links externos agregar el ícono:
```html
<a href="URL" target="_blank" class="inline-link">texto <i class="fa-solid fa-arrow-up-right-from-square text-[10px]"></i></a>
```

CSS:
```css
.inline-link {
  color: var(--brand-blue) !important;
  font-weight: 600;
  text-decoration: none !important;
  transition: color .2s;
}
.inline-link:hover {
  text-decoration: underline !important;
}
```

### 9.8 FAB (Floating Action Button)

**OBLIGATORIO** en toda página. Copiar exacto:

```html
<a href="https://minas.medellin.unal.edu.co/tramitesestudiantiles/unidad-de-apoyo/contacto.html"
   class="fab-asesoria"
   aria-label="Solicite asesoría estudiantil">
  <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
    <path d="M21 11.5a8.38 8.38 0 0 1-.9 3.8 8.5 8.5 0 0 1-7.6 4.7 8.38 8.38 0 0 1-3.8-.9L3 21l1.9-5.7a8.38 8.38 0 0 1-.9-3.8 8.5 8.5 0 0 1 4.7-7.6 8.38 8.38 0 0 1 3.8-.9h.5a8.48 8.48 0 0 1 8 8v.5z"/>
    <circle cx="12" cy="11.5" r="1" fill="var(--brand-yellow)"/>
    <circle cx="8.5" cy="11.5" r="1" fill="var(--brand-yellow)"/>
    <circle cx="15.5" cy="11.5" r="1" fill="var(--brand-yellow)"/>
  </svg>
  <span class="fab-asesoria-label">Solicite asesoría</span>
</a>
```

CSS completo del FAB:
```css
.fab-asesoria {
  position: fixed;
  bottom: 20px;
  right: 20px;
  z-index: 9999;
  display: inline-flex;
  align-items: center;
  gap: 0;
  height: 56px;
  max-width: 56px;
  padding: 0 16px;
  border-radius: 9999px;
  background: var(--brand-yellow);
  color: #fff !important;
  text-decoration: none !important;
  box-shadow: 0 6px 20px rgba(245, 176, 27, 0.4);
  overflow: hidden;
  cursor: pointer;
  transition: max-width 0.35s cubic-bezier(0.4, 0, 0.2, 1),
              box-shadow 0.25s ease,
              gap 0.3s ease;
}

.fab-asesoria:hover {
  max-width: 280px;
  gap: 10px;
  box-shadow: 0 8px 28px rgba(245, 176, 27, 0.55);
}

.fab-asesoria svg {
  width: 24px;
  height: 24px;
  min-width: 24px;
  fill: #fff;
  flex-shrink: 0;
}

.fab-asesoria-label {
  white-space: nowrap;
  font-family: 'Ancizar Sans', 'Inter', sans-serif;
  font-weight: 700;
  font-size: 0.875rem;
  color: #fff;
  opacity: 0;
  max-width: 0;
  overflow: hidden;
  transition: opacity 0.25s ease 0.1s,
              max-width 0.35s cubic-bezier(0.4, 0, 0.2, 1);
}

.fab-asesoria:hover .fab-asesoria-label {
  opacity: 1;
  max-width: 200px;
}
```

> **Nota sobre tipografía del FAB:** La etiqueta del FAB usa `'Ancizar Sans'` como tipografía preferida (la fuente institucional de la UNAL que Joomla carga). Si no está disponible, cae a Inter. Esta es la **única excepción** al uso de Inter como fuente principal.

---

## 10. JavaScript requerido

### Secciones colapsables

```javascript
function toggleSection(sectionId) {
  const card = document.getElementById(sectionId);
  const header = card.querySelector('.section-card-header');
  const body = card.querySelector('.section-card-body');
  const isExpanded = header.getAttribute('aria-expanded') === 'true';

  if (isExpanded) {
    body.style.maxHeight = body.scrollHeight + 'px';
    requestAnimationFrame(() => {
      body.classList.add('collapsed');
      body.style.maxHeight = '0';
    });
    header.setAttribute('aria-expanded', 'false');
  } else {
    body.classList.remove('collapsed');
    body.style.maxHeight = body.scrollHeight + 'px';
    body.addEventListener('transitionend', function handler() {
      body.style.maxHeight = 'none';
      body.removeEventListener('transitionend', handler);
    });
    header.setAttribute('aria-expanded', 'true');
  }
}

// Inicializar secciones como colapsadas
document.querySelectorAll('.section-card-body').forEach(body => {
  body.style.maxHeight = '0';
});
```

### 10.2 Animaciones al cargar (reveal cascada)

Todas las sub-páginas de solicitudes incluyen una animación sutil de aparición al cargar la página. Los elementos aparecen con un fade-in + slide-up escalonado.

**CSS:**
```css
/* ── REVEAL AL CARGAR ── */
.reveal-on-scroll {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity .6s cubic-bezier(0.22, 1, 0.36, 1),
              transform .6s cubic-bezier(0.22, 1, 0.36, 1);
}

.reveal-on-scroll.revealed {
  opacity: 1;
  transform: translateY(0);
}
```

**JavaScript:**
```javascript
// ── REVEAL AL CARGAR (efecto cascada) ──
document.querySelectorAll('.reveal-on-scroll').forEach((el, i) => {
  setTimeout(() => el.classList.add('revealed'), i * 100);
});
```

**Elementos que llevan `reveal-on-scroll`** (en sub-páginas de solicitudes):

| Elemento | Clase a agregar |
|---|---|
| Breadcrumb (`<nav aria-label="Breadcrumb">`) | `reveal-on-scroll` |
| Page Header (`<div class="mb-8">`) | `reveal-on-scroll` |
| Intro Alert (`<div class="intro-alert mb-8">`) | `reveal-on-scroll` |
| Cada Section Card (`<div class="section-card mb-5">`) | `reveal-on-scroll` |

**Timing:** cada elemento aparece 100ms después del anterior, creando un efecto cascada sutil (~700ms total para 7 elementos).

> **Para páginas tipo catálogo** (como Solicitudes Estudiantiles con muchas tarjetas): usar 40ms de delay entre tarjetas del grid para que la cascada no sea demasiado lenta.

---

## 11. Reglas de migración del contenido viejo

Al recibir un HTML del sitio viejo, seguir estas reglas:

### 11.1 Qué PRESERVAR (contenido)

- ✅ Todos los **links** (SGS, formatos, normativa, calendarios, correos)
- ✅ El **texto de los requisitos**, pasos, notas y notificación
- ✅ Los **nombres de los botones** SGS y Formato Sugerido
- ✅ La **dirección de correo** (`asesorestu_med@unal.edu.co`)
- ✅ Los **links a normativa** (acuerdos, resoluciones)
- ✅ El contenido del **intro alert** (contacte al asesor + verifique calendario)

### 11.2 Qué DESCARTAR (del viejo)

- ❌ Todo el **header institucional** de la UNAL (logo, menú, social links, buscador Google)
- ❌ El **footer** de la UNAL
- ❌ Los **scripts de Joomla** (jQuery, MooTools, JCE, Google Analytics)
- ❌ Los **scripts de ofuscación de email** (`document.getElementById('cloak...')`)
- ❌ Los **estilos inline** (`style="font-size: 14pt;"`, `style="text-align: justify;"`)
- ❌ Los **spans vacíos** y `&nbsp;` de relleno
- ❌ Los **iframes** y SVGs de "external link" del editor JCE

### 11.3 Qué TRANSFORMAR

| Viejo | Nuevo |
|---|---|
| `<h2>` plano con título de sección | → `section-card` colapsable |
| `<ul>` con requisitos | → `req-item` con checks verdes |
| `<ol>` con pasos | → `step-card` con números azules |
| `<input type="button" class="btn btn-warning">` | → `<a class="cta-btn cta-primary">` |
| Enlace a formato PDF/Google Drive | → `<a class="cta-btn cta-secondary">` |
| Notas con guiones | → `notes-box` amarillo |
| Link a normativa | → `normativa-link` card |
| `<a class="wfpopup">` (enlaces JCE) | → `<a class="inline-link">` |
| Email ofuscado con script | → `<a href="mailto:..." class="inline-link">` directo |

---

## 12. Principios de diseño

### 12.1 Profesionalismo universitario

- El diseño debe transmitir **seriedad institucional** sin ser aburrido.
- Usar los colores de la UNAL (azul `#1C4587` y amarillo `#F5B01B`) como identidad.
- La información debe ser **fácil de escanear** — un estudiante debe poder encontrar lo que busca en segundos.

### 12.2 Jerarquía visual clara

1. **Título de la solicitud** → lo más grande y prominente
2. **Alert introductorio** → siempre visible, color azul claro
3. **Section cards** → agrupan la información lógicamente
4. **Botones CTA** → destacan con color amarillo (primario) o borde (secundario)
5. **Texto de cuerpo** → legible, `text-sm`, color `text-mid`

### 12.3 Espaciado y ritmo

- Separación entre section-cards: `mb-5`
- Padding interno de cards: `1.25rem 1.5rem`
- Margen del header al contenido: `mb-8`
- Margen inferior del breadcrumb: `mb-8`

### 12.4 Interactividad sutil

- **Cards**: se elevan al hover (`translateY(-4px)`, sombra expandida)
- **Botones CTA**: se elevan al hover (`translateY(-2px)`, sombra más fuerte)
- **Links normativos**: se elevan al hover con sombra sutil
- **FAB**: se expande horizontalmente al hover, revelando texto
- **Secciones**: colapsan/expanden con animación suave (max-height transition)
- **No usar** animaciones agresivas, parpadeos, o colores neón

### 12.5 Responsive

- El layout debe funcionar en móvil (`max-w-4xl` se adapta naturalmente).
- La navbar tiene scroll horizontal en móvil.
- Los botones CTA pasan a `flex-col` en pantallas pequeñas.
- El FAB permanece fijo en la esquina inferior derecha en todas las resoluciones.

---

## 13. Checklist antes de entregar una sub-página

- [ ] `<div id="gae-landing">` envuelve todo el contenido
- [ ] **Escudo CSS completo** incluido (tipografía + decoraciones + subrayados)
- [ ] Tailwind config con la paleta exacta y `important: true`
- [ ] Navbar azul con los items correctos (con o sin "Inicio" según el tipo de página)
- [ ] Breadcrumb: `Inicio > [Sección] > Nombre de la página`
- [ ] Badge de categoría correcto (si es una solicitud)
- [ ] Intro alert con links al asesor y al calendario (si es una solicitud)
- [ ] Todas las secciones son `section-card` colapsables (si aplica)
- [ ] **Section cards inician colapsadas** (`aria-expanded="false"` + `collapsed`)
- [ ] Pasos numerados con `step-card`
- [ ] Requisitos con `req-item` y checks verdes
- [ ] Notas en `notes-box` amarillo
- [ ] Botón SGS como `cta-primary` (si aplica)
- [ ] Botón Formato Sugerido como `cta-secondary` (si aplica)
- [ ] Link "Volver a..." al final (si es sub-página de detalle)
- [ ] FAB de asesoría incluido
- [ ] Todos los links del viejo sitio preservados
- [ ] Email directo (no ofuscado con script)
- [ ] Sin estilos inline heredados del viejo
- [ ] `<title>` y `<meta description>` actualizados
- [ ] Diseño coherente con el Home del GAE
- [ ] Clase `.enlace-subrayado` usada donde corresponda (no en navbar, botones ni cards)
- [ ] `!important` usado donde Joomla sobreescribe estilos
- [ ] **Animación reveal-on-scroll** en breadcrumb, header, intro alert y section cards
- [ ] **Hover lift** en section cards (`translateY(-3px)` + sombra)
