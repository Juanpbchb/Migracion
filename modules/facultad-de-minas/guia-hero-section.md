Rol y Objetivo: Actúa como un Desarrollador Frontend Senior. Tu tarea es maquetar el "Hero Section" y la "Barra de Estadísticas" de la página principal. El diseño debe ser premium, moderno y entregado en un único archivo HTML (con <style> y <script> incluidos).
Regla estricta de Arquitectura CSS (Scoping): Crea un contenedor principal .hero-minas-section y define TODAS las variables de color y espaciado de esta sección dentro de ese selector. NO uses el selector :root.
Tipografía: Utiliza la familia Ansisa para los textos visibles de las estadísticas, con fallback a un sans-serif geométrico.
Estructura Visual y Comportamientos CSS:
	1. Contenedor Principal (.hero-minas-section):
		○ Debe tener la imagen de fondo principal (la cual ya incluye los textos visuales del título).
		○ Altura considerable (ej. min-height: 80vh) y position: relative.
		○ Accesibilidad: Incluye un <h1>Bienvenidos a la Facultad de Minas</h1> y ocúltalo visualmente usando una clase estilo sr-only (screen-reader only) para mantener el SEO.
	2. Capa de Marca de Agua:
		○ Un div con position: absolute cubriendo el fondo, con la imagen de la marca de agua.
		○ Estado inicial: opacity: 0 con transición suave (transition: opacity 0.5s ease-in-out).
		○ Interacción: Al hacer :hover en .hero-minas-section, cambia a opacity: 0.1.
	3. Barra de Estadísticas (Contadores):
		○ Ubicada en la parte inferior, superpuesta al fondo. Usa display: flex.
		○ Fondo inicial: Transparente.
		○ Separadores: border-right: 1px solid rgba(255, 255, 255, 0.3) (excepto el último).
		○ Textos descriptivos visibles: Estudiantes Activos, Programas de Pregrado, Programas de Maestría, Programas de Doctorado, Programas de Especialización.
	4. Interacciones de la Barra (Hover):
		○ En .stat-item:hover, el fondo cambia a un amarillo institucional transparente.
		○ Elevación suave: transform: translateY(-4px).
		○ Transición premium: transition: all 0.3s cubic-bezier(0.25, 0.8, 0.25, 1).
Lógica JavaScript (Animación de Contadores rápida): Implementa un script en Vanilla JS utilizando IntersectionObserver.
	• Los números objetivo son: 7000, 12, 17, 8 y 19.
	• La animación debe completarse en máximo 1.5 segundos (1500ms).
	• El script debe formatear dinámicamente el número 7000 agregando el punto separador de miles ("7.000").
