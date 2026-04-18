# Architectural Blueprint: Rewards Client

Este documento presenta una descripción técnica de alto nivel de la arquitectura de la aplicación **Rewards client**, diseñada para ofrecer una experiencia de usuario premium en servicios de repostería y restaurante.

## 🏛️ Decisiones de Diseño Arquitectónico

La aplicación sigue un enfoque **Modular / Feature-First** implementado en **Flutter**. Esta decisión se tomó para garantizar la escalabilidad y facilitar el mantenimiento independiente de cada módulo crítico del negocio.

### Capas de la Aplicación:
1.  **UI/Features Layer**: Organización por dominio (`auth`, `menu`, `home`). Cada feature es autocontenida, manejando su propia lógica de presentación y navegación local.
2.  **Domain/Logic Layer**: El estado global se gestiona mediante un sistema de `Providers`, permitiendo una comunicación desacoplada entre servicios y la interfaz.
3.  **Data/Service Layer**: Implementación de servicios especializados para la comunicación asíncrona con **Supabase**, asegurando persistencia de datos y autenticación robusta.
4.  **Core & Utilities**: Abstracciones transversales que incluyen configuraciones de diseño (temas), helpers y constantes globales.

---

## 📂 Estructura de Carpetas (Blueprint)

```text
lib/
├── features/           # Módulos de negocio aislados
│   ├── auth/          # Gestión de identidad y acceso
│   ├── home/          # Experiencia principal y navegación raíz
│   └── menu/          # Catálogo interactivo de productos
├── core/               # Lógica compartida, temas y utilidades
├── services/           # Clientes de API y persistencia (Supabase)
├── providers/          # Gestión de estado reactivo
├── models/             # Estructuras de datos (Type-safety)
├── l10n/               # Internalización (Multi-lenguaje)
├── main.dart           # Punto de entrada de la aplicación
└── app.dart            # Configuración global de la aplicación (Router/Theme)
```

---

## 🛠️ Desafíos de Ingeniería Resueltos

Basado en la arquitectura implementada, se han abordado los siguientes retos técnicos críticos:

### 1. Escalabilidad mediante Modularización Estricta
Se implementó una separación clara por dominios (`features`). Esto resuelve el desafío de los "God Files" y permite que el equipo trabaje en el catálogo de productos (`menu`) sin afectar el flujo de autenticación, reduciendo la fricción en el desarrollo y facilitando las pruebas unitarias por componente.

### 2. Sincronización de Datos y Estado Persistente
La integración con servicios de backend en tiempo real (Supabase) a través de una capa de servicios dedicada permite una gestión eficiente de la latencia y asegura que el estado de la aplicación se mantenga sincronizado entre múltiples dispositivos, garantizando una experiencia fluida incluso en condiciones de red variables.

### 3. Gestión de Navegación Compleja y Seguridad
El uso de un sistema de navegación centralizado (`root_nav`) resuelve el desafío de manejar flujos condicionales de usuario. Permite controlar de manera determinista el acceso a rutas protegidas de la aplicación dependiendo del estado de autenticación, mejorando la seguridad y la experiencia de usuario final.
