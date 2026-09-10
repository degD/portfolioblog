---
date: '2026-09-10T21:59:58+03:00'
draft: false
title: 'Omnihealth'
---

[OmniHealth](https://gitlab.com/den.ege.der/cdtpapp) is a system project for monitoring 
the health of patients. It includes a caregiver dashboard, a mobile app and device control.
The mobile app is built with React Native (Expo). Background notifications are integrated 
via Firebase. Health data is received and transmitted to the phone using an ESP32. Caregivers 
can monitor this data through an online dashboard.

### Patient Mobile App (React Native + Expo)
- Real-time health monitoring via BLE connection to ESP32 device
- Heart rate and accelerometer data collection
- Emergency detection and alerts
- Background service for continuous monitoring
- Data synchronization with Firebase

### Caretaker Web Dashboard
- Real-time patient monitoring
- Historical data visualization
- Multi-patient management
- Log download functionality

## Android Build Optimization

The original build configuration was:
- Building for 4 different CPU architectures simultaneously
- Running resource-intensive PNG optimization on every build
- Using default Gradle settings without caching
- Limited JVM memory allocation

This resulted in extremely slow builds (12-13 minutes). Applying the following
optimizations reduced the build time drastically (to 2-3 minutes). Apply the 
optimizations mentioned below and run `cd android && .\gradlew.bat assembleDevRelease`.

### Optimization 1: Enable Gradle Build Cache

Gradle caches the output of tasks (compilation, resource processing, etc.) to
speed up subsequent build processes. Massive speed increase.

**What was added:**
```properties
# android/gradle.properties
# Enable Gradle build cache for incremental builds
org.gradle.caching=true
```

### Optimization 2: Enable Gradle Daemon

Keeps a JVM process running in the background between builds and 
maintains warm caches in memory. Saves up to 30 seconds per build.

**What was added:**
```properties
# android/gradle.properties
# Enable Gradle Daemon for faster builds
org.gradle.daemon=true
```

### Optimization 3: Enable Configuration On Demand

Only configures the projects that are actually needed for the build.
Skips configuration of unrelated modules. Slight speed up.

**What was added:**
```properties
# android/gradle.properties
# Configure on demand to only build what's needed
org.gradle.configureondemand=true
```

### Optimization 4: Reduce Architecture Targets

Each architecture requires separate compilation of all native code.
Building 4 architectures is almost 4x the work. `arm64-v8a` covers 
95%+ of modern Android devices. Results in 60-75% faster native compilation.

**What was changed:**
```properties
# android/gradle.properties

# BEFORE (building 4 architectures):
reactNativeArchitectures=armeabi-v7a,arm64-v8a,x86,x86_64

# AFTER (building 1 architecture):
reactNativeArchitectures=arm64-v8a
```

### Optimization 5: Disable PNG Crunching

PNG crunching optimizes images to reduce APK size. But this process is 
very slow (can take 2-5 minutes). It is not necessary for development.

**What was changed:**
```properties
# android/gradle.properties

# BEFORE:
android.enablePngCrunchInReleaseBuilds=true

# AFTER:
android.enablePngCrunchInReleaseBuilds=false
```

### Optimization 6: Increase JVM Memory

More memory allows Gradle to cache more data in RAM and enables better 
parallel task execution. Up to 20% faster compilation.

**What was changed:**
```properties
# BEFORE:
# android/gradle.properties

org.gradle.jvmargs=-Xmx2048m -XX:MaxMetaspaceSize=512m

# AFTER:
org.gradle.jvmargs=-Xmx4096m -XX:MaxMetaspaceSize=1024m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8
```

### Optimization 7: Create DevRelease Build Type

This new build variant is optimized for speed. It skips all 
time-consuming optimization steps. 30-50% faster than regular 
builds.

**What was added:**
```gradle
// android/app/build.gradle

buildTypes {
    debug {
        signingConfig signingConfigs.debug
    }
    
    // NEW: Fast development release build
    devRelease {
        initWith release
        signingConfig signingConfigs.debug
        minifyEnabled false        // Skip code minification
        shrinkResources false      // Skip resource shrinking
        crunchPngs false          // Skip PNG optimization
        matchingFallbacks = ['release']
    }
    
    release {
        signingConfig signingConfigs.debug
        minifyEnabled enableMinifyInReleaseBuilds
        shrinkResources enableShrinkResources.toBoolean()
        crunchPngs enablePngCrunchInRelease.toBoolean()
    }
}
```

## Mobile App Speech Recognition

The app uses **react-native-vosk** for offline speech-to-text (STT) recognition. 
This allows users to speak keywords or phrases that are automatically transcribed 
and saved in the Settings tab. 

### Included Model
- **Name**: vosk-model-small-en-us-0.15
- **Language**: English (US)
- **Size**: ~40MB
- **Use Case**: General purpose, lightweight model

### Advanced Options

#### Grammar-based Recognition

Limit recognition to specific phrases:

```typescript
await start({ 
  grammar: ['left', 'right', 'up', 'down', '[unk]'] 
});
```

The `[unk]` token allows the recognizer to still detect phrases outside the grammar.

#### Timeout

Auto-stop after a duration:

```typescript
await start({ 
  timeout: 5000 // 5 seconds
});
```
