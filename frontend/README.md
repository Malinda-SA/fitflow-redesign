# FitFlow Frontend

Flutter-based cross-platform client for iOS, Android, and Web.

## Overview

The Flutter frontend is the user-facing layer of FitFlow. It communicates with the NestJS backend via REST and WebSocket, uses native platform channels to access HealthKit (iOS) and Google Fit (Android), and authenticates users via Auth0's universal login.

## Prerequisites

- Flutter SDK 3.x ([install guide](https://docs.flutter.dev/get-started/install))
- Dart 3.x (bundled with Flutter)
- Android Studio or Xcode (for device emulators)
- Auth0 tenant credentials (set in `.env`)

## Setup

```bash
# Install dependencies
flutter pub get

# Run on connected device / emulator
flutter run

# Run on web
flutter run -d chrome

# Build production APK
flutter build apk --release

# Build iOS (requires macOS + Xcode)
flutter build ios --release
```

## Folder Structure (planned)

```
frontend/
├── lib/
│   ├── main.dart            # App entry point
│   ├── app.dart             # Root widget and routing
│   ├── features/
│   │   ├── auth/            # Auth0 login / logout
│   │   ├── dashboard/       # Home dashboard
│   │   ├── workout/         # Workout tracking
│   │   ├── nutrition/       # Nutrition logging
│   │   └── social/          # Activity feed
│   ├── core/
│   │   ├── api/             # HTTP client (Dio)
│   │   ├── models/          # Data models
│   │   └── utils/           # Helpers and extensions
│   └── shared/
│       └── widgets/         # Reusable UI components
├── test/
│   └── widget_test.dart
└── pubspec.yaml
```

## Environment Variables

Create a `.env` file (never commit this):

```
AUTH0_DOMAIN=your-tenant.auth0.com
AUTH0_CLIENT_ID=your-client-id
API_BASE_URL=https://api.fitflow.app
```
