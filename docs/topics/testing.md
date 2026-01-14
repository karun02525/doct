# Testing

## 1) Test Scope

### In Scope

* QR code scan opens a secure `HTTPS` landing page
* Device detection: `iOS` vs `Android` vs other platforms
* App installed: deep-link attempt opens the Smart/Star Source app
* App not installed: redirect to the correct app store listing
* Unsupported devices: user sees a clear unsupported message
* Branding and basic content render correctly
* Failure handling (timeouts, blocked deep links, store unavailable)

### Out of Scope

* User authentication
* App-specific workflows after app launch
* Tracking analytics beyond app launch/download (as specified)

## 2) Test Environments

* `Production-like` hosting over `HTTPS`
* Real devices (preferred) and simulators/emulators (secondary)
* Browsers:

  * iOS: `Safari` (primary), in-app browsers if applicable (secondary)
  * Android: `Chrome` (primary), OEM browsers (secondary)
* Network profiles:

  * `Wi‑Fi`
  * `LTE/5G`
  * `Poor network` (high latency / packet loss), if tooling is available

## 3) Test Data & Materials

* Final QR code(s) pointing to the landing page URL
* Deep-link scheme / intent URL used to open Smart/Star Source
* App Store URL (iOS)
* Play Store URL (Android)
* List of supported OS versions and device types (if defined)

## 4) Test Strategy

* Manual exploratory testing on real devices to validate deep-link behavior
* Scripted test cases for repeatable regression coverage
* Cross-browser rendering checks for the landing page UI
* Negative testing for unsupported devices and failure scenarios

## 5) End-to-End Test Cases

### A) QR Scan → Landing Page

| ID    | Scenario              | Steps                        | Expected Result                                                                    |
| ----- | --------------------- | ---------------------------- | ---------------------------------------------------------------------------------- |
| QR-01 | QR opens landing page | Scan QR with device camera   | Landing page loads successfully over `HTTPS`                                       |
| QR-02 | Invalid QR code       | Scan malformed/incorrect QR  | Clear error message or safe fallback page is shown                                 |
| QR-03 | In-app browser        | Open QR from a messaging app | Landing page renders correctly and the flow works, or clear instructions are shown |

### B) iOS – App Installed – Deep Link

| ID        | Scenario                 | Steps                        | Expected Result                                                           |
| --------- | ------------------------ | ---------------------------- | ------------------------------------------------------------------------- |
| IOS-DL-01 | Deep link success        | Install app → scan QR        | App opens directly (or after a brief prompt)                              |
| IOS-DL-02 | Deep link blocked/denied | Scan → deny any prompt       | User remains on landing page with clear `Open App` retry and store option |
| IOS-DL-03 | Return to browser        | Open app → return to browser | State remains consistent with no broken UI                                |

### C) iOS – App Not Installed – Store Redirect

| ID        | Scenario          | Steps                          | Expected Result                            |
| --------- | ----------------- | ------------------------------ | ------------------------------------------ |
| IOS-ST-01 | Store redirect    | Uninstall app → scan QR        | Redirects to the `Apple App Store` listing |
| IOS-ST-02 | Store unreachable | Simulate blocked store/network | Clear message with retry guidance is shown |

### D) Android – App Installed – Deep Link

| ID        | Scenario                 | Steps                             | Expected Result                                             |
| --------- | ------------------------ | --------------------------------- | ----------------------------------------------------------- |
| AND-DL-01 | Intent/deep link success | Install app → scan QR             | App opens successfully                                      |
| AND-DL-02 | Open-with chooser        | Multiple handlers exist           | Correct handler selection opens Smart/Star Source           |
| AND-DL-03 | Deep link failure        | Break link handler or disable app | Landing page falls back to the store or shows a recovery UI |

### E) Android – App Not Installed – Store Redirect

| ID        | Scenario                | Steps                     | Expected Result                                                     |
| --------- | ----------------------- | ------------------------- | ------------------------------------------------------------------- |
| AND-ST-01 | Play Store redirect     | Uninstall app → scan QR   | Redirects to the `Google Play Store` listing                        |
| AND-ST-02 | No Play Store available | Device without Play Store | Alternative instructions or a clear message are shown (as designed) |

### F) Unsupported Devices / OS

| ID       | Scenario           | Steps                  | Expected Result                                         |
| -------- | ------------------ | ---------------------- | ------------------------------------------------------- |
| UNSUP-01 | Desktop browser    | Open QR URL on desktop | Unsupported message or safe informational page is shown |
| UNSUP-02 | Non‑iOS/Android OS | Open on unsupported OS | Unsupported message is shown                            |

## 6) Additional Use Case: Vehicle Motion Alert

### Description

The system monitors vehicle speed and warns the user when the vehicle is in motion above a defined threshold.

### Business Rules

* Speed threshold: **10 km/h**
* Alert is shown only when speed exceeds the threshold
* Alert automatically disappears when speed falls below the threshold

### Test Cases

| ID    | Scenario                          | Steps                            | Expected Result                                        |
| ----- | --------------------------------- | -------------------------------- | ------------------------------------------------------ |
| VM-01 | Speed exceeds threshold           | Simulate vehicle speed > 10 km/h | Alert dialog is displayed immediately                  |
| VM-02 | Speed drops below threshold       | Reduce speed to ≤ 10 km/h        | Alert dialog disappears automatically                  |
| VM-03 | Speed fluctuates around threshold | Toggle speed between 9–11 km/h   | Alert appears and disappears correctly without flicker |

## 7) UI & Content Validation

* Branding is visible: `Freightliner` + `Smart/Star Source`
* No broken images, layout issues, or overlapping elements
* CTA labels are clear (e.g., `Open App`, `Get the App`)
* Copy is consistent, concise, and non-technical

## 8) Non-Functional Testing

### Performance

* Landing page TTFB and render time meet targets (e.g., loads within `1–2` seconds on typical mobile networks)

### Security

* `HTTPS` only
* No mixed-content warnings
* No sensitive data in URL parameters
* No unintended tracking beyond app launch/download

### Reliability & Resilience

* Graceful handling of:

  * Slow networks
  * Failed deep links
  * Store page unavailability
  * Repeated scans

### Accessibility (Basic)

* Sufficient color contrast
* Tap targets usable on mobile devices
* Content remains readable with text scaling (where applicable)

## 9) Acceptance Criteria

* On supported iOS and Android devices:

  * If the app is installed, scanning the QR opens the app via deep link
  * If the app is not installed, scanning the QR opens the correct store listing
* Unsupported devices show a clear informational message
* Landing page is branded and loads over `HTTPS`
* No authentication is triggered as part of this flow
* No tracking beyond app launch/download completion

## 10) Test Results & Sign-off

Record the following for each test execution:

* Device model, OS version, and browser
* App installation state (installed / not installed)
* Outcome (deep link / store redirect / unsupported)
* Evidence (screenshots or screen recordings)
* Defects identified and their resolutions
