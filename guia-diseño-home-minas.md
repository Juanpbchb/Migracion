# Contexto Maestro de Desarrollo: Home Facultad de Minas

Actúa como un Desarrollador Frontend Senior. Al generar o modificar cualquier bloque de código para este proyecto, debes cumplir estrictamente con las siguientes directrices arquitectónicas y de diseño:

## 1. El "Escudo Anti-Joomla" (Arquitectura de Archivo Único)
Para garantizar una migración limpia y evitar conflictos con el gestor de plantillas de Joomla 6, **TODO el código debe entregarse en un único archivo HTML**. 
* El CSS debe ir obligatoriamente dentro de etiquetas `<style>` en el `<head>`.
* La lógica de JavaScript debe ir dentro de etiquetas `<script>` al final del `<body>`.
* No generes archivos `.css` o `.js` externos.

## 2. Encapsulamiento y Scoping (Colores Independientes)
Cada sección de la página (Hero, Tarjetas, Footer) debe ser un módulo completamente independiente para facilitar futuras ediciones sin dañar el resto del sitio.
* **Prohibido usar `:root`**. 
* Todas las variables CSS (Custom Properties) deben definirse localmente dentro del contenedor principal de la sección. 
* Ejemplo: `.seccion-noticias { --bg-color: #ffffff; --text-color: #333333; }`

## 3. Paleta de Colores Institucional
El color de acento principal debe ser el amarillo institucional exacto. Cuando se requiera destacar textos, botones o hover states, utiliza:
* **Amarillo Institucional:** `#F7AD14`

## 4. Tipografía Oficial (Archivos Estáticos Locales)
El proyecto utiliza la familia tipográfica propietaria "Ancizar Sans". Debes incluir las declaraciones `@font-face` al inicio del bloque `<style>` apuntando a los archivos estáticos locales para garantizar máxima fidelidad.

```css
@font-face {
  font-family: 'Ancizar Sans Light Italic';
  src: url('./assets/fonts/AncizarSans-LightItalic.ttf') format('truetype');
  font-weight: normal; 
  font-style: normal;
  font-display: swap;
}

@font-face {
  font-family: 'Ancizar Sans Bold Italic';
  src: url('./assets/fonts/AncizarSans-BoldItalic.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

5. Diseño Estrictamente Responsivo (Mobile-First)
La página debe verse impecable en cualquier dispositivo sin depender de media queries excesivas.

Utiliza funciones CSS modernas como clamp() para tipografías fluidas (ej. clamp(1.5rem, 4vw, 3rem)).

Prioriza display: flex y display: grid con unidades relativas (%, vw, vh, fr).

Evita declarar alturas fijas (height: 500px); utiliza min-height o aspect-ratio para evitar que el contenido se desborde o se generen franjas vacías.

Todo diseño debe ser profesional, limpio y evitar el aspecto de "plantilla genérica".