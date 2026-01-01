# {Project Name} Flutter Testing Strategy

To meet the "Big Tech" standard (Google/Meta), we require a robust testing strategy that covers **80-90%** of the codebase. This ensures reliability, prevents regressions, and enables rapid iteration.

## The Testing Pyramid

We adhere to the standard testing pyramid:

1.  **Unit Tests (70%)** - Fast, isolated, reliable.
2.  **Widget Tests (20%)** - UI components, layout, interactions.
3.  **Integration Tests (10%)** - End-to-end flows on real devices/emulators.

---

## 1. Unit Testing
**Scope:** Domain Layer (UseCases, Entities), Data Layer (Repositories, Models, DataSources), and Presentation Logic (BLoCs/ViewModels).
**Tools:** `test`, `mockito` (or `mocktail`), `bloc_test`.

### Standards:
*   **Isolation:** Never make real network or database calls. Always mock external dependencies.
*   **Coverage:** 100% coverage for business logic (UseCases and BLoCs).
*   **Naming:** `should [expected result] when [condition]`.
    *   *Example:* `should emit [Loading, Loaded] when data is fetched successfully`.

### Example (BLoC Test):
```dart
blocTest<UserBloc, UserState>(
  'emits [UserLoading, UserLoaded] when FetchUser is added',
  build: () {
    when(() => mockRepo.getUser()).thenAnswer((_) async => User());
    return UserBloc(mockRepo);
  },
  act: (bloc) => bloc.add(FetchUser()),
  expect: () => [isA<UserLoading>(), isA<UserLoaded>()],
);
```

---

## 2. Widget Testing
**Scope:** Presentation Layer (Widgets, Pages). Verifies that the UI renders correctly and responds to user input (taps, text entry).
**Tools:** `flutter_test`, `network_image_mock`.

### Standards:
*   **Golden Tests:** Use Golden files to verify pixel-perfect UI against designs.
*   **Interaction:** Verify tapping buttons triggers the expected events.
*   **Mocking:** Pump widgets with mocked Providers/BLoCs to test different states (Loading, Error, Success).

### Example:
```dart
testWidgets('shows loading spinner when state is UserLoading', (tester) async {
  // Arrange
  when(() => mockBloc.state).thenReturn(UserLoading());

  // Act
  await tester.pumpWidget(createWidgetUnderTest());

  // Assert
  expect(find.byType(CircularProgressIndicator), findsOneWidget);
});
```

---

## 3. Integration Testing
**Scope:** Full app flows (e.g., Login -> Home -> Profile). Runs on a simulator or real device.
**Tools:** `integration_test` (Official Flutter package), `Maestro` (Optional external tool).

### Standards:
*   **Critical Paths:** Cover the "Happy Paths" and critical error flows.
*   **Performance:** Use integration tests to capture performance metrics (startup time, scroll smoothness).
*   **Environment:** Run against a Mock Server or a Staging Environment.

### Command:
`flutter test integration_test/app_test.dart`

---

## 4. Manual QA & Exploratory Testing
*   **Device Farm:** Test on a wide range of devices (Samsung, Pixel, various iOS iPhones) to catch fragmentation issues.
*   **Edge Cases:** Test low battery, airplane mode, backgrounding the app, font scaling.

## 5. CI/CD Integration
All tests must pass before merging.
*   **Pull Request:** Runs `flutter analyze`, Unit Tests, and Widget Tests.
*   **Nightly/Merge:** Runs Integration Tests (as they are slower).

## Change Log
| Change | Date | Version | Description | Author |
| :--- | :--- | :--- | :--- | :--- |
| Initial Draft | YYYY-MM-DD | 0.1 | Initial Testing Strategy | {Agent/Name} |
