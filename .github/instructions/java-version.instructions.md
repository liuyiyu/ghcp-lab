---
description: "Use when working with Java projects, build configurations, pom.xml, build.gradle, or Java version requirements. Ensures JDK version is higher than JDK 11."
applyTo: ["**/*.java", "**/pom.xml", "**/build.gradle", "**/build.gradle.kts", "gradle.properties", "**/.java-version"]
---

# Java Version Requirements

## Project Standards

- **Minimum JDK Version**: JDK 11
- All Java projects in this repository MUST use JDK 12 or higher
- Verify version configuration in build files before making changes

## Version Check Guidelines

When working with Java code or build configurations:

1. **Check Maven projects** (`pom.xml`):
   - Verify `maven.compiler.source` and `maven.compiler.target` are set to 12 or higher
   - Check `<java.version>` property if present

2. **Check Gradle projects** (`build.gradle`, `build.gradle.kts`):
   - Verify `sourceCompatibility` and `targetCompatibility` are set to 12 or higher
   - Check `java.toolchain.languageVersion` if using toolchains

3. **Alert on violations**:
   - If JDK 11 or lower is detected, inform the user immediately
   - Suggest upgrading to a supported version (JDK 17, 21, or latest LTS)

## Examples

### Maven (pom.xml)
```xml
<properties>
    <maven.compiler.source>17</maven.compiler.source>
    <maven.compiler.target>17</maven.compiler.target>
</properties>
```

### Gradle (build.gradle)
```groovy
java {
    sourceCompatibility = JavaVersion.VERSION_17
    targetCompatibility = JavaVersion.VERSION_17
}
```

### Gradle Kotlin DSL (build.gradle.kts)
```kotlin
java {
    sourceCompatibility = JavaVersion.VERSION_17
    targetCompatibility = JavaVersion.VERSION_17
}
```
