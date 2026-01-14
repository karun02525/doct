# Business Objective

Provide seamless access to the Source app via QR code scanning, reducing friction for new and returning users.

#### Opportunity

- Enable near\-instant access to the app within `1–2` seconds after scanning the QR code.
- Ensure compatibility with supported `iOS` and `Android` phones and tablets.
- Present a visually appealing landing page with `Freightliner` - `Smart Source` branding or
- Present a visually appealing landing page with `Western Star` - `Star Source` branding.

### Goals


- Reduce app access friction for users
- Increase app download and installation rates
- Enhance user trust and experience

## Business Value

- Faster and simpler access improves user engagement
- Higher app adoption rates across fleet and driver personnel
- Strengthens brand perception with a professional, seamless experience

### Primary Use Cases

### 1) App Installed

**Trigger:** User scans the QR code.  
**Outcome:** User is deep-linked into the Smart/Star Source app.

**Flow:**
1. Scan QR code
2. Open branded landing page
3. Detect OS and attempt deep link
4. Smart/Star Source app opens

### 2) App Not Installed(`New User`)

**Trigger:** User scans the QR code without the app installed.  
**Outcome:** User is redirected to the correct app store listing.

**Flow:**
1. Scan QR code
2. Open branded landing page
3. Detect OS
4. Redirect to `Apple App Store` or `Google Play Store`


## Business Rules

- The QR code resolves to a secure `HTTPS` landing page
- The landing page routes users based on:
    - Operating system (`iOS` - `Android` )
    - App install availability (`installed` - `not installed`)
- Branding must be visible on the landing page (`Freightliner` - `Smart Source`)
- Branding must be visible on the landing page (`Western Star` - `Star Source`)

![deeplink.png](deeplink.png)

## Use Case Diagram
![qr_landing.png](qr_landing.png)
