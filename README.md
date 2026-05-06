## 📁 Estructura del Proyecto

```bash
/project-root
│
├── /assets
│   ├── /images          # Imágenes globales (logos, recursos compartidos)
│   ├── /icons
│   ├── /fonts
│
├── /styles              # Estilos globales
│   ├── main.css
│   ├── variables.css
│   ├── reset.css
│
├── /scripts             # JavaScript global
│   ├── main.js
│   ├── utils.js
│
├── /modules
│   ├── /facultad-minas
│   │   ├── index.html   # Home del micrositio Facultad de Minas
│   │   │
│   │   ├── /asuntos-estudiantiles
│   │   │   ├── index.html   # Home de Asuntos Estudiantiles
│   │   │   │
│   │   │   ├── /solicitudes
│   │   │   │   └── index.html
│   │   │   │
│   │   │   ├── /constancias
│   │   │   │   └── index.html
│   │   │   │
│   │   │   ├── /calendarios
│   │   │   │   └── index.html
│   │   │   │
│   │   │   ├── /assets
│   │   │       ├── /images
│   │   │       ├── /css
│   │   │       ├── /js
│   │   │
│   │   ├── /bienestar-universitario
│   │       ├── index.html
│   │       ├── /assets
│   │           ├── /images
│   │           ├── /css
│   │           ├── /js
│
├── index.html           # Home principal del sitio
```

---

## 📌 Convenciones

* `index.html` representa la página principal de cada módulo o submódulo.
* Cada sección tiene su propia carpeta para permitir URLs limpias.
* Los recursos compartidos se ubican en `/assets`.
* Los recursos específicos de cada módulo se ubican dentro de su propio `/assets`.

---

## 🌐 Ejemplo de URLs

* `/facultad-minas/`
* `/facultad-minas/asuntos-estudiantiles/`
* `/facultad-minas/asuntos-estudiantiles/solicitudes/`
* `/facultad-minas/asuntos-estudiantiles/constancias/`

```
```
