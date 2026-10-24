# Sentinel Den SDKs for Swift Package Manager

The Sentinel Den iOS SDKs as one Swift package. Each SDK is a library
product backed by a binary XCFramework, so you add only the ones your licence
covers. iOS 17 or later.

## 1. Give SwiftPM a download token

The binaries are served to licensed accounts only. In your dashboard at
https://sentinelden.com/dashboard, create a download token under "Package
managers and CI" (it is shown once). SwiftPM signs in with your account email
as the user name and the token as the password. Add them to `~/.netrc` (Xcode
and the `swift` command both read it):

```
machine api.sentinelden.com
  login you@example.com
  password sdt_YOUR_DOWNLOAD_TOKEN
```

Your licence key does not work here: it ships inside your app, so it is not a
download credential.

Then `chmod 600 ~/.netrc`. On CI, write the same three lines from your secret
store before resolving packages.

Instead of `~/.netrc`, the `swift` command on macOS also reads an internet
password for `api.sentinelden.com` from the keychain.

## 2. Add the package

In Xcode: File > Add Package Dependencies, enter
`https://github.com/sentinelden/sentinelden-spm`, and pick the products you
licence.

In a `Package.swift`:

```swift
dependencies: [
    .package(url: "https://github.com/sentinelden/sentinelden-spm", from: "1.0.0"),
],
targets: [
    .target(name: "App", dependencies: [
        .product(name: "RuntimeGuardSDK", package: "sentinelden-spm"),
    ]),
]
```

Then `import RuntimeGuardSDK` and configure it as the SDK's docs describe.

## Versions

Each release of this package pins one version of every SDK, with the SHA-256
checksum of its released zip. SwiftPM refuses a download whose checksum does
not match. The SDK versions are listed next to each binary target in
`Package.swift`, and at https://sentinelden.com/changelog.

## If resolving fails

- **401**: the email or download token in `~/.netrc` is wrong, the token was
  replaced or revoked in the dashboard, or the line is for a different host.
- **403**: the licence does not cover that SDK, or it has lapsed. Check it in
  your dashboard at https://sentinelden.com/dashboard.
- **checksum mismatch**: tell us at https://sentinelden.com/contact; do not
  work around it.

ManifestGuard is not in this package: it must stay out of release builds,
and SwiftPM cannot link a product in Debug only. Download it from the
dashboard and add it to your Debug configuration.

Android SDKs install from the Sentinel Den Maven repository with the same
email and download token; see https://sentinelden.com/quickstart#android.
