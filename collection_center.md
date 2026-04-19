# 🏛️ Collection Center - Architectural Blueprint

This document outlines the architectural decisions, design patterns, and structural organization implemented in **Collection Center**, demonstrating best practices in modular and scalable Flutter development.

## 📁 System Architecture & Design Pattern

The project utilizes a structured **Model-View-Presenter (MVP)** inspired approach, mapped out via `View` and `Presenter` layers. This establishes a clean separation of concerns, ensuring that the visual components remain fully decoupled from the core business logic, asynchronous operations, and database interactions.

### Folder Structure Overview

```text
lib/
├── Presenter/            # Core business logic, controllers, and database interactions
│   ├── Amigos.dart       # Logic for friend requests, validations, and social queries
│   ├── Categorias.dart   # Data management and CRUD logic for user categories
│   ├── Cuentas.dart      # Authentication flows, session handling, and user data
│   └── Objects.dart      # Collectible items lifecycle and cloud storage handling
├── View/                 # UI Components, Screens, and Layouts
│   ├── Amigos/           # Interfaces for social networking, finding friends, and viewing shared collections
│   ├── Categorias/       # Screens for rendering category lists and submission forms
│   ├── Cuentas/          # Registration, login, and user profile interfaces
│   ├── Objects/          # Detail views, grids, and formulation for collectible objects
│   └── recursos/         # Shared, reusable UI components and styled widgets
├── assets/               # Local static assets (images, icons) and environment config (.env)
├── firebase_options.dart # Emitted Firebase configurations for multi-platform support
└── main.dart             # Application initialization, root widget, and main routing
```

## 🛠️ Key Engineering Challenges Solved

Based on the system's architecture and technology stack, here are three significant engineering challenges successfully addressed during development:

### 1. Robust Architectural Separation (Modularity)
By splitting the application into distinct `Presenter` and `View` directories based on feature domains (Amigos, Categorias, Cuentas, Objects), the application achieves high maintainability and scalability. `Presenter` modules act as dedicated controllers handling complex data transactions with Firebase. This design allows `View` components to stay strictly focused on rendering state, significantly decreasing "spaghetti code" and making future integrations or UI overhauls vastly efficient.

### 2. Complex Relational Data & Real-time Graphing
Building a social "Collection Sharing" feature required structuring and querying nested NoSQL data correctly. Overcoming the challenge of asynchronous interactions between Firebase Auth and dynamic Cloud Firestore paths (e.g., querying nested `Users -> Categories -> Objects` while adhering to security rules). The logic in `Presenter/Amigos.dart` demonstrates the capability to securely execute two-way relational handshakes (requests/acceptances) to build social graphs seamlessly.

### 3. Media Management & Caching Optimization
Because visualizing collectibles is central to the user experience, managing media efficiently is crucial. The architecture leverages `image_picker` alongside `firebase_storage` to capture and upload media assets securely. To mitigate performance bottlenecks, minimize latency, and reduce data bandwidth, the system is designed to consume `cached_network_image`. This ensures smooth UI scrolling through dense graphical lists of high-resolution artifacts, keeping the app performant across varying network conditions.
