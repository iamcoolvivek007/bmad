# {Project Name} Flutter Coding Standards

## 1. Architectural Patterns
Adherence to **Clean Architecture** is mandatory to ensure scalability and testability, mirroring "Big Tech" standards.

*   **Layer Separation:**
    *   **Domain (Inner Layer):** Pure Dart. Entities, UseCases, Repository Interfaces. No Flutter dependencies.
    *   **Data (Outer Layer):** Repository Implementations, Data Sources (API/DB), DTOs (Data Transfer Objects).
    *   **Presentation (Outer Layer):** UI (Widgets), State Management (Bloc/Riverpod).
*   **Dependency Rule:** Dependencies only point inwards. Domain knows nothing about Data or Presentation.

## 2. Code Style & Linting
*   **Linter:** Strict adherence to `very_good_analysis` or `flutter_lints` (recommended).
*   **Formatting:** Run `dart format .` before every commit.
*   **Naming Conventions:**
    *   Classes/Enums: `PascalCase` (e.g., `user_repository.dart` -> `UserRepository`)
    *   Variables/Methods: `camelCase` (e.g., `fetchUserData`)
    *   Files: `snake_case` (e.g., `user_profile_screen.dart`)
    *   Constants: `lowerCamelCase` (preferred in Dart over SCREAMING_SNAKE)
*   **Imports:** Sort imports alphabetically. Put `package:` imports before relative imports.

## 3. State Management Best Practices
*   **Unidirectional Data Flow:** UI -> Event/Action -> Logic -> State -> UI.
*   **Immutability:** All States and Events must be immutable (use `Equatable` or `Freezed`).
*   **Logic in UI:** Strictly forbidden. No complex logic in `build()` methods. Move it to the Bloc/Controller.

## 4. Performance Optimization
*   **const Constructors:** Use `const` for widgets whenever possible to reduce garbage collection.
*   **Build Context:** Avoid watching/listening to state at the top of a large widget tree. Scope listeners as low as possible.
*   **Lists:** Use `ListView.builder` for long lists.
*   **Images:** Cache images using `cached_network_image`.
*   **Profiling:** Regularly check for re-builds using the Flutter Performance Overlay.

## 5. Error Handling & Logging
*   **Exceptions:** Catch specific exceptions, not generic `catch (e)`.
*   **Result Type:** Use a Functional Programming approach (e.g., `dartz` or `fpdart`) to return `Either<Failure, Success>` instead of throwing exceptions in the Domain layer.
*   **Logging:** Use a logger (e.g., `logger` package) instead of `print()`.
    *   `print()` is stripped in release builds, but structured logging is better for debugging.

## 6. Security
*   **Sensitive Data:** Never commit API keys to Git. Use `.env` files and `flutter_dotenv`.
*   **Storage:** Use `flutter_secure_storage` for tokens/credentials.
*   **Network:** Enforce HTTPS. Implement SSL Pinning for high security.
*   **Obfuscation:** Build release apps with `--obfuscate --split-debug-info`.

## 7. Accessibility (a11y)
*   **Semantics:** Wrap interactive elements in `Semantics` widgets if necessary.
*   **Contrast:** Ensure text/background contrast ratios meet WCAG AA standards.
*   **Text Scale:** UI must handle dynamic text scaling (user font size settings).

## 8. Internationalization (l10n)
*   No hardcoded strings in the UI.
*   Use `flutter_localizations` and `.arb` files for all user-facing text.

## 9. Change Log
| Change | Date | Version | Description | Author |
| :--- | :--- | :--- | :--- | :--- |
| Initial Draft | YYYY-MM-DD | 0.1 | Initial Standards | {Agent/Name} |
