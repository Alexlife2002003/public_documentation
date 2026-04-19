# CinemaMagic - Architectural Blueprint 🎬

This document outlines the high-level architecture and design decisions for **CinemaMagic**, serving as a technical blueprint of the application's structure.

## 🏗️ Folder Structure

The project follows a feature-centric, modular architecture to ensure scalability, ease of maintenance, and separation of concerns.

```text
src/
├── assets/             # Static assets like images and global icons
├── components/         # Reusable, self-contained UI components (e.g., carousel, movieCard, videoPopup)
│   ├── carousel/
│   ├── circleRating/
│   ├── contentWrapper/
│   ├── footer/
│   ├── header/
│   ├── lazyLoadImage/
│   └── ...
├── hooks/              # Custom React hooks encapsulating business and layout logic
├── pages/              # Route-level components grouping features into single views
│   ├── 404/
│   ├── details/
│   ├── explore/
│   ├── home/
│   └── searchResult/
├── store/              # Global state management configuration (Redux Toolkit)
│   ├── homeSlice.js
│   └── store.js
├── utils/              # Helper functions and API configuration (Axios instances)
│   └── api.js
├── App.jsx             # Root component mapping the layout and router logic
├── index.scss          # Global entry point for styling structure
├── mixins.scss         # SCSS mixins and responsive design variables
└── main.jsx            # React application entry point (DOM attachment)
```

## 🛠️ Stack & Core Technologies
- **Core UI:** React 18 (+ Vite for lightning-fast build tooling)
- **State Architecture:** Redux Toolkit 
- **Routing Layer:** React Router DOM (v6)
- **Network Interface:** Axios (Custom centralized API handlers)
- **Style System:** SCSS (Modular scoping and global mixins)

## 🚀 Key Engineering Challenges Solved

Based on the architectural structure, the following complex engineering challenges were successfully addressed:

### 1. Advanced Performance Optimization & Rendering Control
**Challenge**: Rendering large lists of heavy media elements (movie posters) without degrading layout stability or maxing out memory during long scrolling sessions.
**Solution**: Designed a custom `lazyLoadImage` wrapper and combined it with dynamic infinite pagination on generic view layers (like `explore` and `searchResult`). By implementing these patterns, only elements inside or near the viewport consume network and rendering resources, drastically reducing initial load times and preserving high framerates.

### 2. Predictable State Architecture & Decoupling
**Challenge**: Preventing "prop-drilling" and maintaining data consistency (such as system configuration, image URLs, or genre mapping) across deeply nested view trees.
**Solution**: Architected a robust implementation of Redux Toolkit (`store/`). By separating critical asynchronous metadata into an independent `homeSlice.js`, UI components remain purely functional and deterministic without knowing how the data is fetched or where it's stored.

### 3. Modular Reusability & Layout Consistency
**Challenge**: Enforcing a unified design system and structural constraints across multiple distinct pages showing varying data types (movies vs. TV shows).
**Solution**: Built a strictly modular component library within `components/`, featuring primitives like `movieCard`, `carousel`, and `circleRating`. To ensure perfect responsive boundaries, a global `contentWrapper` semantic structural component and SCSS `mixins.scss` were implemented. This ensures structural CSS logic is written once and inherited everywhere (DRY principle).
