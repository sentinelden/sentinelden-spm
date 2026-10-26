# Retired: each SDK has its own package

This package listed all eleven Sentinel Den iOS SDKs as binary targets.
SwiftPM and Xcode download every binary target in a package, not only the
products a project uses, so it resolved only with a license covering all
eleven, and none does: the SDKs a license does not cover were refused, and the
whole resolve failed. It is archived and will not be updated.

Add the package for each SDK you licensed instead. Each one pins the same
signed binary by SHA-256, is tagged with the SDK's version, and signs in with
the same account email and download token in `~/.netrc`:

| SDK | Package |
|---|---|
| AgenticGuard | https://github.com/sentinelden/agenticguard-spm |
| AnomalyKit | https://github.com/sentinelden/anomalykit-spm |
| BehaviorGuard | https://github.com/sentinelden/behaviorguard-spm |
| EnclaveVault | https://github.com/sentinelden/enclavevault-spm |
| InputGuard | https://github.com/sentinelden/inputguard-spm |
| IntentKit | https://github.com/sentinelden/intentkit-spm |
| PayloadGuard | https://github.com/sentinelden/payloadguard-spm |
| PresenceKit | https://github.com/sentinelden/presencekit-spm |
| RedactKit | https://github.com/sentinelden/redactkit-spm |
| RuntimeGuard | https://github.com/sentinelden/runtimeguard-spm |
| ScreenGuard | https://github.com/sentinelden/screenguard-spm |

In a `Package.swift`, replace the `sentinelden-spm` dependency with one per
SDK, and name that package in each product:

```swift
dependencies: [
    .package(url: "https://github.com/sentinelden/runtimeguard-spm", from: "1.4.0"),
],
targets: [
    .target(name: "App", dependencies: [
        .product(name: "RuntimeGuardSDK", package: "runtimeguard-spm"),
    ]),
]
```

In Xcode, remove `sentinelden-spm` under Package Dependencies and add the
per-SDK packages. Setup and troubleshooting are in each package's README and
at https://sentinelden.com/quickstart#packages.
