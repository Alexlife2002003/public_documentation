# 🚀 CASE Usuario: Architectural Blueprint & System Design

Este repositorio contiene la **documentación de arquitectura y diseño de sistemas** de la aplicación móvil **CASE Usuario**. El código fuente lógico detallado se mantiene en reservado para proteger la propiedad intelectual de la implementación, mientras que este documento explone la estructuración técnica, la infraestructura y las decisiones de ingeniería detrás del proyecto.

## 🛠️ Stack Tecnológico
- **Core**: [Flutter](https://flutter.dev/) SDK (Dart `v3.4.3+`)
- **Gestión de Estado**: `provider` (Inyección de dependencias para el manejo de estados escalable).
- **Backend-as-a-Service**: [Supabase](https://supabase.com/) (`supabase_flutter` y `supabase_auth_ui` para identidades y persistencia en tiempo real).
- **Control de Eventos**: `easy_debounce` para mitigar la sobrecarga de peticiones backend ante ráfagas de interacciones.
- **Micro-Interacciones UI**: `flutter_spinkit` para la entrega de feedback visual eficiente durante cargas de red.

---

## 🏗️ Estructura del Sistema (System Architecture)

La aplicación sigue una arquitectura guiada por características (**Feature-First Modular Design**), lo que optimiza la mantenibilidad y delimita correctamente las fronteras lógicas:

```text
lib/
├── pages/                    # Subsistemas de interfaz agrupados por dominio de negocio:
│   ├── AuthWrapper.dart      # Router Centinela: Orquesta la navegación basada en estado de sesión
│   ├── customDrawer.dart     # Estructura de navegación lateral principal globalizada
│   ├── home/                 # Subsistema de Dashboard y flujo post-login
│   ├── login/                # Subsistema de autenticación de identidades existentes
│   ├── quiz/                 # Subsistema de encuestas interactivo y autónomo
│   ├── registro/             # Subsistema de onboarding y captura de nuevas cuentas
│   └── widgets/              # UI atómica específica de subsistemas aislados
├── colors.dart               # Sistema de Diseño: Tokens globales y paleta de marca centralizada
└── main.dart                 # Root Orchestrator: Inyecta dependencias e inicializa BaaS
```

## 🧠 3 Desafíos de Ingeniería Resueltos

Basándome en la estructura y dependencias de la plataforma, abordé estos 3 grandes desafíos técnicos:

1. **Gestión Reactiva de Rutas Protegidas (AuthWrapper Paradigm)**
   En lugar de inyectar rutas mediante comprobaciones secuenciales al cargar componentes, delegué la responsabilidad a un `AuthWrapper`. Este patrón observa el listener asíncrono de *Supabase* en tiempo real; si el token expira o el usuario es purgado de la sesión remotamente, la aplicación lo traslada instantáneamente del módulo `home` al módulo de `login` sin colgar la interfaz gráfica de usuario.

2. **Modularidad y Escalabilidad Vertical por "Features"** 
   Se refactorizó el paradigma genérico de organización ("controllers vs views") dividiendo el código en carpetas de característica (`home`, `quiz`, `registro`). Al agrupar los `widgets` o reglas exclusivas del módulo `quiz` dentro de su propio ecosistema, reduje exponencialmente las colisiones en Git y garantizo que cualquier nuevo módulo (ej. `payments`) se pueda integrar sin enredar otras lógicas ya aprobadas en producción.

3. **Optimización de Desempeño Bidireccional (Debouncing & Inyección)** 
   La conjunción de `provider` de inyección selectiva y `easy_debounce` resolvió los bloqueos (janks) que surgían en operaciones asíncronas intensas. El debouncing reduce el desborde de writes/reads concurrentes hacia la BD de Supabase garantizando un mejor uso de los recursos de la capa gratuita, mientras el *Provider* actualiza únicamente los fragmentos atómicos de UI que han mutado, omitiendo la recomposición de toda la rama principal de navegación.
