# Riesgo Suicida - Architectural Blueprint 🚀

This document serves as a high-level architectural overview of the **Riesgo Suicida** application. It is intended to showcase the core engineering decisions, system design, and folder structure without exposing sensitive source code or proprietary logic.

## 🏗 System Architecture

The application is built upon a modern, cross-platform architecture utilizing **Flutter** for the frontend and **Firebase** for the Backend-as-a-Service (BaaS). This decoupled approach ensures quick iteration, reliable background synchronization, and a seamless native experience across devices.

### Tech Stack
*   **Frontend Framework**: Flutter (Dart)
*   **Backend & Database**: Firebase (Cloud Firestore for real-time document storage, Firebase Auth for secure login)
*   **Key Native Integrations**: `flutter_phone_direct_caller` (for immediate emergency dialing), `url_launcher` (external resources).
*   **Environment Security**: `flutter_dotenv` (for secure runtime injection of environment variables).

---

## 📂 Structural Design (Folder Tree)

The project follows a **Feature-Driven** and **Role-Based** folder structure. This modular approach allows for scalable development by isolating Administrative logic from End-User flows.

```text
lib/
├── Admin/                     # 🛡️ Core logic & UI strictly for Administrative roles
│   ├── Answers/               # Views for tracking user submissions (APGAR, Plutchik, etc.)
│   └── Screens/               # Admin dashboards, contact management (agregar/editar)
├── User/                      # 🧑‍💻 Primary feature module for end-users
│   ├── Screens/               # Educational resources (Mitos, Factores) and Dashboards
│   └── multchoice/            # Interactive quiz engines (Models & Stateful components)
├── login/                     # 🔐 Authentication flows (Login, Register, Forgot Password)
├── Answers/                   # 📊 Shared evaluation logic and result mappers
├── assets/                    # 🖼️ Static resources (Images, GIFs, Env configuration)
├── firebase_options.dart      # ⚙️ Auto-generated Firebase initialization
├── main.dart                  # 🚀 Application entry point and route initialization
└── utils.dart                 # 🛠️ Shared helper functions & generic utilities
```

---

## 🧠 Core Engineering Decisions

1. **Strict Role-Based Access Control (RBAC):** By physically separating the `Admin/` and `User/` modules in the file tree, we minimize the risk of exposing privileged endpoints or sensitive analytical data to standard users. This also simplifies routing logic and authorization middleware.
2. **Abstracted Evaluation Engine:** The surveys (APGAR, Desesperanza, Ideacion, Plutchik) are decoupled into reusable interactive components (`first_quiz`, `Second_quiz`, etc.) via the `multchoice` folder, allowing rapid deployment of new psychological evaluation forms.
3. **Direct OS Integration for Emergencies:** Emergency contact functionality was prioritized by using direct native bridges (`flutter_phone_direct_caller`), bypassing standard dialer prompt flows when explicit immediate action is required by the user in crisis.

---

## 🏆 Engineering Challenges Solved

Below are three specific technical challenges successfully addressed during the development lifecycle:

### 1. Modularizing Asynchronous Survey Flows
**Challenge**: Handling complex, multi-stage psychological assessments (APGAR, Plutchik, etc.) without cluttering the UI thread or creating monolithic "Screen" files.
**Solution**: Designed a decoupled `multchoice` architecture mapping isolated JSON-like data models (`question.dart`, `answer.dart`) to robust stateful widgets (`quiz.dart`). This ensures that UI updates based on user responses happen instantly, while final score calculations and Firebase payload transmissions run asynchronously in the background.

### 2. High-Priority Native Action Routing
**Challenge**: Ensuring that users in a state of crisis have zero latency and minimal friction when attempting to seek help.
**Solution**: Implemented deep OS integrations utilizing `flutter_phone_direct_caller` combined with robust permission handling (`permission_handler`). This circumvents the native "copy-paste" or "open dialer" standard behaviors, enabling one-touch instant dialing for emergency contacts while gracefully failing over to `url_launcher` if device hardware limits apply.

### 3. Scalable Data Synchronization with Firebase
**Challenge**: Securely managing real-time data sync for dynamic support contacts, user data, and quiz results without compromising app performance.
**Solution**: Leveraged **Cloud Firestore** streams matched with a secure `.env` configuration layer (`flutter_dotenv`). The database schema is cleanly mirrored by the app's internal component structure, allowing the dashboard and contact lists to reflect instant updates from the `Admin` management tools.
