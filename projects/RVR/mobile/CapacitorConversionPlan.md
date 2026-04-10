# RVR Chat: Capacitor Mobile App Conversion Plan

**Created:** 2026-04-10
**Updated:** 2026-04-10 (codebase review complete)
**Stack:** Next.js 14 (existing) + Capacitor (new)
**Goal:** Native Android + iOS apps from existing Next.js codebase

---

## Codebase Analysis Summary

**Build output:** `.next` (not `out`) — Next.js default. Must specify in Capacitor config.
**Auth:** iron-session + Patreon OAuth + Google OAuth + Discord OAuth + Email (multiple providers)
**Session:** Cookie-based (`rvr-ai-chat-session`), 7-day expiry, cross-domain SSO handoff via `redvelvetrenders.com/api/auth/sync`
**No PWA:** No service worker or manifest currently exists
**Key dependencies:** ioredis (Redis), @prisma/client, @supabase/ssr, iron-session, @stripe/stripe-js

---

## Overview

Capacitor wraps your existing Next.js web app in native shells for Android and iOS. Your Next.js codebase stays largely intact — you add a Capacitor layer on top. This means:
- Same React components
- Same API calls to your backend
- Same authentication flow
- Native mobile features via plugins (push notifications, camera, biometrics, etc.)

**What changes:** Build process, native project configs, OAuth redirect URL schemes, app store assets.

**What doesn't change:** 95% of your existing code.

---

## Phase 1: Project Setup (Days 1-2)

### 1.1 Install Capacitor Core

```bash
npm install @capacitor/core @capacitor/cli
npx cap init RVRChat com.redvelvetrenders.chat --web-dir=.next
```

Note: `.next` is your Next.js build output directory (not `out`).

### 1.2 Add Platform Targets

```bash
npm install @capacitor/android @capacitor/ios
npx cap add android
npx cap add ios
```

### 1.3 Verify Shell Build

```bash
npm run build          # Next.js build → .next/ directory
npx cap copy android   # Copy web assets to android/ folder
npx cap copy ios       # Copy web assets to ios/ folder
npx cap sync android   # Sync plugins + assets
npx cap sync ios
```

**Goal:** APK/AAB generated on day 2 to confirm the shell project works before any customization.

### 1.5 OAuth Redirect URL Configuration (CRITICAL)

Your codebase validates redirect URLs to specific hostnames. Currently allows:
- `rvr.chat`
- `*.rvr.chat`
- `redvelvetrenders.com`
- `*.redvelvetrenders.com`

**For Capacitor, you need to add:**
- `rvrchat://` (iOS custom URL scheme)
- `https://rvrchat.app/` (Android intent)

**Files to modify (server-side):**
- `src/app/api/auth/patreon/callback/route.ts` — add `rvrchat://` to `isValidRedirect()`
- `src/app/api/auth/google/callback/route.ts` — same
- `src/app/api/auth/discord/callback/route.ts` — same
- `src/app/api/auth/patreon/connect/callback/route.ts` — same

**Also update environment:**
- `PATREON_REDIRECT_URI=rvrchat://auth/patreon/callback` (for mobile)
- Or handle via mobile deep linking config in native projects

### 1.6 Known Stack Conflicts

| Concern | Risk | Mitigation |
|---------|------|-----------|
| Redis / WebSocket connections | Low | HTTP polling works fine |
| iron-session cookie auth | Medium | Works via WKWebView/ChromeTab but needs deep link URL scheme |
| Cross-domain SSO handoff | High | `redvelvetrenders.com/api/auth/sync` must be reachable from native webview |
| Supabase SSR | Low | Same HTTP calls, `@capacitor-community/supabase` if needed |
| AIML API calls | Low | Same HTTP calls from native |

---

## Phase 2: Native Feature Integration (Days 3-7)

### 2.1 Deep Linking (CRITICAL - Auth depends on this)

```bash
npm install @capacitor/app @capacitor/deep-links
```

This is the most critical plugin — without it, OAuth callbacks won't reach your app.

**iOS configuration** (`ios/App/App/Info.plist`):
```xml
<key>CFBundleURLTypes</key>
<array>
  <dict>
    <key>CFBundleURLSchemes</key>
    <array>
      <string>rvrchat</string>
    </array>
  </dict>
</array>
```

**Android configuration** (`android/app/src/main/AndroidManifest.xml`):
```xml
<intent-filter>
  <action android:name="android.intent.action.VIEW" />
  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />
  <data android:scheme="rvrchat" />
</intent-filter>
<intent-filter>
  <action android:name="android.intent.action.VIEW" />
  <category android:name="android.intent.category.DEFAULT" />
  <category android:name="android.intent.category.BROWSABLE" />
  <data android:host="auth" android:scheme="https" />
</intent-filter>
```

### 2.2 Push Notifications

**Recommended:** Firebase Cloud Messaging (FCM)

```bash
npm install @capacitor/firebase
npx cap sync
```

**Steps:**
1. Create Firebase project, download `google-services.json` (Android) and `GoogleService-Info.plist` (iOS)
2. Add to respective native folders
3. Implement `FirebaseMessaging` plugin for token retrieval and foreground/background message handling
4. Wire up to your existing notification backend (or build a simple endpoint)

### 2.3 Biometric Authentication

```bash
npm install @capacitor-community/biometric-auth
```

Allows FaceID/TouchID as an alternative to password for returning users.

### 2.4 Haptic Feedback

```bash
npm install @capacitor/haptics
```

Chat apps benefit from subtle haptics on message send, receive, and reactions.

### 2.5 File/Camera Access

For future features (user avatars, image generation sharing):

```bash
npm install @capacitor/camera @capacitor/filesystem
```

---

## Phase 3: PWA + Offline Support (Days 5-7)

**No PWA currently exists** — this is new work.

1. Install `next-pwa` package for service worker generation
2. Configure `next.config.mjs` with PWA options (cache strategies, runtime caching)
3. Add web manifest for add-to-homescreen behavior
4. Test service worker registration on native webview (different from browser)

**For chat specifically:** Messages should cache locally for offline reading (not offline sending). Use IndexedDB for message history.

---

## Phase 4: App Store Assets & Configuration (Days 8-12)

### 4.1 Android (Google Play)

**Required:**
- App icon: 1024x1024 PNG (generates all sizes)
- Feature graphic: 1024x500 PNG
- Screenshots: Phone (1080x1920), Tablet (optional)
- AAB file signing (generate keystore)
- Privacy policy URL (required)
- Content rating questionnaire (4-5 min, ~$25 one-time fee)

**Assets checklist:**
- [ ] 1024x1024 app icon
- [ ] 512x512 Play Store listing icon
- [ ] 1024x500 feature graphic
- [ ] 2-8 phone screenshots (show the chat UI, character cards)
- [ ] Short description (80 chars)
- [ ] Full description (4000 chars max)
- [ ] Privacy policy page on your site

### 4.2 iOS (App Store)

**Required:**
- App icon: 1024x1024 PNG
- Screenshots: iPhone (6.7", 6.5", 5.5") + iPad (optional)
- Description, keywords, privacy policy
- Apple Developer Account ($99/year)
- Xcode to upload (Capacitor generates Xcode project)

**Screenshots needed per size:**
- 6.7" (iPhone 14 Pro Max): 1290x2796
- 6.5" (iPhone 11 Pro Max): 1242x2688
- 5.5" (iPhone 8/13 SE): 1242x2208

**Tip:** Use Capacitor's built-in screenshot tooling or a simulator to capture real UI screenshots.

---

## Phase 5: Testing (Days 8-14)

### Device Matrix

Test on real hardware, not just emulators:

| Device | OS | Priority |
|--------|-----|----------|
| Samsung Galaxy (mid-range) | Android 13 | HIGH |
| Samsung Galaxy (flagship) | Android 14 | HIGH |
| Xiaomi/Pixel (if accessible) | Android 13+ | MEDIUM |
| iPhone 13/14 (standard) | iOS 17 | HIGH |
| iPhone SE (older hardware) | iOS 17 | MEDIUM |
| iPad (if supporting tablet) | iPadOS 17 | LOW |

### Critical Test Cases

1. **OAuth flow (HIGH RISK):** Patreon/Google/Discord login from native webview → redirect to `rvrchat://` → session cookie set → user logged in
2. **Cross-domain SSO:** The `redvelvetrenders.com/api/auth/sync` handoff must work from WKWebView (iOS) and CustomTab (Android)
3. **Session persistence:** User stays logged in after app restart (cookie persistence on native webview)
4. **Push notifications:** Received while app is foregrounded/backgrounded/killed
5. **Message send/receive:** Real-time updates work (or graceful fallback)
6. **Voice playback:** TTS/voice messages play correctly
7. **Offline behavior:** Can read cached messages, shows offline indicator
8. **Deep linking:** Tapping a shared chat link opens the app to that conversation
9. **Keyboard handling:** Proper keyboard avoidance in chat input
10. **Biometrics:** FaceID/TouchID prompt appears and authenticates

### Tools

```bash
# Android
adb logcat                    # Device logs
Android Studio Emulator       # Quick testing without real device

# iOS
Xcode Simulator               # Basic testing
TestFlight                    # External beta testers
```

---

## Phase 6: Submission & Review (Days 15-21)

### Android (Google Play)
- Create developer account ($25 one-time)
- Fill store listing
- Upload AAB
- Content rating questionnaire
- Submit → Review typically 1-3 days

### iOS (App Store)
- Create Apple Developer account ($99/year)
- Create app in App Store Connect
- Upload via Xcode (organizer → validate → distribute)
- Wait for review → typically 24-48 hours for new apps
- If rejection: usually fix + resubmit same day

---

## Timeline Summary

| Phase | Duration | Focus |
|-------|----------|-------|
| Phase 1 | Days 1-2 | Shell project, verify build |
| Phase 2 | Days 3-7 | Native plugins (push, biometrics, haptics) |
| Phase 3 | Days 5-7 | PWA/service worker tuning |
| Phase 4 | Days 8-12 | App store assets, signing |
| Phase 5 | Days 8-14 | Testing (overlaps with 4) |
| Phase 6 | Days 15-21 | Submission and review |

**Total: ~3 weeks** if no major blockers.

---

## Estimated Claude Code Effort

| Task | Sessions | Notes |
|------|----------|-------|
| Initial Capacitor setup + shell verification | 1 | Straightforward |
| Push notification integration | 2-3 | Firebase config is manual |
| Biometric auth | 1-2 | Depends on existing auth structure |
| Deep linking for OAuth | 2 | Tricky — URL schemes differ by platform |
| App store asset guidance | 1 | Mostly generate, not write |
| Testing + bug fixes | 2-3 | Device-specific issues |

**~9-12 Claude Code sessions** for full conversion.

---

## Decisions Needed Before Starting

1. **Minimum Android version?** Recommend Android 8.0+ (covers 95% of devices)
2. **Support tablets?** Adds layout testing complexity
3. **Notification backend:** Do you have one, or do you need to build it?
4. **Biometric auth:** Optional but recommended — skip if auth is complex
5. **Apple Developer Account:** Need to purchase before iOS build

---

## Immediate Next Steps

1. **Apple Developer Account** ($99/year) — required before iOS build
2. **Firebase project** — create for push notifications
3. **Add OAuth redirect schemes** to server-side validation in `isValidRedirect()` across 4 callback routes
4. **Set up deep linking** in Capacitor config before testing any auth flow
5. **Test cross-domain SSO** from a mobile browser first to validate `redvelvetrenders.com/api/auth/sync` works from a mobile context
6. **Verify `.next` build output** works with `npx cap copy`

## Decisions Needed Before Starting

1. **Minimum Android version?** Recommend Android 8.0+ (covers 95% of devices)
2. **Support tablets?** Adds layout testing complexity
3. **Notification backend:** Do you have one, or do you need to build it?
4. **Biometric auth:** Optional but recommended — skip if auth is complex
5. **Separate mobile OAuth app on Patreon/Google/Discord?** Some providers require registering separate client IDs for mobile redirect schemes
