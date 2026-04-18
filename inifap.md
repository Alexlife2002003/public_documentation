# INIFAP: Architectural Blueprint

## Overview
This document outlines the architectural decisions and structural design of the **INIFAP** project, a Flutter-based application dedicated to the real-time monitoring and visualization of meteorological data from varied stations. The architecture is designed for scalability, local-first data resilience, and dynamic visualization.

---

## 🏗 Directory Structure & Blueprint

The project follows a modular Flutter architecture, separating logical concerns from UI presentation to ensure maintainability and testability.

```text
lib/
├── backend/            # Core Business Logic & Infrastructure
│   ├── fetch_data.dart     # Asynchronous API orchestration & background processes
│   ├── navegacion.dart      # Centralized routing and flow control logic
│   └── validaciones.dart    # Logic for data integrity and input sanitization
├── screens/            # UI Layer (Feature-based views)
│   ├── map_screen.dart      # Geospatial integration and interactive mapping
│   ├── grafica.dart         # Dynamic data visualization (Temporal analysis)
│   └── resumen_real.dart    # Real-time dashboard implementation
├── widgets/            # Atomized UI Components
│   ├── weather_card.dart    # Reusable platform-agnostic data card
│   └── icons/               # Custom asset management and icon transformations
├── datos/              # Data Layer
│   └── datos.dart           # Static definitions, models, and constants
└── assets/             # Project-wide resources
    └── puntos_cardinales/   # Specialized SVG/PNG assets for wind direction
```

---

## 🛠 Design Decisions

### 1. Centralized Logic & Infrastructure (`/backend`)
Rather than coupling logic within the `Widget` tree, the project extracts all API interaction, storage management (Secure Storage), and notification scheduling into a dedicated `backend` module. This ensures that the UI remains as a pure reflection of the application state.

### 2. Persistence-First Data Strategy
To handle intermittent connectivity in remote agricultural areas, the architecture implements a persistent caching layer. Data is fetched asynchronously and immediately committed to `FlutterSecureStorage`, allowing the app to remain functional and display the last known state without a live connection.

### 3. Modular Navigation & Reusable Atomized Widgets
The separation of `screens` and `widgets` allows for a highly modular UI. Complex screens like `resumen_avance_mensual` and `resumen_real` share an underlying set of atomized widgets (e.g., `WeatherCard`), ensuring visual consistency and significantly reducing code duplication across the platform.

---

## 🚀 Engineering Challenges Solved

### 1. Resilient Data Synchronization & Background Notifications
**Problem**: Maintaining data freshiness while handling API downtime and ensuring users are notified of updates without requiring the app to be active.
**Solution**: Orchestrated a robust sync system in `fetch_data.dart` that integrates `http` for fetching, `SecureStorage` for persistence, and `FlutterLocalNotifications` for background alerts. This creates a resilient loop that survives network failures and keeps users informed.

### 2. Dynamic Temporal Data Visualization
**Problem**: Visualizing multiple tiers of meteorological data (Real-time, Historical, and Monthly trends) through a unified interface.
**Solution**: Architected a dynamic visualization engine (`grafica.dart`) that handles varied data schemas and maps them onto interactive charts. This allows users to switch between granular hourly reports and high-level monthly summaries with minimal latency.

### 3. Cross-Platform Asset & Icon Management
**Problem**: Rendering precise meteorological indicators (like wind direction via cardinal points) consistently across mobile and web platforms using mixed media (SVG and PNG).
**Solution**: Developed a custom asset management system in `lib/widgets/icons` and `lib/assets`, utilizing SVG for precision and PNG for platform compatibility, coupled with logical rotations to visually represent complex environmental vectors.

---
*This document was generated to highlight the architectural integrity and engineering solutions implemented in the INIFAP project for portfolio documentation purposes.*
