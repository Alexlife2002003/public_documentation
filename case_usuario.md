# 🚀 CASE Usuario: Architectural Blueprint & System Design

This repository contains the **architecture and system design documentation** for the **CASE Usuario** mobile application. The core logical source code is kept in a private repository to protect the intellectual property of the implementation. However, this document details the technical structure, underlying infrastructure, and the engineering decisions that power the project.

## 🛠️ Technology Stack
- **Core**: [Flutter](https://flutter.dev/) SDK (Dart `v3.4.3+`)
- **State Management**: `provider` (Dependency injection for scalable state handling).
- **Backend-as-a-Service**: [Supabase](https://supabase.com/) (`supabase_flutter` and `supabase_auth_ui` for identity management and real-time persistence).
- **Event Control**: `easy_debounce` to mitigate backend request overload during rapid user interactions.
- **UI Micro-Interactions**: `flutter_spinkit` for efficient visual feedback and smooth loading indicators during network operations.

---

## 🏗️ System Architecture

The application follows a **Feature-First Modular Design** architecture. This modern pattern optimizes maintainability and clearly defines the logical boundaries between different sections of the app:

```text
lib/
├── pages/                    # UI Subsystems grouped by business domain:
│   ├── AuthWrapper.dart      # Sentinel Router: Orchestrates navigation based on session state
│   ├── customDrawer.dart     # Globalized main lateral navigation structure
│   ├── home/                 # Dashboard subsystem and post-login flow
│   ├── login/                # Existing identity authentication subsystem
│   ├── quiz/                 # Interactive and autonomous survey subsystem
│   ├── registro/             # Onboarding and new account capture subsystem
│   └── widgets/              # Atomic UI specific to isolated subsystems
├── colors.dart               # Design System: Global tokens and centralized brand palette
└── main.dart                 # Root Orchestrator: Injects dependencies and initializes BaaS
```

## 🧠 3 Key Engineering Challenges Solved

Based on the structure and dependencies of the platform, the following three major technical challenges were successfully addressed:

1. **Reactive Protected Route Management (AuthWrapper Paradigm)**
   Instead of securing routes via synchronous checks during component loads, the responsibility was delegated to a centralized `AuthWrapper`. This pattern observes the asynchronous *Supabase* state stream in real-time. If an auth token expires or if a user is revoked remotely, the application instantly transitions the user from the protected `home` environment back to the `login` flow, avoiding UI freezes or unauthorized blink states.

2. **Vertical Modularity & Feature-Based Scalability** 
   The traditional organizational paradigm ("controllers vs. views") was refactored by splitting the codebase into feature-based modules (`home`, `quiz`, `registro`). By encapsulating the `widgets` and specific business rules of the `quiz` module inside its own self-contained folder ecosystem, Git merge conflicts were significantly reduced. This guarantees that implementing any new module (e.g., payments) won't inadvertently break stable logic already running in production.

3. **Bidirectional Performance Optimization (Debouncing & Injection)** 
   The combination of selective UI rebuilding (via `provider`) and interaction throttling (via `easy_debounce`) resolved UI jank during resource-intensive asynchronous operations. Debouncing handles the overflow of concurrent read/write requests to the Supabase database—drastically lowering bandwidth and preventing free-tier quota exhaustion. Meanwhile, the Provider is scoped to trigger granular redraws only on atomic UI fragments that mutated, skipping the costly recomposition of the main navigation tree entirely.
