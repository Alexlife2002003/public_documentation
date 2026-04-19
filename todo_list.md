# To-Do Application - Architectural Blueprint

## Overview
This architectural document outlines the high-level design, structure, and technical decisions behind the React-based To-Do application. This project was built emphasizing modularity, readable component design, and maintainable state management without relying on heavy external dependencies.

## Technology Stack
- **Framework:** React.js
- **Styling:** Tailwind CSS + PostCSS
- **State Management:** React Hooks (`useState`, `useEffect`)
- **Testing:** Jest + React Testing Library
- **Build / Deploy:** Create React App Webpack + GitHub Pages (`gh-pages`)

## Architectural Design Patterns

### Component-Centric Architecture
The application employs a strict component-based architecture where logical elements are separated into their own distinct directories. This enables high reusability and isolated maintainability.

### State Container Pattern
A central "Smart Component" (`App.js`) acts as the state container, managing the global application logic (ToDo items, active filters, and filtered views). It passes down state and callback handler functions via props to its child "Dumb Components" (presentation components), rigorously adhering to a unidirectional data flow.

## Project Structure

```text
root/
 ├── public/                  # Public assets (index.html, logos)
 ├── src/
 │   ├── App.js               # Main State Container component
 │   ├── index.js             # React DOM rendering entry point
 │   ├── index.css / App.css  # Base styling and Tailwind directives
 │   └── components/          # Modular UI Components
 │       ├── Title/           # Header and title presentation
 │       ├── Todo/            # Individual To-Do item presentation 
 │       ├── TodoFilters/     # Filter controls (All, Active, Completed)
 │       ├── TodoInput/       # User input handling and creation
 │       └── TodoList/        # List rendering and component mapping
 ├── tailwind.config.js       # Tailwind CSS configuration token
 ├── postcss.config.js        # PostCSS build behavior
 └── package.json             # Configuration and CI/CD Scripts
```

## Engineering Challenges Resolved

### 1. Scalable Component Modularity
**Context:** Monolithic files in frontend applications typically scale poorly as functional complexity grows.
**Resolution:** The project decouples the UI into strict, cohesive atomic directories (e.g., `TodoFilters`, `TodoInput`, `TodoList`). This folder-per-component pattern ensures logic, tests, and localized stylistic decisions are encapsulated. It mitigates global complexity, prevents merge conflicts, and boosts code maintainability.

### 2. Reactive Derived State Handling
**Context:** Managing changing "views" of data (All vs Active vs Completed) alongside the "source of truth" often leads to state desynchronization and memory leaks.
**Resolution:** Developed a lightweight, custom state management flow utilizing React Hooks (`useState`, `useEffect`). The project systematically derives `filteredTodos` automatically whenever the `activeFilter` string or the primary `todos` array mutates. This creates an optimized, bug-free rendering cycle without external heavy libraries like Redux.

### 3. Predictable Styling with Utility-First CSS
**Context:** Managing cascading side-effects from traditional CSS or CSS-in-JS abstractions in large projects is cumbersome.
**Resolution:** Implemented a robust styling pipeline with Tailwind CSS and PostCSS execution. Rather than managing complex semantic classes, styles are composed via utility tokens directly in the components. This addresses the challenge of global namespace collision, ensures highly responsive layouts, and builds an ultra-lean CSS artifact for final production.
