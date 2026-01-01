# {Project Name} Flutter Project Structure

This project follows a **Feature-First Clean Architecture** structure. This ensures scalability, decoupling, and easier navigation for large teams.

## Root Directory

```
/
├── android/            # Native Android code (Gradle, Manifest)
├── ios/                # Native iOS code (Runner, Info.plist)
├── assets/             # Images, fonts, icons, JSON files
├── lib/                # Main Flutter source code
├── test/               # Unit and Widget tests
├── integration_test/   # End-to-End integration tests
├── pubspec.yaml        # Dependencies and assets configuration
├── analysis_options.yaml # Linter rules
└── README.md           # Project documentation
```

## `lib/` Directory Structure

The `lib` folder is the heart of the application. It is divided into `core` (shared resources) and `features` (specific business verticals).

```
lib/
├── main_development.dart   # Entry point for Dev flavor
├── main_production.dart    # Entry point for Prod flavor
├── app.dart                # Root Widget (MaterialApp)
├── l10n/                   # Localization files (.arb)
├── core/                   # Shared/Common code accessible by all features
│   ├── config/             # Environment variables, Router config, Theme
│   ├── constants/          # App-wide constants (Assets, Strings, Styles)
│   ├── error/              # Failure classes, Exception definitions
│   ├── network/            # Dio client, Interceptors, API endpoints
│   ├── services/           # Third-party services (Analytics, Crashlytics)
│   ├── usecases/           # Base UseCase interface
│   └── widgets/            # Reusable generic widgets (Buttons, Inputs)
└── features/               # Feature-based modules (e.g., Auth, Feed, Profile)
    ├── feature_a/
    │   ├── data/           # Data Layer
    │   │   ├── datasources/    # Remote/Local data fetching
    │   │   ├── models/         # JSON serializable models (DTOs)
    │   │   └── repositories/   # Implementation of Domain Repository
    │   ├── domain/         # Domain Layer (Pure Dart)
    │   │   ├── entities/       # Business objects
    │   │   ├── repositories/   # Abstract Repository Interface
    │   │   └── usecases/       # Business logic units (e.g., LoginUser)
    │   └── presentation/   # Presentation Layer (Flutter)
    │       ├── bloc/           # BLoC / Cubit / Provider logic
    │       ├── pages/          # Full screen pages (Scaffold)
    │       └── widgets/        # Feature-specific widgets
    └── feature_b/
        └── ...
```

## Explanation of Layers

### 1. Domain (Inner Circle)
*   **Entities:** Plain Dart objects. Core business logic rules.
*   **Repositories (Interfaces):** Defines *what* the data layer must do, but not *how*.
*   **UseCases:** Encapsulates a specific business action (e.g., `GetMessages`, `UpdateProfile`).

### 2. Data (Outer Circle)
*   **Models:** Subclasses of Entities with JSON `fromJson`/`toJson` methods.
*   **DataSources:**
    *   `RemoteDataSource`: Calls REST APIs (Dio/Retrofit).
    *   `LocalDataSource`: Accesses DB (Hive/Isar) or Secure Storage.
*   **Repositories (Implementation):** Implements the Domain interface. Decides whether to fetch from Remote or Local (Caching strategy).

### 3. Presentation (Outer Circle)
*   **Pages:** The screens. Should be minimal and mostly scaffold code.
*   **Widgets:** Components used in pages.
*   **State Management (Bloc/Riverpod):** Holds the UI state and communicates with UseCases.

## Testing Structure

The `test/` directory mirrors the `lib/` structure exactly.

```
test/
├── core/
└── features/
    ├── feature_a/
    │   ├── data/
    │   │   ├── datasources/    # Mock API calls
    │   │   ├── models/         # JSON parsing tests
    │   │   └── repositories/   # Repository logic tests
    │   ├── domain/
    │   │   └── usecases/       # UseCase unit tests
    │   └── presentation/
    │       ├── bloc/           # State management tests (bloc_test)
    │       └── pages/          # Widget tests (golden tests, finder tests)
```
