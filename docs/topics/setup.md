# Build Process Documentation

Kotlin Multiplatform Mobile (KMM) Project targeting Android platforms:

- **Smart Source** (Freightliner Trucks)
- **Star Source** (Western Star Trucks)

## Overview

Both projects share the same codebase and are configured using sealed classes for product, environment.

## Product Configuration

The product is determined using the `Product` sealed class:
- FSSHMIEvo (Smart Source)
- WSSHMIEvo (Star Source)

The product is selected based on `BuildKonfig.PRODUCT`.

## Environment Configuration

The environment is determined using the `Environment` sealed class:
- DEV
- QA
- PROD

The environment is selected based on `BuildKonfig.ENVIRONMENT`.

##  Product Configuration & Environments

## FSS (Smart Source)

**API Key Properties File Location:**

`/Users/<username>/.gradle/apikey.properties`


| Key                                 | Value           | Description        |
|-------------------------------------|-----------------|--------------------|
| `PRODUCT`                           | FSSHMIEvo       | Product Name       |
| `PLATFORM`                          | Android         | Target Platforms   |
| `ENVIRONMENT`                       | DEV / QA / PROD | Build Environment  |

---
**Notes:**
- Store sensitive keys in the `apikey.properties` file. Contact the team for access.
- Update values as needed for each environment and platform.


## WSS (Star Source)

**API Key Properties File Location:**  
`/Users/<username>/.gradle/apikey.properties`

> **Important:**
> - This file contains sensitive API keys and configuration values. Do **not** commit it to version control.
> - If you do not have access, contact the project team to request the file securely.

### Example `apikey.properties` file
```properties
PRODUCT=WSSHMIEvo
PLATFORM=Android
ENVIRONMENT=DEV
```

### Keys & Values

| Key                                 | Value           | Description        |
|-------------------------------------|-----------------|--------------------|
| `PRODUCT`                           | WSSHMIEvo       | Product Name       |
| `PLATFORM`                          | Android         | Target Platforms   |
| `ENVIRONMENT`                       | DEV / QA / PROD | Build Environment  |

---

**Notes:**
- Store sensitive keys in the `apikey.properties` file. Contact the team for access.
- Update values as needed for each environment and platform.

# Build Process
## Android
### Product Flavors

- **fss**
  - `applicationId`: com.daimler_trucksnorthamerica.smartsource.qrapp
  - `app_name`: Smart Source

- **wss**
  - `applicationId`: com.daimler_trucksnorthamerica.wst_starsource.android.qrapp
  - `app_name`: Star Source
---

Step 1: Open Build Variants
### Build Types & Build Variants

| Flavor | Build Type | Example Variant Name |
|--------|------------|----------------------|
| fss    | debug      | fssDebug             |
| fss    | qa         | fssQa                |
| fss    | release    | fssRelease           |
| wss    | debug      | wssDebug             |
| wss    | qa         | wssQa                |
| wss    | release    | wssRelease           |


# App Release Process

## Generating a Signed APK or AAB

### Step 1: Open the Build Menu
- In Android Studio, navigate to  
  `Build` \> `Generate Signed Bundle / APK`

### Step 2: Choose the Build Type
- Select **Android App Bundle (AAB)** or
- Select **APK**

### Step 3: Select the Release Variant
- Pick the desired flavor/build type combination:
  - `fssRelease`
  - `wssRelease`

### Step 4: Configure the Keystore
-  Contact the team for access keystore file and credentials.

### Step 5: Generate the Bundle
- Click **Finish** to build the signed AAB/APK file.

### Step 6: Internal Testing

- **Verify Versioning**
  - Ensure the latest app version number and version code are updated.
- **Upload Build**
  - Upload the build to the Internal Testing track in Google Play Console.
- **Add Testers**
  - Add QA/tester email IDs to the internal testing list.
- **Distribute Testing Link**
  - Share the testing link with the QA team and stakeholders.
- **Test Critical Flows**
  - Validate all major flows: login, signup, payments, push notifications, deep links, etc.
- **Collect Feedback**
  - Gather feedback from testers and log issues in Jira/GitHub.

---

### Step 7: Share Build with MASSP

- [MASSP SmartSourceApps(Android)](https://dt-massp.app.tbdir.net/projects/20dc6337-eee7-408f-bff3-840d8cb9cde9)
- [MASSP StarSource(Android)](https://dt-massp.app.tbdir.net/projects/44f5867a-3d11-4b22-92eb-93c2b2c8ebd9)

  **Process:**
  - **Generate & Sign Build:**
    Generate and sign the APK/AAB using the release key.
  - ![android_build.png](android_build.png)
  - ![android_abb_apk.png](android_abb_apk.png)
  - ![android_select_product.png](android_select_product.png)
    - **Submit for Review:**  
      Upload the signed build for review and rollout.
    - **Share with MASSP:**  
      Share release notes and the testing/production link with MASSP for validation.
  ---
**Note:**
- The signed AAB/APK will be located in  
  `app/build/outputs/bundle/release/`
---

## Build Generation via GitHub Actions
how to generate a build for the project using GitHub Actions.

![gitHubActions.png](gitHubActions.png)

### Step 1: Open GitHub

- Go to [GitHub Enterprise](https://git.t3.daimlertruck.com/).

### Step 2: Select the Repository

- Navigate to the repository:  
  [`NA-CSG-Driver-Services/ds-mobile-smartsource-qrapp`](https://git.t3.daimlertruck.com/NA-CSG-Driver-Services/ds-mobile-smartsource-qrapp)

### Step 3: Open the Actions Tab

- Click on the **Actions** tab at the top of the repository page.

### Step 4: Choose the Workflow

- In the **Actions** section, locate the **Workflows** sidebar.
- Select the workflow file: `qa-release.yml`

### Step 5: Run the Workflow

- Click **Run workflow**.
- Fill out the workflow form with the following options:
  - **Branch:** `develop`
  - **Product to deploy:** `WSS_HMIEVO` / `FSS_HMIEVO`
  - **Environment to deploy:** `QA`

---
**Note:**  
After running the workflow, monitor the progress and download the generated build artifacts once the workflow completes.