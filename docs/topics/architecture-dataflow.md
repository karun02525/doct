# Architecture


## Architecture - Dataflow (MVVM, KMM)

### Overview
This project follows an MVVM pattern in the shared (KMM) module. UI observes `StateFlow` from shared `ViewModel`s and renders state reactively. Domain/data providers expose streams (for example, GPS speed) that are transformed in the `ViewModel` into UI\-ready state.

### Key concepts
- **Model**: data sources and providers (for example, GPS).
- **ViewModel** (shared): transforms model streams into `StateFlow` state and exposes UI actions.
- **View** (platform UI): collects `StateFlow` and shows/hides UI such as dialogs.

###  `SpeedViewModel` (shared)
Package: `com.daimler_trucksnorthamerica.smartsource.qrapp.shared.viewmodel`

### Dependencies
- `SpeedProvider`: exposes a `Flow` of current vehicle speed.
- `DeviceTypeProvider`: exposes current `DeviceType` (`CAR`, etc.).
- `Constants.SPEED_LOCK_THRESHOLD`: speed threshold (km/h) used to determine “in motion”.
- `appClose()`: shared function used to force close the app for unsupported devices.

### Exposed state
- `isCarDevice: StateFlow<Boolean>`  
  Indicates whether the app is running on a supported car device.
- `inMotion: StateFlow<Boolean>`  
  Derived from speed: `true` when `speed >= SPEED_LOCK_THRESHOLD`, otherwise `false`.

### More details
[System Architecture Diagram](https://con.t3.daimlertruck.com/spaces/DTNATELE/pages/516307547/3d.System+Architecture+Diagram)


### QR Code Architecture
![qrcode_system.png](qrcode_system.png)

### Dataflow
  - ![mvvm-speed-lock-dataflow.png](mvvm-speed-lock-dataflow.png)
  - ![data_flow_mvvm.png](data_flow_mvvm.png)
