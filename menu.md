# 📱 Restaurant Menu Filter: Architectural Blueprint

Este repositorio contiene la **documentación de diseño y arquitectura** del proyecto Menu. Aunque la lógica interna esencial se protege en desarrollo o servidores, este "Architectural Blueprint" describe la toma de decisiones, la organización y cómo se resolvió la capa de presentación de forma óptima usando React.

## 🛠️ Stack Tecnológico
- **Core**: [React 18](https://react.dev/) (Componentes Funcionales y React Hooks: `useState`).
- **Styling**: Estilos globales mediante `index.css` y `App.css`.
- **Ecosistema**: Bootstrapped con Create React App.

---

## 🏗️ Estructura del Proyecto (System Architecture)

El proyecto opta por una arquitectura "Flat" (plana) concentrada dentro de la carpeta `src`, lo que es ideal para jerarquías basadas en estado puro (*State-driven UIs*) y extracción dinámica de propiedades.

```text
src/
├── App.js              # Controlador principal (State Container) y Enrutador de Props.
├── Categories.js       # Componente de interfaz: Filtros que alteran el State global.
├── Menu.js             # Componente de presentación: Renderizado iterativo del catálogo.
├── data.js             # Capa de datos simulada (Mock DB) que expone las propiedades del Menú.
├── App.css             # Estilos de maquetación de vista contenedora.
└── index.css           # Estilos base y resets de aplicación.
```

---

## 🧠 Decisiones Técnicas Destacadas (3 Retos de Desarrollo)

Aunque la estructura parece sencilla a simple vista, el desarrollo resolvió tres retos de diseño que certifican calidad en la gestión del DOM en tecnologías de Frontend asíncronas:

### 1. Extracción y Derivación Dinámica de Estructuras de Datos
**El Reto**: Depender de "hardcoding" (escribir categorías estáticas) requiere que, al agendar nuevos productos en la base de datos, el desarrollador altere el frontend.
**La Solución**: En `App.js`, se utilizó manipulación avanzada de arreglos y el objeto nativo de JavaScript `Set`:  
`["all", ...new Set(items.map(...))]`  
Esto asegura que las interfaces de filtro (Categorías) se construyan dinámicamente de forma orgánica. Si agregáramos un producto en la base de datos `data.js` bajo una nueva categoría, el navegador la crearía sin necesidad de alterar código lógico.

### 2. "State Lifting" y Flujo Unidireccional Re-utilizable
**El Reto**: Mantener coordinada la barra de navegación entre los ítemes que se muestran para evitar un refresco brusco de la página.
**La Solución**: Adopción de patrón de elevación del estado (State Lifting). `App.js` mantiene la fuente principal de la Verdad (Data), pero inyecta los métodos `filterItems` y `activeCategory` directamente a sus "hijos" por medio de Prop Drilling. Esto favorece un ciclo One-Way Data Binding sumamente estable que reduce tiempos de depuración.

### 3. Retroalimentación Visual Reactiva (UI State Management)
**El Reto**: Que el sitio web se sienta en todo momento responsivo y claro en su navegación "in-state". 
**La Solución**: Componentización de `Categories.js` que no sólo dispara un evento, sino que se subordina al comparador `activeCategory === category`. Esto inserta la clase semántica `"filter-btn active"`, proporcionando a los usuarios un *feedback* visual inmediato mientras se recalcula de forma concurrente el render de la matriz (Grid) mostrada por `Menu.js`.

---

> **Uso y Escalado**: El diseño estructural escogido asegura que para escalar esta app comercialmente, bastaría con reemplazar la importación de `data.js` con un *Fetch* o iteración de promesas de una API RESTful sin tener que cambiar los demás componentes gráficos.
