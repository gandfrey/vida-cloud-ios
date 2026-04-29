# Vida Cloud — iOS Client Brand Status

Fork of [`nextcloud/ios`](https://github.com/nextcloud/ios) (GPL-3.0-or-later with App Store linking exception) rebranded to Vida Cloud.

## What's done

Brand strings live in [`Brand/NCBrand.swift`](Brand/NCBrand.swift):

- `brand` = `VidaCloud`, `brandUserAgent` = `VidaCloud`
- `loginBaseUrl` = `https://vidatools.com/cloud`
- `webLoginAutenticationProtocol` = `vc://` (matches Info.plist URL scheme)
- `appStoreUrl` = `""` — fill in once App Store listing is approved
- `privacy` = `https://vidatools.com/privacy`
- `sourceCode` = `https://github.com/gandfrey/vida-cloud-ios`
- `capabilitiesGroup` = `group.com.vidatools.cloud`, `capabilitiesGroupApps` = `group.com.vidatools.cloud.apps`
- `customer` color (default brand color) = Vida sage `#6B8F71`
- Push proxy default no longer kicks in (was Nextcloud-specific)
- Copyright text updated to "Vida Cloud for iOS / © Vida Design 2026"

Bundle IDs in [`Nextcloud.xcodeproj/project.pbxproj`](Nextcloud.xcodeproj/project.pbxproj) — all 20 occurrences rewritten:
- Main app: `it.twsweb.$(PRODUCT_NAME)` → `com.vidatools.cloud.ios`
- `…Notification-Service-Extension` → `com.vidatools.cloud.ios.NotificationService`
- `…File-Provider-Extension` → `com.vidatools.cloud.ios.FileProvider`
- `…File-Provider-Extension-UI` → `com.vidatools.cloud.ios.FileProviderUI`
- `…Share` → `com.vidatools.cloud.ios.Share`
- `…Widget` → `com.vidatools.cloud.ios.Widget`
- `…WidgetDashboardIntentHandler` → `com.vidatools.cloud.ios.WidgetDashboardIntentHandler`
- Tests / UI tests / Integration tests all renamed under `com.vidatools.cloud.ios.*`

Entitlements + plists in [`Brand/`](Brand/) — App Group references updated:
- `group.it.twsweb.Crypto-Cloud` → `group.com.vidatools.cloud`
- `group.com.nextcloud.apps` → `group.com.vidatools.cloud.apps`

[`Brand/iOSClient.plist`](Brand/iOSClient.plist):
- `CFBundleDisplayName` = `Vida Cloud`
- BG task identifiers renamed to `com.vidatools.cloud.refreshTask` / `processingTask`
- URL scheme registered as `vc://` and `vidacloud://` under URL name `com.vidatools.cloud.ios`

## What's still needed before first release

### Apple (shared with desktop)
1. **Apple Developer Program** ($99/yr) — same membership as desktop covers iOS
2. **Apple Developer Team ID** — needed for code signing
3. **App ID** registered in [Apple Dev portal](https://developer.apple.com/account/resources/identifiers): `com.vidatools.cloud.ios`
4. **App Group** registered: `group.com.vidatools.cloud` (and `group.com.vidatools.cloud.apps` for the inter-extension shared container)
5. **Push Notification certificate** for `com.vidatools.cloud.ios`
6. **Provisioning profiles** — one per extension target (5 extensions + main app = 6 profiles). Xcode can auto-manage these once the App ID + App Group are registered.
7. **App Store Connect listing** — App Review will scrutinize a "cloud sync" app; supply screenshots from a working Vida Cloud account

### Brand assets

| File / folder | Used for | Notes |
|---|---|---|
| `AppIcon.icon/` | Apple's new .icon format (iOS 17+) | Contains a full asset hierarchy — Apple Icon Composer can build this from a single 1024px master |
| `Brand/Custom.xcassets/AppIcon.appiconset/Icon-App-1024x1024@1x.png` | App Store icon (legacy slot) | Must be 1024×1024 PNG, no alpha, no rounded corners (Apple does the rounding) |
| `Brand/LaunchScreen.storyboard` | Launch screen shown while app loads | Currently shows Nextcloud logo; replace with Vida wordmark |
| `Brand/Intro/NCIntro.storyboard` | 4-slide first-run onboarding carousel | |
| `Brand/Intro/Intro_*.png` | Onboarding artwork | 4 images, currently illustrate Nextcloud features — replace with Vida-branded illustrations OR remove the intro by setting `disable_intro = true` |

### App Store metadata
- Marketing description (long + short)
- Privacy policy URL
- Support URL
- 6.7" iPhone screenshots (3-10)
- 6.1" iPhone screenshots (3-10)
- 13" iPad screenshots (3-10)
- App preview video (optional but recommended for cloud-sync apps)

## Build flow

Open `Nextcloud.xcodeproj` in Xcode 16+. The .xcworkspace is what you actually build; the `.xcodeproj` is referenced by the workspace.

```
./xcodebuild.sh   # if it exists
# OR
xcodebuild -workspace Nextcloud.xcworkspace -scheme Nextcloud \
  -configuration Debug -destination 'platform=iOS Simulator,name=iPhone 15'
```

Until Apple Dev membership + App ID are registered, signing fails — you can still build for the simulator, where signing is bypassed.

## License

GPL-3.0-or-later **with the App Store linking exception** in [`COPYING.iOS`](COPYING.iOS). This exception is what lets us redistribute on Apple's App Store at all (Apple's standard EULA is technically incompatible with vanilla GPL).

## Watch out for

- **Push notifications**: forked clients connecting to a self-hosted Nextcloud server work fine with Nextcloud's free public push relay (default). To run a private relay, set `pushNotificationServerProxy` in `NCBrand.swift` to your own URL.
- **App Review**: Apple has historically scrutinized Nextcloud-style apps. Submit with a working test account (a Vida Cloud user with sample files) to expedite review.
- **TestFlight first**: 1-2 weeks of TestFlight testing before public submission catches most issues that would otherwise mean a rejection round-trip.
