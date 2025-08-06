# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

### Library Module
```bash
# Build the library
./gradlew :documentscanner:build

# Run unit tests
./gradlew :documentscanner:test

# Run instrumented tests (requires connected device/emulator)
./gradlew :documentscanner:connectedAndroidTest

# Clean build
./gradlew :documentscanner:clean
```

### Demo Application
```bash
# Build and install demo app
./gradlew :documentscanner-demo:installDebug

# Run demo app
./gradlew :documentscanner-demo:assembleDebug

# Clean demo
./gradlew :documentscanner-demo:clean
```

### Full Project
```bash
# Build everything
./gradlew build

# Run all tests
./gradlew test

# Clean everything
./gradlew clean
```

## Architecture Overview

This is a multi-module Android project with an OpenCV-based document scanning library and demo application.

### Module Structure
- **documentscanner/**: Main library module providing document scanning functionality
- **documentscanner-demo/**: Demo application demonstrating library usage

### Core Architecture Components

1. **DocumentScanner** (library entry point): Handles Activity result contracts and provides callback-based API with success/error/cancel handlers. Supports Base64 or file path response types.

2. **DocumentScannerActivity**: Main scanning UI that manages camera integration, document detection, corner adjustment, and multi-document scanning (up to 24 documents).

3. **DocumentDetector**: OpenCV-powered computer vision component that detects document corners using edge detection algorithms. Falls back to image bounds if corners can't be detected.

### Key Technical Details

- **OpenCV Integration**: Uses OpenCV 4.1.0 for computer vision operations
- **Camera Handling**: Direct camera capture with FileProvider for secure file sharing
- **Image Processing**: Perspective correction and cropping based on detected/adjusted corners
- **API Pattern**: Callback-based with ActivityResultContract for modern Android development
- **Orientation**: Portrait-only with full-screen theme for optimal scanning experience

### Data Flow
1. DocumentScanner launches DocumentScannerActivity with configuration
2. Camera captures photo and saves to temporary file
3. DocumentDetector analyzes image to find document corners
4. User can adjust corners manually if enabled
5. Image is cropped and perspective-corrected
6. Result returned as Base64 or file path based on configuration

### Configuration Constants (DefaultSetting.kt)
- Image Quality: 100 (lossless)
- Max Documents: 24
- User Corner Adjustment: Enabled by default
- Response Type: File path by default

## Development Notes

### Gradle Configuration
- Gradle 8.9 with Android Gradle Plugin 7.2.2
- Kotlin 1.6.10
- Library targets SDK 30 (min SDK 21)
- Demo app targets SDK 32

### Testing
- Unit tests in `documentscanner/src/test/`
- Instrumented tests in `documentscanner/src/androidTest/`
- Both modules have minimal test coverage currently

### Publishing
- Configured for Maven Central publishing with signing
- Version managed in library's build.gradle (currently 1.3.5)

### Required Permissions
- Camera hardware feature (required)
- External storage read/write permissions