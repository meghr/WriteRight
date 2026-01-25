# WriteRight - Android AI Typing Assistant

## 📱 Implementation Plan v2.1

> A floating overlay assistant that automatically converts user-typed text into grammatically correct and polite language, working across all Android applications.

---

## 📋 Table of Contents

1. [Project Overview](#project-overview)
2. [Development Environment](#development-environment)
3. [Solution Architecture](#solution-architecture)
4. [Technology Stack](#technology-stack)
5. [User Flows](#user-flows)
6. [Development Phases](#development-phases)
7. [Task Breakdown](#task-breakdown)
8. [API Integration](#api-integration)
9. [Security & Privacy](#security--privacy)
10. [Testing Strategy](#testing-strategy)
11. [Deployment & Distribution](#deployment--distribution)

---

## 🎯 Project Overview

### Problem Statement
Users struggle to write grammatically correct, polite, and professional sentences while typing on mobile devices. Existing solutions require manual copy-paste or are limited to specific apps.

### Solution
**WriteRight Floating Assistant** - A lightweight Android app that provides:
- 🫧 Floating bubble accessible from any app
- 📋 Clipboard-based text correction
- 🤖 Gemini AI-powered grammar and politeness enhancement
- ⚡ Quick one-tap correction workflow

### Why Floating Overlay (Not Custom Keyboard)?

| Aspect | Custom Keyboard | Floating Overlay ✅ |
|--------|-----------------|---------------------|
| Development Time | 12 weeks | 6-8 weeks |
| User Trust | Low (keystroke access) | High (on-demand only) |
| User Adoption | Must switch keyboard | Keep existing keyboard |
| Complexity | Very High | Medium |
| Maintenance | Complex | Simple |

### Key Features
- ✅ Floating bubble accessible from any app
- ✅ Text selection menu integration
- ✅ Automatic clipboard monitoring (optional)
- ✅ Multiple correction styles (Formal, Casual, Professional)
- ✅ Quick apply with one tap
- ✅ History of corrections
- ✅ Offline mode with cached corrections
- ✅ Works with WhatsApp, Gmail, Notes, and ALL apps

---

## 💻 Development Environment

### Platform
- **Development Machine**: macOS
- **IDE**: Android Studio (Latest stable version)
- **Version Control**: Git + GitHub

### Android SDK Configuration

> ⚠️ **Note**: Select Target SDK and Min SDK based on your Android Studio setup and the devices you want to support.

| Setting | Recommended | Notes |
|---------|-------------|-------|
| **Target SDK** | Latest available in your Android Studio | Check `Preferences > Appearance & Behavior > System Settings > Android SDK` |
| **Min SDK** | API 26 (Android 8.0) or higher | Lower = more devices, but fewer features |
| **Compile SDK** | Same as Target SDK | Must match or exceed Target SDK |

**How to Check Your SDK:**
```
Android Studio > Preferences > Appearance & Behavior > System Settings > Android SDK
```

### GitHub Repository Setup

**Repository Name**: `writeright-android`

**Recommended Structure:**
```
writeright-android/
├── .github/
│   ├── workflows/          # CI/CD (optional)
│   └── ISSUE_TEMPLATE/
├── app/
│   └── src/
├── docs/
│   ├── IMPLEMENTATION_PLAN.md
│   └── TASK_TRACKER.md
├── .gitignore
├── README.md
├── LICENSE
└── build.gradle.kts
```

**Branch Strategy:**
```
main          ← Production-ready code
├── develop   ← Development branch
│   ├── feature/floating-bubble
│   ├── feature/gemini-integration
│   └── feature/settings-screen
```

**Initial GitHub Setup Commands (Mac Terminal):**
```bash
# Create new project directory
mkdir writeright-android
cd writeright-android

# Initialize git
git init

# Create initial structure
touch README.md
mkdir -p docs

# Add remote (after creating repo on GitHub)
git remote add origin https://github.com/YOUR_USERNAME/writeright-android.git

# Initial commit
git add .
git commit -m "Initial project setup"
git push -u origin main
```

**.gitignore for Android:**
```
# Android Studio
*.iml
.gradle/
local.properties
.idea/
*.hprof

# Build outputs
/build/
/app/build/
/app/release/

# Signing
*.jks
*.keystore
keystore.properties

# Secrets
secrets.properties
google-services.json

# Mac
.DS_Store

# API Keys (NEVER commit!)
**/secrets/
```

---

## 🏗️ Solution Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    WriteRight Architecture                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                    Presentation Layer                    │   │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐    │   │
│  │  │   Floating   │ │  Correction  │ │   Settings   │    │   │
│  │  │    Bubble    │ │    Popup     │ │   Activity   │    │   │
│  │  └──────────────┘ └──────────────┘ └──────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                   │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                     Service Layer                        │   │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐    │   │
│  │  │   Overlay    │ │  Clipboard   │ │   Gemini     │    │   │
│  │  │   Service    │ │   Manager    │ │   Manager    │    │   │
│  │  └──────────────┘ └──────────────┘ └──────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                   │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │                      Data Layer                          │   │
│  │  ┌──────────────┐ ┌──────────────┐ ┌──────────────┐    │   │
│  │  │  DataStore   │ │    Room      │ │   Network    │    │   │
│  │  │ (Preferences)│ │  (History)   │ │  (Retrofit)  │    │   │
│  │  └──────────────┘ └──────────────┘ └──────────────┘    │   │
│  └─────────────────────────────────────────────────────────┘   │
│                              │                                   │
│                    ┌─────────▼─────────┐                        │
│                    │   Gemini AI API   │                        │
│                    │  (Google Cloud)   │                        │
│                    └───────────────────┘                        │
└─────────────────────────────────────────────────────────────────┘
```

### Component Overview

| Component | Responsibility |
|-----------|---------------|
| **FloatingBubbleService** | Foreground service managing the floating bubble UI |
| **CorrectionPopupView** | Overlay window showing original vs corrected text |
| **ClipboardManager** | Monitors and manages clipboard operations |
| **GeminiManager** | Handles API communication with Gemini |
| **SettingsActivity** | User preferences and configuration |
| **HistoryRepository** | Stores correction history locally |

---

## 🛠️ Technology Stack

| Category | Technology | Purpose |
|----------|------------|---------|
| **Language** | Kotlin | Modern, safe Android development |
| **Min SDK** | User's choice (recommend API 26+) | Device compatibility |
| **Target SDK** | User's choice (latest stable) | Latest features |
| **Architecture** | MVVM + Clean Architecture | Maintainable codebase |
| **DI** | Hilt | Dependency injection |
| **Networking** | Retrofit + OkHttp | Gemini API calls |
| **Async** | Kotlin Coroutines + Flow | Background operations |
| **Local Storage** | DataStore + Room | Preferences & history |
| **AI** | Google Gemini API | Text correction |
| **UI** | Jetpack Compose + XML | Modern UI + Overlays |
| **Testing** | JUnit5 + Mockk | Unit testing |

### Dependencies

```kotlin
// build.gradle.kts (app)
android {
    namespace = "com.writeright"
    
    compileSdk = YOUR_COMPILE_SDK  // e.g., 34
    
    defaultConfig {
        applicationId = "com.writeright"
        minSdk = YOUR_MIN_SDK      // e.g., 26
        targetSdk = YOUR_TARGET_SDK // e.g., 34
        versionCode = 1
        versionName = "1.0.0"
    }
}

dependencies {
    // Core
    implementation("androidx.core:core-ktx:1.12.0")
    implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.7.0")
    
    // Compose (for Settings)
    implementation(platform("androidx.compose:compose-bom:2024.01.00"))
    implementation("androidx.compose.ui:ui")
    implementation("androidx.compose.material3:material3")
    
    // Hilt
    implementation("com.google.dagger:hilt-android:2.50")
    kapt("com.google.dagger:hilt-compiler:2.50")
    
    // Networking
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
    implementation("com.squareup.okhttp3:logging-interceptor:4.12.0")
    
    // Local Storage
    implementation("androidx.datastore:datastore-preferences:1.0.0")
    implementation("androidx.room:room-runtime:2.6.1")
    implementation("androidx.room:room-ktx:2.6.1")
    kapt("androidx.room:room-compiler:2.6.1")
    
    // Testing
    testImplementation("junit:junit:4.13.2")
    testImplementation("io.mockk:mockk:1.13.9")
}
```

---

## 👆 User Flows

### Flow 1: Floating Bubble (Primary)

```
┌─────────────────────────────────────────────────────────────┐
│                  Floating Bubble Flow                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ① User types text in any app (WhatsApp, Gmail, etc.)      │
│                         │                                    │
│                         ▼                                    │
│   ② User COPIES the text to clipboard                       │
│                         │                                    │
│                         ▼                                    │
│   ③ User taps the floating bubble 🫧                        │
│                         │                                    │
│                         ▼                                    │
│   ④ Correction popup appears with:                          │
│      ┌─────────────────────────────────┐                    │
│      │  Original: "i want meet u tmrw" │                    │
│      │  ─────────────────────────────  │                    │
│      │  Corrected: "I would like to    │                    │
│      │  meet you tomorrow."            │                    │
│      │  ─────────────────────────────  │                    │
│      │  [Style ▼]  [✗ Cancel] [✓ Apply]│                    │
│      └─────────────────────────────────┘                    │
│                         │                                    │
│                         ▼                                    │
│   ⑤ User taps "Apply" → Corrected text copied               │
│                         │                                    │
│                         ▼                                    │
│   ⑥ User pastes the corrected text                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Flow 2: Auto-Clipboard Detection (Optional)

```
┌─────────────────────────────────────────────────────────────┐
│              Auto-Detection Flow (Optional)                  │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ① User copies text anywhere                               │
│                         │                                    │
│                         ▼                                    │
│   ② App detects clipboard change                            │
│                         │                                    │
│                         ▼                                    │
│   ③ Small notification/toast appears:                       │
│      "Tap to improve: 'i want meet...'"                     │
│                         │                                    │
│                         ▼                                    │
│   ④ User taps → Correction popup opens                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Flow 3: In-App Quick Launch

```
┌─────────────────────────────────────────────────────────────┐
│                    Quick Launch Flow                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│   ① User opens WriteRight app                               │
│                         │                                    │
│                         ▼                                    │
│   ② Text field with "Paste or type text here"              │
│                         │                                    │
│                         ▼                                    │
│   ③ User pastes/types text                                  │
│                         │                                    │
│                         ▼                                    │
│   ④ Taps "Fix It" button                                    │
│                         │                                    │
│                         ▼                                    │
│   ⑤ Shows corrected text + Copy button                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 📅 Development Phases

| Phase | Name | Duration | Weeks |
|-------|------|----------|-------|
| 1 | Foundation & Setup | 1 week | Week 1 |
| 2 | Floating Overlay Service | 1.5 weeks | Week 2-3 |
| 3 | Gemini AI Integration | 1 week | Week 3-4 |
| 4 | Correction UI & UX | 1.5 weeks | Week 4-5 |
| 5 | Settings & Customization | 1 week | Week 5-6 |
| 6 | Testing & Polish | 1 week | Week 7 |
| 7 | Release Preparation | 1 week | Week 8 |

**Total: ~8 weeks**

---

## ✅ Task Breakdown

### 📦 Phase 1: Foundation & Setup (Week 1)

#### Task 1.1: Project Setup
- [ ] Create GitHub repository `writeright-android`
- [ ] Clone repository on Mac
- [ ] Create new Android project in Android Studio
- [ ] Configure Kotlin and Gradle (Kotlin DSL)
- [ ] Set up project structure (Clean Architecture)
- [ ] Add all required dependencies
- [ ] Configure ProGuard/R8 rules
- [ ] Push initial commit to GitHub

**Project Structure:**
```
app/src/main/java/com/writeright/
├── di/                          # Hilt modules
│   ├── AppModule.kt
│   └── NetworkModule.kt
├── data/
│   ├── remote/                  # API
│   │   ├── GeminiApi.kt
│   │   └── models/
│   ├── local/                   # Room + DataStore
│   │   ├── HistoryDao.kt
│   │   └── PreferencesManager.kt
│   └── repository/
│       └── CorrectionRepository.kt
├── domain/
│   ├── model/
│   │   └── Correction.kt
│   └── usecase/
│       └── CorrectTextUseCase.kt
├── presentation/
│   ├── main/                    # Main Activity
│   ├── settings/                # Settings screens
│   └── overlay/                 # Overlay views
├── service/
│   ├── FloatingBubbleService.kt
│   └── ClipboardService.kt
└── WriteRightApp.kt             # Application class
```

#### Task 1.2: Manifest Configuration
- [ ] Configure required permissions
- [ ] Declare foreground service
- [ ] Set up app theme and icons
- [ ] Configure backup rules

**AndroidManifest.xml:**
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android">

    <!-- Permissions -->
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW"/>
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
    <uses-permission android:name="android.permission.FOREGROUND_SERVICE_SPECIAL_USE"/>
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>

    <application
        android:name=".WriteRightApp"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/Theme.WriteRight">
        
        <!-- Main Activity -->
        <activity
            android:name=".presentation.main.MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>
                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
        
        <!-- Floating Bubble Service -->
        <service
            android:name=".service.FloatingBubbleService"
            android:exported="false"
            android:foregroundServiceType="specialUse">
            <property
                android:name="android.app.PROPERTY_SPECIAL_USE_FGS_SUBTYPE"
                android:value="Floating text correction assistant"/>
        </service>
        
    </application>
</manifest>
```

#### Task 1.3: Base Classes & Utilities
- [ ] Create WriteRightApp (Application class)
- [ ] Set up Hilt Application
- [ ] Create base ViewModel class
- [ ] Create NetworkManager utility
- [ ] Set up Timber for logging
- [ ] Create string resources file

---

### 🫧 Phase 2: Floating Overlay Service (Week 2-3)

#### Task 2.1: Floating Bubble Service
- [ ] Create FloatingBubbleService class
- [ ] Implement foreground service with notification
- [ ] Request SYSTEM_ALERT_WINDOW permission
- [ ] Handle service lifecycle (start/stop)
- [ ] Add service binder for activity communication
- [ ] Test service persistence across app switches

**FloatingBubbleService.kt (skeleton):**
```kotlin
@AndroidEntryPoint
class FloatingBubbleService : Service() {
    
    private lateinit var windowManager: WindowManager
    private lateinit var bubbleView: View
    private lateinit var correctionView: View
    
    override fun onCreate() {
        super.onCreate()
        windowManager = getSystemService(WINDOW_SERVICE) as WindowManager
        createBubbleView()
        startForeground(NOTIFICATION_ID, createNotification())
    }
    
    private fun createBubbleView() {
        // Inflate and configure bubble
    }
    
    private fun showCorrectionPopup(text: String) {
        // Show correction overlay
    }
    
    override fun onBind(intent: Intent?): IBinder? = null
}
```

#### Task 2.2: Floating Bubble UI
- [ ] Design bubble layout (circular icon)
- [ ] Implement drag-to-move functionality
- [ ] Add bubble expand/collapse animation
- [ ] Handle bubble positioning (edge snapping)
- [ ] Add bubble visibility toggle
- [ ] Implement double-tap to hide

**Bubble Features:**
- Draggable across screen
- Snaps to edges when released
- Semi-transparent when idle
- Highlights when tapped
- Long-press to access quick menu

#### Task 2.3: Correction Popup Window
- [ ] Design correction popup layout
- [ ] Show original text (scrollable)
- [ ] Show corrected text with highlighting
- [ ] Add style selector dropdown
- [ ] Implement Apply button
- [ ] Implement Cancel/Dismiss button
- [ ] Add loading state animation
- [ ] Handle error states

**Popup Layout Elements:**
```
┌─────────────────────────────────────┐
│  ✍️ WriteRight                  [X] │
├─────────────────────────────────────┤
│  Original:                          │
│  ┌─────────────────────────────────┐│
│  │ i want meet u tmrw if ur free  ││
│  └─────────────────────────────────┘│
│                                     │
│  Corrected:                         │
│  ┌─────────────────────────────────┐│
│  │ I would like to meet you       ││
│  │ tomorrow if you are free.      ││
│  └─────────────────────────────────┘│
│                                     │
│  Style: [Polite ▼]                  │
│                                     │
│  [Cancel]              [✓ Apply]    │
└─────────────────────────────────────┘
```

#### Task 2.4: Clipboard Integration
- [ ] Create ClipboardManager wrapper
- [ ] Read text from clipboard on bubble tap
- [ ] Write corrected text to clipboard
- [ ] Show toast confirmation on copy
- [ ] Handle empty clipboard gracefully
- [ ] Optional: Implement clipboard listener

---

### 🤖 Phase 3: Gemini AI Integration (Week 3-4)

#### Task 3.1: Gemini API Setup
- [ ] Create Google Cloud project
- [ ] Enable Generative Language API
- [ ] Generate API key
- [ ] Set up secure API key storage
- [ ] Create GeminiConfig object

**GeminiConfig.kt:**
```kotlin
object GeminiConfig {
    const val BASE_URL = "https://generativelanguage.googleapis.com/v1beta/"
    const val MODEL = "gemini-1.5-flash"  // Fast model for real-time
    
    // Prompt templates
    const val PROMPT_POLITE = """
        You are a helpful writing assistant. Improve the following text by:
        1. Fixing all grammar and spelling errors
        2. Making it polite and professional
        3. Keeping the original meaning intact
        4. Keeping it natural and conversational
        
        Style: %s
        Original text: "%s"
        
        Respond with ONLY the corrected text, nothing else.
    """.trimIndent()
}
```

#### Task 3.2: Network Layer
- [ ] Set up Retrofit client with OkHttp
- [ ] Create GeminiApi interface
- [ ] Implement request models (GeminiRequest)
- [ ] Implement response models (GeminiResponse)
- [ ] Add network interceptors (logging, auth)
- [ ] Implement error handling

**GeminiApi.kt:**
```kotlin
interface GeminiApi {
    @POST("models/{model}:generateContent")
    suspend fun generateContent(
        @Path("model") model: String = GeminiConfig.MODEL,
        @Query("key") apiKey: String,
        @Body request: GeminiRequest
    ): Response<GeminiResponse>
}

data class GeminiRequest(
    val contents: List<Content>,
    val generationConfig: GenerationConfig = GenerationConfig()
)

data class Content(
    val parts: List<Part>
)

data class Part(
    val text: String
)

data class GenerationConfig(
    val temperature: Float = 0.3f,
    val maxOutputTokens: Int = 256,
    val topP: Float = 0.8f
)
```

#### Task 3.3: Correction Repository
- [ ] Create CorrectionRepository
- [ ] Implement text correction method
- [ ] Handle API responses
- [ ] Implement caching strategy
- [ ] Add retry logic with exponential backoff

#### Task 3.4: CorrectText UseCase
- [ ] Create CorrectTextUseCase
- [ ] Implement business logic
- [ ] Handle different correction styles
- [ ] Validate input text
- [ ] Format output text

**CorrectionStyle.kt:**
```kotlin
enum class CorrectionStyle(val displayName: String, val promptHint: String) {
    POLITE("Polite", "polite and friendly"),
    FORMAL("Formal", "formal and professional"),
    CASUAL("Casual", "casual but correct"),
    PROFESSIONAL("Professional", "professional business tone")
}
```

---

### 🎨 Phase 4: Correction UI & UX (Week 4-5)

#### Task 4.1: Loading States
- [ ] Design loading animation for popup
- [ ] Show "Correcting..." text with animation
- [ ] Implement skeleton loader for text areas
- [ ] Add timeout handling with retry option

#### Task 4.2: Error Handling UI
- [ ] Design error state layout
- [ ] Show network error message
- [ ] Show API error message
- [ ] Add retry button
- [ ] Handle "no internet" gracefully

#### Task 4.3: Success Animations
- [ ] Add checkmark animation on apply
- [ ] Toast/Snackbar confirmation
- [ ] Smooth popup dismiss animation
- [ ] Bubble pulse animation on success

#### Task 4.4: Text Comparison View
- [ ] Highlight differences between original and corrected
- [ ] Color-code additions (green) and removals (red)
- [ ] Make text areas scrollable for long text
- [ ] Add copy button for each text area

#### Task 4.5: Quick Actions
- [ ] Add undo functionality (restore original)
- [ ] Add re-process with different style
- [ ] Add "Edit before applying" option
- [ ] History quick access

---

### ⚙️ Phase 5: Settings & Customization (Week 5-6)

#### Task 5.1: Main Activity
- [ ] Create home screen with quick correction input
- [ ] Add "Enable Floating Bubble" toggle
- [ ] Show bubble status indicator
- [ ] Add "How to Use" guide
- [ ] Add navigation to settings

**Main Screen Layout:**
```
┌─────────────────────────────────────┐
│        ✍️ WriteRight                │
├─────────────────────────────────────┤
│                                     │
│   ┌─────────────────────────────┐   │
│   │ Type or paste text here... │   │
│   │                             │   │
│   │                             │   │
│   └─────────────────────────────┘   │
│                                     │
│        [  🚀 Fix It  ]              │
│                                     │
├─────────────────────────────────────┤
│  Floating Bubble    [========●]  ON │
├─────────────────────────────────────┤
│  ⚙️ Settings                        │
│  📜 History                         │
│  ❓ How to Use                      │
└─────────────────────────────────────┘
```

#### Task 5.2: Settings Screen
- [ ] Create Settings Activity with Compose
- [ ] Default correction style preference
- [ ] Bubble appearance settings (size, opacity)
- [ ] Auto-clipboard detection toggle
- [ ] Notification settings
- [ ] API key input (advanced users)
- [ ] About & Privacy Policy link

**Settings Options:**
```
CORRECTION
  • Default Style: [Polite ▼]
  • Auto-correct on copy: [Off]

APPEARANCE
  • Bubble Size: [Medium ▼]
  • Bubble Opacity: [====●===] 70%
  • Theme: [System ▼]

ADVANCED
  • Custom API Key: [Enter key...]
  • Clear History
  • Clear Cache

ABOUT
  • Version: 1.0.0
  • Privacy Policy
  • Terms of Service
```

#### Task 5.3: Correction History
- [ ] Create Room database for history
- [ ] Create HistoryDao with CRUD operations
- [ ] Design history list screen
- [ ] Show original → corrected pairs
- [ ] Add search/filter functionality
- [ ] Add delete individual/all option

**History Entity:**
```kotlin
@Entity(tableName = "correction_history")
data class CorrectionHistory(
    @PrimaryKey(autoGenerate = true) val id: Long = 0,
    val originalText: String,
    val correctedText: String,
    val style: String,
    val timestamp: Long = System.currentTimeMillis()
)
```

#### Task 5.4: Onboarding Flow
- [ ] Create first-launch onboarding
- [ ] Request overlay permission with explanation
- [ ] Show how-to-use tutorial
- [ ] Quick style preference selection

---

### 🧪 Phase 6: Testing & Polish (Week 7)

#### Task 6.1: Unit Tests
- [ ] Test CorrectTextUseCase
- [ ] Test CorrectionRepository
- [ ] Test ClipboardManager
- [ ] Test PreferencesManager
- [ ] Test API response parsing

#### Task 6.2: Integration Tests
- [ ] Test Gemini API end-to-end
- [ ] Test clipboard read/write
- [ ] Test history persistence
- [ ] Test settings persistence

#### Task 6.3: UI Tests
- [ ] Test floating bubble interactions
- [ ] Test correction popup flow
- [ ] Test settings changes
- [ ] Test main activity flow

#### Task 6.4: Manual Testing Checklist
- [ ] Test with WhatsApp
- [ ] Test with Gmail
- [ ] Test with Google Keep
- [ ] Test with Chrome
- [ ] Test with SMS app
- [ ] Test edge cases (empty text, very long text)
- [ ] Test without internet connection

#### Task 6.5: Performance Optimization
- [ ] Profile memory usage
- [ ] Optimize overlay rendering
- [ ] Reduce battery consumption
- [ ] Implement efficient caching

---

### 🚀 Phase 7: Release Preparation (Week 8)

#### Task 7.1: App Polish
- [ ] Create app icon (launcher icon)
- [ ] Create notification icon
- [ ] Add splash screen
- [ ] Final UI polish and consistency check

#### Task 7.2: Play Store Assets
- [ ] Write app description
- [ ] Create feature graphic (1024x500)
- [ ] Create screenshots (phone)
- [ ] Create screenshots (tablet - optional)
- [ ] Record demo video (optional)

#### Task 7.3: Legal & Compliance
- [ ] Write Privacy Policy
- [ ] Write Terms of Service
- [ ] Data Safety form for Play Store
- [ ] Ensure GDPR compliance if targeting EU

#### Task 7.4: Release Build
- [ ] Generate signed APK/AAB
- [ ] Test release build
- [ ] Create Play Console listing
- [ ] Submit for review
- [ ] Plan for beta testing

---

## 🔌 API Integration Details

### Gemini API Request

```json
{
  "contents": [
    {
      "parts": [
        {
          "text": "You are a helpful writing assistant. Improve the following text by:\n1. Fixing all grammar and spelling errors\n2. Making it polite and professional\n3. Keeping the original meaning intact\n\nStyle: polite and friendly\nOriginal text: \"i want meet u tmrw if ur free\"\n\nRespond with ONLY the corrected text, nothing else."
        }
      ]
    }
  ],
  "generationConfig": {
    "temperature": 0.3,
    "maxOutputTokens": 256,
    "topP": 0.8
  }
}
```

### Gemini API Response

```json
{
  "candidates": [
    {
      "content": {
        "parts": [
          {
            "text": "I would like to meet you tomorrow if you are free."
          }
        ]
      },
      "finishReason": "STOP"
    }
  ]
}
```

### API Cost Estimation

| Usage Level | Requests/Day | Requests/Month | Estimated Cost |
|-------------|--------------|----------------|----------------|
| Light | 10 | 300 | **Free** |
| Regular | 50 | 1,500 | **Free** |
| Heavy | 200 | 6,000 | ~$0.50-1 |
| Power User | 500 | 15,000 | ~$2-3 |

*Gemini 1.5 Flash has generous free tier: ~1M tokens/month free*

---

## 🔒 Security & Privacy

### Permissions Justification

| Permission | Reason | User Benefit |
|------------|--------|--------------|
| `INTERNET` | Connect to Gemini API | AI-powered corrections |
| `SYSTEM_ALERT_WINDOW` | Show floating bubble | Access from any app |
| `FOREGROUND_SERVICE` | Keep bubble running | Persistent availability |
| `POST_NOTIFICATIONS` | Show service notification | User awareness |

### API Key Security

```kotlin
// ❌ NEVER DO THIS
const val API_KEY = "AIzaSy..."

// ✅ RECOMMENDED APPROACHES

// Option 1: BuildConfig (for initial development)
// In local.properties: GEMINI_API_KEY=your_key
// In build.gradle: buildConfigField("String", "GEMINI_API_KEY", ...)

// Option 2: Encrypted SharedPreferences (user provides key)
val encryptedPrefs = EncryptedSharedPreferences.create(...)

// Option 3: Android Keystore (production)
val keyStore = KeyStore.getInstance("AndroidKeyStore")
```

### Data Privacy Principles

1. **No Persistent Text Storage**: Text is only stored in history if user enables it
2. **Local Processing First**: Consider on-device options for simple corrections
3. **Transparent Data Usage**: Clear privacy policy about API data
4. **User Control**: Easy delete all data option
5. **Secure Transmission**: All API calls over HTTPS

### Privacy Policy Must Include

- Text is sent to Google's Gemini API for processing
- Google's data retention policies apply
- User can opt-out of history storage
- No personal data is sold or shared beyond API usage

---

## 🧪 Testing Strategy

### Testing Coverage

```
              ┌───────────────┐
              │  UI Tests     │  15%
              │  (Espresso)   │
              ├───────────────┤
              │ Integration   │  25%
              │   Tests       │
              ├───────────────┤
              │  Unit Tests   │  60%
              │ (JUnit+Mockk) │
              └───────────────┘
```

### Critical Test Scenarios

| Priority | Scenario | Type |
|----------|----------|------|
| P0 | Bubble appears and is draggable | UI |
| P0 | Clipboard text is read correctly | Integration |
| P0 | API returns corrected text | Integration |
| P0 | Corrected text copies to clipboard | Integration |
| P1 | Different styles produce different outputs | Unit |
| P1 | Network errors handled gracefully | Unit |
| P1 | Settings persist across restarts | Integration |
| P2 | History saves corrections | Integration |
| P2 | Long text scrolls properly | UI |

### Device Testing Matrix

| Device Type | Android Version | Priority |
|-------------|-----------------|----------|
| Pixel 6/7/8 | Android 14 | P0 |
| Samsung Galaxy S23 | Android 14 | P0 |
| OnePlus | Android 13 | P1 |
| Xiaomi | Android 12 | P1 |
| Budget Device | Android 10 | P2 |
| Tablet | Android 13+ | P2 |

---

## 📦 Deployment & Distribution

### Build Variants

```kotlin
android {
    buildTypes {
        debug {
            applicationIdSuffix = ".debug"
            isDebuggable = true
        }
        release {
            isMinifyEnabled = true
            isShrinkResources = true
            proguardFiles(...)
        }
    }
}
```

### Release Checklist

- [ ] Version code incremented
- [ ] Version name updated (semantic versioning)
- [ ] Release notes written
- [ ] APK signed with release key
- [ ] ProGuard mapping uploaded to Play Console
- [ ] All tests passing
- [ ] Manual testing on 3+ devices
- [ ] Privacy policy URL active
- [ ] Data safety form completed

### Distribution Options

| Channel | Pros | Cons |
|---------|------|------|
| **Google Play Store** | Wide reach, trust | Review process |
| **Direct APK** | Immediate release | Manual updates |
| **Firebase App Distribution** | Beta testing | Limited to testers |

---

## 📊 Success Metrics

| Metric | Target | How to Measure |
|--------|--------|----------------|
| Install Rate | 500+ first month | Play Console |
| Daily Active Users | 30%+ retention | Firebase Analytics |
| Correction Acceptance | 75%+ | In-app tracking |
| API Success Rate | 99%+ | Crash/error logs |
| App Rating | 4.2+ stars | Play Store |
| Crash-free Rate | 99.5%+ | Crashlytics |

---

## 🗓️ Timeline Summary

| Week | Phase | Deliverables |
|------|-------|--------------|
| 1 | Foundation | Project setup, GitHub repo, permissions, base classes |
| 2 | Overlay Pt.1 | Floating bubble service, drag mechanics |
| 3 | Overlay Pt.2 | Correction popup, clipboard integration |
| 4 | AI Integration | Gemini API, text processing |
| 5 | UI/UX | Animations, error handling, polish |
| 6 | Settings | Settings screen, history, onboarding |
| 7 | Testing | Full testing, bug fixes, optimization |
| 8 | Release | Play Store preparation, launch |

**Total Estimated Time: 8 weeks**

---

## 📚 Resources

### Official Documentation
- [Floating Windows (Overlay)](https://developer.android.com/develop/ui/views/components/floating-windows)
- [Foreground Services](https://developer.android.com/develop/background-work/services/foreground-services)
- [Gemini API Quickstart](https://ai.google.dev/gemini-api/docs/quickstart)
- [Clipboard Framework](https://developer.android.com/develop/ui/views/touch-and-input/copy-paste)

### Sample Projects
- [Floating Widget Sample](https://github.com/nickmafra/android-floating-widget-sample)
- [Gemini Android Sample](https://github.com/google/generative-ai-android)

### Design Resources
- Material Design 3 Guidelines
- Android UI Patterns for Overlays

---

## 📝 Notes

### Key Decisions Made

1. **Floating Overlay over Custom Keyboard**: Faster development, higher user trust
2. **Gemini 1.5 Flash**: Fast responses suitable for real-time corrections
3. **Clipboard-based**: Works with all apps without special integration
4. **Optional clipboard monitoring**: Respects user privacy preferences

### Future Enhancements (v2.0)

- [ ] Offline mode with basic grammar rules
- [ ] Multi-language support
- [ ] Custom prompt templates
- [ ] Text expansion/snippets
- [ ] Integration with accessibility service (optional)
- [ ] Widget for home screen

---

*Document Version: 2.1*  
*Last Updated: January 25, 2026*  
*Project: WriteRight - Android AI Typing Assistant*  
*Repository: github.com/YOUR_USERNAME/writeright-android*
