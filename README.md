<p align="center">
  <img src="laz_logo.png" alt="LAZ Store" width="120">
</p>

<h1 align="center">LAZ Store</h1>
<p align="center">Tesla aftermarket parts e-commerce app for Android.</p>

<p align="center">
  <img src="https://img.shields.io/badge/Kotlin-7F52FF?logo=kotlin&logoColor=white" alt="Kotlin">
  <img src="https://img.shields.io/badge/Jetpack%20Compose-4285F4?logo=jetpackcompose&logoColor=white" alt="Jetpack Compose">
  <img src="https://img.shields.io/badge/Firebase-DD2C00?logo=firebase&logoColor=white" alt="Firebase">
  <img src="https://img.shields.io/badge/Material%203-757575?logo=materialdesign&logoColor=white" alt="Material 3">
</p>

---

<!-- TODO: Screenshots. Drop PNGs into a /screenshots folder and uncomment the block below.
<p align="center">
  <img src="screenshots/home.png" width="200" alt="Home">
  <img src="screenshots/ai-chat.png" width="200" alt="AI Chat">
  <img src="screenshots/cart.png" width="200" alt="Cart">
  <img src="screenshots/admin-dashboard.png" width="200" alt="Admin Dashboard">
</p>
-->

## What it is

LAZ Store is a production Android app for buying and selling Tesla aftermarket parts. Customers browse the catalog, identify unknown parts by uploading photos to an AI chat (powered by Claude and GPT-4), and check out with a cart that syncs across devices in real time. Admins and employees manage inventory, process orders and returns, and view sales analytics through dedicated role-based interfaces. The entire app is bilingual (Arabic/English).

## Features

**AI parts identification**
Upload a photo of any Tesla part. The app sends it to Claude or GPT-4 via OpenRouter for image analysis, then returns part matches, compatibility details, and pricing. Supports follow-up questions in a full chat interface.

**Three role-based interfaces**
Admin: dashboard with KPIs, full CRUD on products, complete order lifecycle management.
Employee: inventory browsing, order status updates, limited analytics.
Customer: catalog filtered to in-stock items, cart, checkout, order tracking and history.

**Real-time cart sync**
Cart state is backed by Firebase Realtime Database listeners. Add something on your phone, see it on your tablet instantly. Stock is held for 5 minutes per cart item with automatic cleanup.

**Push notifications**
TypeScript Cloud Functions watch for order status changes and trigger FCM push notifications to the relevant user.

**Bilingual UI**
Full Arabic and English support throughout the app.

**Order lifecycle**
Order tracking, returns processing with automatic stock restoration, history with status filtering. Currency displayed in JOD.

## Tech stack

| Layer | Technologies |
|-------|-------------|
| **Mobile** | Kotlin, Jetpack Compose, Material 3, Coil (image loading) |
| **Backend** | Firebase Auth, Realtime Database, Storage, Remote Config, Cloud Functions (TypeScript), FCM |
| **AI** | OpenRouter API, Claude, GPT-4, image analysis, conversational chat |
| **Architecture** | MVVM, StateFlow, Kotlin Coroutines, unidirectional data flow |

## Architecture

ViewModels expose immutable UI state via `StateFlow`. Compose screens subscribe with `collectAsState()` and send user intents back to ViewModels, keeping data flow strictly unidirectional. A centralized `PermissionManager` enforces role-based access at the ViewModel layer, so UI components never reason about authorization directly. Firebase Realtime Database listeners push updates through repositories into ViewModels, meaning the catalog, cart, and order screens all update live without polling.

For a detailed architecture walkthrough with diagrams, sequence flows, code excerpts, and the full permission model, see [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md).

<!-- TODO: Drop a 30-second demo GIF or screen recording here.
## Demo
<p align="center">
  <img src="screenshots/demo.gif" width="300" alt="Demo">
</p>
-->

## Run locally

1. **Clone the repo**
   ```bash
   git clone https://github.com/zzaaid03/laz-store.git
   ```

2. **Firebase setup**
   Create a Firebase project with Auth, Realtime Database, and Storage enabled. Download `google-services.json` and place it in the project root. The file is in `.gitignore`.

3. **OpenRouter API key**
   Add your key to `local.properties`:
   ```properties
   OPENROUTER_API_KEY=your_key_here
   ```

4. **Deploy Cloud Functions** (push notifications)
   ```bash
   cd functions && npm install && firebase deploy --only functions
   ```

5. **Build and run**
   Open in Android Studio, sync Gradle, run on a device or emulator running API 26+.

## Project structure

```
src/main/kotlin/com/laz/
  FirebaseMainActivity.kt           # App entry point
  models/                           # Domain models (Product, Order, User, Cart, etc.)
  ui/screens/                       # Compose screens per role and feature
  ui/components/                    # Reusable Compose components
  viewmodels/                       # MVVM ViewModels with StateFlow
  repositories/                     # Firebase data access layer
functions/                          # TypeScript Cloud Functions (FCM notifications)
```

---

Solo project, active development. Built as a student business by [@zzaaid03](https://github.com/zzaaid03).
