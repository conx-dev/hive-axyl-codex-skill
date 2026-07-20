# Hive Axyl SDK Installation

Official docs: https://conx-dev.github.io/hive-axyl-docs/

All SDKs need:

- `projectId` from the Hive Axyl console project.
- `apiKey` issued for the project and runtime environment.

## Web

```bash
npm install @hive-axyl/web-sdk
```

```ts
import { createHiveAxyl } from "@hive-axyl/web-sdk";

const hive = createHiveAxyl({
  projectId: "your-project-id",
  apiKey: "your-api-key",
});

await hive.init();
```

## Android

`settings.gradle.kts`:

```kotlin
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        google()
        mavenCentral()
    }
}
```

App module `build.gradle.kts`:

```kotlin
dependencies {
    implementation("io.github.conx-dev:hive-axyl-android-sdk:0.1.0")
}
```

## iOS

```swift
dependencies: [
    .package(
        url: "https://github.com/conx-dev/hive-axyl-ios-sdk.git",
        from: "0.1.0"
    )
]

.target(
    name: "YourApp",
    dependencies: [
        .product(name: "HiveAxylSDK", package: "hive-axyl-ios-sdk")
    ]
)
```

## Unity

`Packages/manifest.json`:

```json
{
  "dependencies": {
    "com.hiveaxyl.sdk": "https://github.com/conx-dev/hive-axyl-unity-sdk.git#0.4.0"
  }
}
```

## Godot

```bash
git submodule add https://github.com/conx-dev/hive-axyl-godot-sdk.git addons/hive_axyl
git -C addons/hive_axyl checkout 0.4.0
```

## Integration Notes

- Fetch login providers before rendering provider buttons.
- Use Guest only when the console login-provider configuration exposes it.
- Keep OAuth client secrets and payment verification credentials out of game clients.
- Store full API keys outside source control. The console shows full API keys only once after issue.
