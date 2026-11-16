# 📱 CookBook - Android Recipe & Discovery App

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="Android" />
  <img src="https://img.shields.io/badge/Language-Kotlin-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white" alt="Kotlin" />
  <img src="https://img.shields.io/badge/Architecture-MVVM%20%2B%20Clean-FF6F00?style=for-the-badge&logo=google&logoColor=white" alt="Architecture" />
  <img src="https://img.shields.io/badge/DI-Dagger%20Hilt-4285F4?style=for-the-badge&logo=dagger&logoColor=white" alt="Hilt" />
  <img src="https://img.shields.io/badge/Local%20DB-Room%20SQLite-00B0FF?style=for-the-badge&logo=sqlite&logoColor=white" alt="Room" />
  <img src="https://img.shields.io/badge/Networking-Retrofit2-29B6F6?style=for-the-badge&logo=square&logoColor=white" alt="Retrofit" />
  <img src="https://img.shields.io/badge/Cloud-Firebase-FFCA28?style=for-the-badge&logo=firebase&logoColor=black" alt="Firebase" />
  <img src="https://img.shields.io/badge/Maps-Google%20Maps%20SDK-EA4335?style=for-the-badge&logo=googlemaps&logoColor=white" alt="Google Maps" />
</p>

---

## 📌 Overview

**CookBook** is a native Android application built with **Kotlin** that delivers an offline-first recipe discovery and social culinary experience. 

The app seamlessly combines global recipe exploration powered by **TheMealDB REST API** with user-authored recipe publishing powered by **Firebase** (Auth, Firestore, Cloud Storage), all backed by an interactive **Google Maps** view linking culinary creations to geographical coordinates.

Engineered following Google's official architecture guidelines, it employs **MVVM**, **Clean Architecture Repositories**, **Dagger Hilt Dependency Injection**, **Room Local Persistence**, and **Kotlin Coroutines / Flow**.

---

## ✨ Key Features

- **⚡ Offline-First Single Source of Truth (SSOT):**
  - Seamless Room SQLite caching provides instantaneous UI loading even when offline.
  - Transparent cache invalidation and background remote refresh from TheMealDB API.
- **💉 Scalable Dependency Injection:**
  - Fully decoupled architecture using **Dagger Hilt** with modular injection (`DatabaseModule`, `FirebaseModule`, `NetworkModule`).
- **🌐 Dual-Source Culinary Content:**
  - **Global Recipes:** Fetched dynamically from TheMealDB API using **Retrofit 2** and **Gson**.
  - **Community Recipes:** Published by users and synchronized in real time via **Cloud Firestore**.
- **📍 Interactive Maps & Location Tagging:**
  - **Google Maps SDK** integration with custom map markers for geo-tagged recipes.
  - **FusedLocationProviderClient** for accurate device location tracking and nearby recipe discovery.
- **🔐 Cloud Authentication & Media Pipeline:**
  - Firebase Authentication (Email/Password registration and login).
  - High-resolution recipe photo uploads via **Firebase Cloud Storage**.
  - Custom EXIF orientation correction transformation with **Picasso**.
- **🎨 Modern Android UI:**
  - Android Jetpack **Navigation Component** managing single-activity fragment transactions.
  - Type-safe **ViewBinding** eliminating `findViewById` boilerplate.
  - Polished **Material Design 3** dialogs, loading indicators, and responsive layouts.

---

## 🏗️ Architecture & Tech Stack

The project adheres to modern Android Clean Architecture principles, clearly separating UI, Domain, and Data layers.

```
com.cookbook.app
├── CookBook.kt                     # Application entry point with @HiltAndroidApp
├── adapters/                       # RecyclerView adapters (RecipeAdapter, MealRecipeAdapter)
├── di/                             # Dagger Hilt Dependency Injection Modules
│   ├── DatabaseModule.kt           # Room DB & DAO providers
│   ├── FirebaseModule.kt           # FirebaseAuth, Firestore, Storage providers
│   └── NetworkModule.kt            # Retrofit & MealDbService providers
├── firebase/                       # Firebase Firestore & Storage repository abstractions
│   ├── FirebaseRepository.kt
│   └── FirebaseRepositoryImpl.kt
├── model/                          # Domain & Data Models
│   ├── Recipe.kt                   # User recipe model (Firestore & UI)
│   ├── MealRecipe.kt               # TheMealDB recipe entity (Room @Entity)
│   ├── MealDbResponse.kt           # Network DTOs
│   ├── RecipeLocation.kt           # Geographical coordinates model
│   └── User.kt                     # User profile entity
├── repository/                     # Single Source of Truth Repository
│   └── DatabaseRepository.kt       # Coordinates Room, Retrofit & Firebase data streams
├── retrofit/                       # Remote Networking Layer
│   ├── MealDbService.kt            # Retrofit REST API interface
│   ├── MealDbRepository.kt
│   └── MealDbRepositoryImpl.kt
├── room/                           # Local SQLite Persistence Layer
│   ├── AppDatabase.kt              # Room Database definition
│   └── AppDao.kt                   # Room Data Access Object (CRUD queries)
├── utils/                          # Common Utilities & Helpers
│   ├── Constants.kt                # UI dialogs, network helpers, formatting
│   ├── ExifTransformation.kt       # Image orientation normalization
│   ├── NetworkUtils.kt             # Connectivity observers
│   └── ValidationUtil.kt           # Form input validators
├── view/                           # Presentation Layer (Views & UI)
│   ├── activities/                 # SplashActivity, AuthActivity, MainActivity, BrowseRecipesActivity
│   └── fragments/                  # Feed, RecipeDetail, AddRecipe, EditRecipe, Map, Profile, EditProfile, SignIn, SignUp
└── viewmodel/                      # Architecture ViewModels
    ├── AuthViewModel.kt            # Handles authentication states & user sessions
    └── RecipeViewModel.kt          # Manages recipe feeds, queries, and map locations
```

---

## 🛠️ Technology Stack & Libraries

| Category | Technology | Purpose |
| :--- | :--- | :--- |
| **Language** | Kotlin 1.9+ | Core programming language |
| **Architecture** | MVVM + Repository Pattern | Clean separation of concerns & testability |
| **Dependency Injection** | Dagger Hilt | Compile-time dependency injection |
| **Local Database** | Room SQLite Persistence | Offline caching and fast local queries |
| **Network & REST** | Retrofit 2 + Gson Converter | TheMealDB REST API consumption |
| **Asynchronous** | Kotlin Coroutines & Flow | Non-blocking background concurrency |
| **Cloud Services** | Firebase Auth, Firestore, Storage | Authentication, NoSQL database & cloud media storage |
| **Maps & Location** | Google Maps SDK & Play Services Location | Interactive geographical recipe maps |
| **Image Loading** | Picasso + CircleImageView | Network image loading, caching & EXIF transformation |
| **Jetpack Components** | ViewModel, LiveData, Navigation, ViewBinding | Modern Android UI architecture components |

---

## 📱 App Flow & Screens

1. **Splash Screen:** Checks active authentication session and directs user to Auth or Main flow.
2. **Authentication (Sign In / Sign Up):** Validates credentials, integrates with Firebase Auth, stores user profile.
3. **Feed Fragment:** Dynamic dual-feed featuring user-published recipes and TheMealDB international meals with offline caching.
4. **Recipe Detail:** Comprehensive recipe breakdown displaying ingredients, cooking instructions, prep time, and author info.
5. **Add / Edit Recipe:** Form input with camera/gallery photo upload to Firebase Storage, location coordinate picker, and validation.
6. **Map View:** Google Map displaying interactive pins for geo-tagged recipes. Tapping a pin opens recipe details.
7. **Profile & Edit Profile:** Manage personal info, bio, avatar image, and view personal recipe portfolio.

---

## 🚀 Getting Started

### Prerequisites
- [Android Studio](https://developer.android.com/studio) Ladybug / Hedgehog or newer
- JDK 17 or higher
- Android SDK 35 (minSdk 24, targetSdk 34)

### Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone https://github.com/Michal200/CookBook-Android.git
   cd CookBook-Android
   ```

2. **Open in Android Studio:**
   - Select `Open an Existing Project` and navigate to the cloned directory.
   - Wait for Gradle sync to complete.

3. **Google Services & Maps API Key:**
   - The project includes `app/google-services.json` for Firebase setup.
   - Add your Google Maps API key in `app/src/main/AndroidManifest.xml` if replacing the default:
     ```xml
     <meta-data
         android:name="com.google.android.geo.API_KEY"
         android:value="YOUR_GOOGLE_MAPS_API_KEY" />
     ```

4. **Build & Run:**
   - Select an Android device or emulator running Android 7.0 (API 24) or higher.
   - Click **Run** (`Shift + F10`).

---

## 👩‍💻 Author

**Michal**  
- GitHub: [@Michal200](https://github.com/Michal200)
