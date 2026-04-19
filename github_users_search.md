# 🏗️ Architectural Blueprint: GitHub Users Search

> **Nota:** Este documento detalla las decisiones arquitectónicas y el diseño técnico del proyecto "GitHub Users Search", una aplicación interactiva en React que interactúa de manera eficiente con las APIs públicas de GitHub.

## 📐 Visión General de la Arquitectura

El proyecto adopta una arquitectura basada en componentes fuertemente tipificada y orientada a la separación de responsabilidades. Se utiliza React para la construcción de interfaces declarativas y TailwindCSS para estilos utilitarios ágiles y de alta performance.

### Pestañas Tecnológicas Relevantes:
- **Core Library:** React 18
- **Routing:** React Router DOM (v6) para navegación en lado del cliente sin recargas ("Single-Page Application").
- **Estilizos:** Tailwind CSS (con configuraciones modulares y adaptativas).
- **Consumo de APIs:** API pública de GitHub.

---

## 📂 Organización y Estructura de Directorios

La estructura de carpetas asegura mantenibilidad, escalabilidad y una experiencia de desarrollo fluida.

```text
📦 github-users-search
 ┣ 📂 public/              # Archivos estáticos accesibles públicamente
 ┣ 📂 src/
 ┃ ┣ 📂 assets/            # Multimedia, íconos y archivos vectoriales
 ┃ ┣ 📂 components/        # Componentes UI reutilizables y atómicos
 ┃ ┃ ┣ 📜 Events.jsx                # Renderizado histórico de eventos
 ┃ ┃ ┣ 📜 Loading.jsx               # Spinners de carga contextual
 ┃ ┃ ┣ 📜 LoadingSkeletonUsersContainer.jsx # Skeletons para UX fluida
 ┃ ┃ ┣ 📜 Repo.jsx                  # Tarjeta modular para repositorios
 ┃ ┃ ┣ 📜 Tabs.jsx                  # Navegador interno interactivo
 ┃ ┃ ┗ 📜 UsersContainer.jsx        # Lista de mampostería (Grid) de usuarios
 ┃ ┣ 📂 Routes/            # Componentes jerárquicos (Páginas/Vistas)
 ┃ ┃ ┣ 📜 UserInfo.jsx              # Detalle granular del usuario, tabs asíncronos
 ┃ ┃ ┗ 📜 Users.jsx                 # Layout principal, listado general y buscador
 ┃ ┣ 📜 App.jsx            # Contenedor de Providers (Router) y diseño base (Layout)
 ┃ ┗ 📜 index.js           # Punto de entrada de la aplicación y root del DOM
 ┣ 📜 tailwind.config.js   # Configuraciones del Design System
 ┗ 📜 package.json         # Gestión de dependencias y scripts del entorno
```

### Decisiones de Diseño por Capa

1. **Capa UI (Components):** 
   Los componentes en el directorio `components` son en su mayoría presentacionales (Dumb Components). Fueron fraccionados de forma atómica para máxima reusabilidad. Por ejemplo, el `UsersContainer` es reutilizado no solo en los resultados de búsqueda generales en `Routes/Users.jsx`, sino también en la vista de *Seguidores* de un perfil detallado.
2. **Capa Vista/Estado (Routes):** 
   La lógica de estado pesado y llamadas a red se traslada a los componentes de rutas (Smart Components). Mantienen la comunicación con el exterior y delegan las propiedades y estilos a los componentes menores.

---

## 🚀 Desafíos de Ingeniería Resueltos

Para garantizar una experiencia ágil, responsiva y orientada a los detalles (como requiere el consumo masivo de listas de usuarios), se implementaron resoluciones técnicas clave:

### 1️⃣ Optimización del Flujo de Rendering y "Skeleton Screens" (UX Oculta)
**Desafío:** El consumo de APIs públicas tiende a tener tiempos de respuesta variables. Mostrar una pantalla vacía o un solo loader disminuye dramáticamente la percepción del rendimiento del usuario.
**Solución:** Se diseñó implementó un componente `LoadingSkeletonUsersContainer`. En la vista inicial de `Routes/Users.jsx`, mientras la llamada a red principal se ejecuta, los esqueletos previenen el Cumulative Layout Shift (CLS) construyendo la cuadricula de antemano y mejorando drásticamente el flujo de espera sin perjudicar el tiempo de interactividad.

### 2️⃣ Modularidad Intencionada y Composición (Clean Code)
**Desafío:** Evitar el "Prop Drilling" (paso excesivo de propiedades) y mantener el archivo de la página asíncrona de usuarios lo suficientemente delgado para futuras expansiones.
**Solución:** Separación agresiva entre vistas (`Routes/*`) y componentes (`components/*`). Componentes clave como las pestañas de detalles (`Tabs.jsx`), el detalle del repositorio (`Repo.jsx`), y listados de eventos (`Events.jsx`) son unidades agnósticas a su modelo padre, permitiendo un manejo condicional del renderizado dentro del perfil granular de la ruta dinámica (`UserInfo.jsx`).

### 3️⃣ Centralización asíncrona mediante Manejo de Estado Dependiente (Hooks Efectivos)
**Desafío:** En el componente de Detalle de Usuario (`Routes/UserInfo.jsx`), los datos adicionales que el usuario solicita (por ejemplo: ver repositorios vs. ver seguidores) deben depender de la pestaña actual (`type`) del estado de navegación, requiriendo nuevas llamadas a red.
**Solución:** Se implementó una sincronización eficaz aprovechando el arreglo de dependencias (`Dependency Array`) del `useEffect`, de forma que al actualizar el pathname (Ruta reactiva) o la categoría de información (Repositorios, Eventos, Seguidores), el componente dispara mutaciones asíncronas precisas para el nuevo arreglo de datos de la sub-lista.
