# WriteRight - Task Tracker

## 📊 Progress Overview

| Phase | Status | Progress | Target Date |
|-------|--------|----------|-------------|
| Phase 1: Foundation | 🟡 In Progress | 1/3 tasks | Week 1 |
| Phase 2: Floating Overlay | ⬜ Not Started | 0/4 tasks | Week 2-3 |
| Phase 3: AI Integration | ⬜ Not Started | 0/4 tasks | Week 3-4 |
| Phase 4: UI/UX | ⬜ Not Started | 0/5 tasks | Week 4-5 |
| Phase 5: Settings | ⬜ Not Started | 0/4 tasks | Week 5-6 |
| Phase 6: Testing | ⬜ Not Started | 0/5 tasks | Week 7 |
| Phase 7: Release | ⬜ Not Started | 0/4 tasks | Week 8 |

**Overall Progress: 0/29 tasks (0%)**

---

## � Development Environment

| Setting | Value |
|---------|-------|
| **Machine** | macOS |
| **IDE** | Android Studio (Latest) |
| **Repository** | GitHub: `writeright-android` |
| **Target SDK** | *Set based on your Android Studio* |
| **Min SDK** | *Set based on your Android Studio (recommend API 26+)* |

### Quick Start (Mac Terminal)
```bash
# Clone your repo
git clone https://github.com/YOUR_USERNAME/writeright-android.git
cd writeright-android

# Open in Android Studio
open -a "Android Studio" .
```

---

## �📦 Phase 1: Foundation & Setup

### Task 1.1: Project & Repository Setup
**Status:** ✅ Completed  
**Priority:** 🔴 Critical  
**Estimated Time:** 2-3 hours

**GitHub Setup:**
- [x] Create repository `writeright-android` on GitHub
- [x] Clone repository to Mac
- [x] Add `.gitignore` for Android
- [x] Add `README.md` with project description
- [ ] Add `LICENSE` file (MIT or Apache 2.0)

**Android Studio Setup:**
- [ ] Create new Android project (File > New > New Project)
- [ ] Select "Empty Activity" template
- [ ] Set package name: `com.writeright`
- [ ] Set app name: `WriteRight`
- [ ] Select language: Kotlin
- [ ] Select Min SDK based on your Android Studio settings
- [ ] Configure Gradle with dependencies (Hilt, Retrofit, Room, etc.)
- [ ] Set up Clean Architecture folder structure
- [ ] Configure ProGuard/R8 rules
- [x] Initial commit and push to GitHub

**Folder Structure to Create:**
```
app/src/main/java/com/writeright/
├── di/
├── data/
│   ├── remote/
│   ├── local/
│   └── repository/
├── domain/
│   ├── model/
│   └── usecase/
├── presentation/
│   ├── main/
│   ├── settings/
│   └── overlay/
├── service/
└── WriteRightApp.kt
```

**Notes:**
```
📍 Check SDK versions: Android Studio > Preferences > Android SDK
🔑 Do NOT commit API keys - use local.properties
```

---

### Task 1.2: Manifest Configuration
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 1 hour

- [ ] Add INTERNET permission
- [ ] Add SYSTEM_ALERT_WINDOW permission
- [ ] Add FOREGROUND_SERVICE permission
- [ ] Add FOREGROUND_SERVICE_SPECIAL_USE permission
- [ ] Add POST_NOTIFICATIONS permission
- [ ] Configure foreground service type (specialUse)
- [ ] Declare FloatingBubbleService
- [ ] Commit changes to GitHub

**Notes:**
```
- Android 14+ requires foregroundServiceType
- Need to justify SYSTEM_ALERT_WINDOW for Play Store
```

---

### Task 1.3: Base Classes & Utilities
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 2 hours

- [ ] Create WriteRightApp (Application class)
- [ ] Set up Hilt Application (`@HiltAndroidApp`)
- [ ] Create base ViewModel class
- [ ] Create NetworkManager utility
- [ ] Set up Timber for logging
- [ ] Create string resources file
- [ ] Commit changes to GitHub

---

## 🫧 Phase 2: Floating Overlay Service

### Task 2.1: Floating Bubble Service
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 4-5 hours

- [ ] Create FloatingBubbleService class
- [ ] Implement foreground service with notification
- [ ] Create notification channel (API 26+)
- [ ] Handle overlay permission request
- [ ] Implement service lifecycle (start/stop)
- [ ] Add service binder for activity communication
- [ ] Test service persistence across app switches
- [ ] Commit changes to GitHub

**Notes:**
```
- Must handle Android 12+ foreground service restrictions
- Use TYPE_APPLICATION_OVERLAY for window type
```

---

### Task 2.2: Floating Bubble UI
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 4-5 hours

- [ ] Create bubble layout XML (`layout/view_bubble.xml`)
- [ ] Create bubble icon/drawable
- [ ] Add bubble view to WindowManager
- [ ] Implement touch listener for drag
- [ ] Implement edge snapping after drag
- [ ] Add press/click animation
- [ ] Add long-press menu (optional)
- [ ] Implement double-tap to hide
- [ ] Commit changes to GitHub

**Notes:**
```
- WindowManager.LayoutParams for positioning
- Use TYPE_APPLICATION_OVERLAY window type
```

---

### Task 2.3: Correction Popup Window
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 5-6 hours

- [ ] Design correction popup layout (`layout/view_correction_popup.xml`)
- [ ] Create scrollable original text area
- [ ] Create scrollable corrected text area
- [ ] Add style dropdown selector (Spinner)
- [ ] Add Cancel button with dismiss
- [ ] Add Apply button with action
- [ ] Implement loading state with ProgressBar
- [ ] Implement error state
- [ ] Add popup entrance/exit animations
- [ ] Commit changes to GitHub

---

### Task 2.4: Clipboard Integration
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 2-3 hours

- [ ] Create ClipboardHelper class
- [ ] Implement read from clipboard
- [ ] Implement write to clipboard
- [ ] Handle empty clipboard gracefully
- [ ] Add toast confirmation ("Copied!")
- [ ] (Optional) Implement clipboard change listener
- [ ] Commit changes to GitHub

---

## 🤖 Phase 3: Gemini AI Integration

### Task 3.1: Gemini API Setup
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 2 hours

- [ ] Create Google Cloud project at [console.cloud.google.com](https://console.cloud.google.com)
- [ ] Enable Generative Language API
- [ ] Generate API key
- [ ] Add API key to `local.properties` (NEVER commit!)
- [ ] Configure BuildConfig to read API key
- [ ] Create GeminiConfig object
- [ ] Commit changes (WITHOUT API key!)

**API Key Storage (local.properties):**
```properties
# This file should NOT be committed to git
GEMINI_API_KEY=your_api_key_here
```

**build.gradle.kts:**
```kotlin
android {
    defaultConfig {
        val geminiApiKey = project.findProperty("GEMINI_API_KEY") ?: ""
        buildConfigField("String", "GEMINI_API_KEY", "\"$geminiApiKey\"")
    }
}
```

---

### Task 3.2: Network Layer
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 3-4 hours

- [ ] Create NetworkModule (Hilt)
- [ ] Set up OkHttpClient with timeouts
- [ ] Add logging interceptor (debug only)
- [ ] Set up Retrofit client
- [ ] Create GeminiApi interface
- [ ] Create GeminiRequest data class
- [ ] Create GeminiResponse data class
- [ ] Create Content, Part data classes
- [ ] Implement error handling
- [ ] Commit changes to GitHub

---

### Task 3.3: Correction Repository
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 2-3 hours

- [ ] Create CorrectionRepository interface
- [ ] Create CorrectionRepositoryImpl
- [ ] Implement `correctText(text, style)` method
- [ ] Handle API errors with Result/Either wrapper
- [ ] Add retry logic with exponential backoff
- [ ] Implement simple in-memory caching
- [ ] Commit changes to GitHub

---

### Task 3.4: Prompt Engineering
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 2-3 hours

- [ ] Create CorrectionStyle enum
- [ ] Write base prompt template
- [ ] Write Polite style prompt
- [ ] Write Formal style prompt
- [ ] Write Casual style prompt
- [ ] Write Professional style prompt
- [ ] Test prompts with various inputs (use Postman/curl)
- [ ] Optimize prompts for speed and quality
- [ ] Commit changes to GitHub

---

## 🎨 Phase 4: Correction UI & UX

### Task 4.1: Loading States
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 2 hours

- [ ] Design loading animation (ProgressBar or Lottie)
- [ ] Show "Correcting..." text
- [ ] Add shimmer/skeleton effect (optional)
- [ ] Implement timeout handler (10s max)
- [ ] Add cancel button during loading
- [ ] Commit changes to GitHub

---

### Task 4.2: Error Handling UI
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 2 hours

- [ ] Design error state layout
- [ ] Show network error message with icon
- [ ] Show API error message
- [ ] Add retry button
- [ ] Handle connectivity changes dynamically
- [ ] Commit changes to GitHub

---

### Task 4.3: Success Flow
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 2 hours

- [ ] Add success checkmark animation
- [ ] Show "Copied!" Toast/Snackbar
- [ ] Smooth dismiss animation for popup
- [ ] Bubble pulse animation on success
- [ ] Commit changes to GitHub

---

### Task 4.4: Text Comparison View
**Status:** ⬜ Not Started  
**Priority:** 🟢 Low  
**Estimated Time:** 3 hours

- [ ] Implement text diff algorithm
- [ ] Color-code additions (green)
- [ ] Color-code removals (red/strikethrough)
- [ ] Make texts selectable
- [ ] Add individual copy buttons
- [ ] Commit changes to GitHub

---

### Task 4.5: Quick Actions
**Status:** ⬜ Not Started  
**Priority:** 🟢 Low  
**Estimated Time:** 2 hours

- [ ] Add undo (restore original to clipboard)
- [ ] Add re-process with different style
- [ ] Add "Edit before apply" option
- [ ] Commit changes to GitHub

---

## ⚙️ Phase 5: Settings & Customization

### Task 5.1: Main Activity
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 4 hours

- [ ] Create MainActivity layout (XML or Compose)
- [ ] Add text input field (EditText/TextField)
- [ ] Add "Fix It" button
- [ ] Show corrected result with copy button
- [ ] Add bubble toggle switch (Switch/Toggle)
- [ ] Handle overlay permission request flow
- [ ] Add navigation to Settings
- [ ] Add navigation to History
- [ ] Commit changes to GitHub

---

### Task 5.2: Settings Screen
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 4 hours

- [ ] Create SettingsActivity (Jetpack Compose recommended)
- [ ] Default correction style preference
- [ ] Bubble size preference (Small/Medium/Large)
- [ ] Bubble opacity slider (0-100%)
- [ ] Theme preference (Light/Dark/System)
- [ ] Auto-clipboard detection toggle
- [ ] API key input (for advanced users)
- [ ] About section with version
- [ ] Privacy policy link
- [ ] Create PreferencesManager with DataStore
- [ ] Commit changes to GitHub

---

### Task 5.3: Correction History
**Status:** ⬜ Not Started  
**Priority:** 🟢 Low  
**Estimated Time:** 4 hours

- [ ] Create Room database (`WriteRightDatabase`)
- [ ] Create CorrectionHistory entity
- [ ] Create HistoryDao with CRUD operations
- [ ] Create HistoryRepository
- [ ] Design HistoryActivity/Screen
- [ ] Implement LazyColumn/RecyclerView list
- [ ] Add search/filter functionality
- [ ] Add delete individual item
- [ ] Add "Clear All" option
- [ ] Commit changes to GitHub

---

### Task 5.4: Onboarding Flow
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 3 hours

- [ ] Create OnboardingActivity with ViewPager2
- [ ] Screen 1: Welcome + App intro
- [ ] Screen 2: Permission explanation (overlay)
- [ ] Screen 3: How-to-use tutorial
- [ ] Screen 4: Style preference selection
- [ ] Mark first-launch complete in preferences
- [ ] Commit changes to GitHub

---

## 🧪 Phase 6: Testing & Polish

### Task 6.1: Unit Tests
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 4 hours

- [ ] Test CorrectTextUseCase
- [ ] Test CorrectionRepository (mock API)
- [ ] Test ClipboardHelper
- [ ] Test PreferencesManager
- [ ] Test API response parsing
- [ ] Commit tests to GitHub

---

### Task 6.2: Integration Tests
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 3 hours

- [ ] Test full Gemini API flow
- [ ] Test clipboard read/write operations
- [ ] Test history persistence with Room
- [ ] Test settings persistence with DataStore
- [ ] Commit tests to GitHub

---

### Task 6.3: UI Tests
**Status:** ⬜ Not Started  
**Priority:** 🟢 Low  
**Estimated Time:** 3 hours

- [ ] Test MainActivity flow
- [ ] Test settings changes apply correctly
- [ ] Test navigation between screens
- [ ] Commit tests to GitHub

---

### Task 6.4: Manual Testing
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 4 hours

**Test with Popular Apps:**
- [ ] WhatsApp
- [ ] Gmail
- [ ] Google Keep / Notes
- [ ] Chrome browser
- [ ] SMS/Messages
- [ ] Twitter/X
- [ ] Instagram

**Edge Cases:**
- [ ] Empty clipboard (should show message)
- [ ] Very long text (500+ chars)
- [ ] Special characters & symbols
- [ ] Emojis in text
- [ ] No internet connection
- [ ] API timeout (slow network)
- [ ] Multiple rapid taps

---

### Task 6.5: Performance Optimization
**Status:** ⬜ Not Started  
**Priority:** 🟡 Medium  
**Estimated Time:** 3 hours

- [ ] Profile memory usage (Android Profiler)
- [ ] Check for memory leaks (LeakCanary)
- [ ] Optimize overlay rendering
- [ ] Check battery impact (Battery Historian)
- [ ] Implement efficient caching
- [ ] Commit optimizations to GitHub

---

## 🚀 Phase 7: Release Preparation

### Task 7.1: App Polish
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 3 hours

- [ ] Create app icon (Adaptive Icon)
  - [ ] Foreground layer
  - [ ] Background layer
- [ ] Create notification icon (white on transparent)
- [ ] Add splash screen (SplashScreen API)
- [ ] Final UI consistency check
- [ ] Fix any visual bugs
- [ ] Commit to GitHub

---

### Task 7.2: Play Store Assets
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 3 hours

- [ ] Write short description (80 chars max)
- [ ] Write full description (4000 chars max)
- [ ] Create feature graphic (1024x500 px)
- [ ] Create phone screenshots (min 2, recommended 8)
- [ ] Optional: Create promo video (30s-2min)
- [ ] Push assets to `docs/` folder on GitHub

---

### Task 7.3: Legal & Compliance
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 2 hours

- [ ] Write Privacy Policy (use template)
- [ ] Host Privacy Policy (GitHub Pages recommended)
- [ ] Write Terms of Service
- [ ] Complete Play Store Data Safety form
- [ ] Review Google Play policies for overlays
- [ ] Commit docs to GitHub

---

### Task 7.4: Release
**Status:** ⬜ Not Started  
**Priority:** 🔴 Critical  
**Estimated Time:** 2-3 hours

- [ ] Increment version code (1) and name (1.0.0)
- [ ] Create release signing key (if not exists)
- [ ] Generate signed AAB (Android App Bundle)
- [ ] Test release build on physical device
- [ ] Create Google Play Console listing
- [ ] Upload AAB
- [ ] Fill in all store listing details
- [ ] Submit for review
- [ ] Create GitHub Release tag (v1.0.0)
- [ ] Monitor review status

---

## 📝 Git Commit Guidelines

Use conventional commits for clarity:

```
feat: Add floating bubble service
fix: Fix clipboard read on Android 12+
docs: Update README with setup instructions
style: Format code with ktlint
refactor: Extract API response parsing
test: Add unit tests for CorrectionRepository
chore: Update dependencies
```

---

## 📝 Change Log

| Date | Change |
|------|--------|
| 2026-01-25 | Initial task tracker created |
| 2026-01-25 | Updated app name to WriteRight |
| 2026-01-25 | Added Mac development setup |
| 2026-01-25 | Added GitHub repository configuration |
| 2026-01-25 | Made SDK versions configurable |

---

## 🐛 Bug Tracker

| ID | Description | Status | Priority |
|----|-------------|--------|----------|
| - | No bugs yet | - | - |

---

## 💡 Ideas & Future Improvements (v2.0)

| Idea | Priority | Notes |
|------|----------|-------|
| Offline mode | High | Basic grammar without API |
| Multi-language | High | Support Hindi, Spanish, etc. |
| Widget | Medium | Home screen quick access |
| Share menu integration | Medium | "Share to WriteRight" |
| Wear OS companion | Low | Quick corrections on watch |
| Keyboard integration | Low | Optional IME add-on |

---

## 📊 Weekly Progress Reports

### Week 1
**Status:** ⬜ Not Started  
**Tasks Planned:** Phase 1 (Foundation)  
**Tasks Completed:** -  
**Blockers:** -  
**Next Week:** Phase 2 (Floating Overlay)

---

*Last Updated: January 25, 2026*  
*Project: WriteRight - Android AI Typing Assistant*  
*Repository: github.com/YOUR_USERNAME/writeright-android*
