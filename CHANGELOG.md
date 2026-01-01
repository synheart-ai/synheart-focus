# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial repository structure and documentation
- Multi-platform SDK architecture
- HSI compatibility framework
- Integration guides for standalone and HSI modes

## [0.1.0] - 2025-01-01

### Added

#### Multi-Platform SDK Support
- **Python SDK**: Cross-platform Python package
  - Will be published to PyPI as `synheart-focus`
  - Installable via `pip install synheart-focus`

- **Flutter/Dart SDK**: Primary SDK for Flutter applications
  - Will be published to pub.dev as `synheart_focus`
  - Full API parity with all features

- **Android SDK**: Kotlin library for Android applications
  - Will be published to Maven Central as `ai.synheart:focus`
  - Minimum SDK 21 (Android 5.0+)

- **iOS SDK**: Swift package for iOS/macOS/watchOS/tvOS
  - Available via Swift Package Manager
  - Supports iOS 13.0+, macOS 11.0+

#### Core Features
- Real-time focus inference engine (0.0-1.0 score)
- Multimodal fusion (biosignals + behavioral patterns)
- Discrete focus labels (focused, distracted, scattered, fatigued)
- Cognitive load estimation
- On-device processing (< 20ms latency)
- Privacy-first design
- HSI-compatible output schema

#### Architecture
- Standalone mode for focus-only applications
- HSI integration mode via Synheart Core
- Schema validation against HSI specification
- Cross-platform consistency

### Documentation
- Architecture guide
- API reference
- Integration guide
- Model card
- Contributing guidelines

### Architectural Decisions
- **No built-in storage**: Library focuses on focus inference only
- **Privacy-first**: All processing happens on-device, no network calls
- **HSI-compatible**: Output schema validated against HSI specification
- **Standalone capable**: Can be used independently or via Synheart Core

### Performance Targets
- **Inference Latency**: < 20ms on-device
- **Model Footprint**: < 3MB
- **Battery Impact**: Minimal (< 0.5%/hr)
- **Update Frequency**: Every 60-120 seconds

### Known Limitations
- Requires biosignal inputs (HR, HRV, stress index)
- Requires behavioral inputs for optimal performance
- SDK implementations pending initial release

[Unreleased]: https://github.com/synheart-ai/synheart-focus/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/synheart-ai/synheart-focus/releases/tag/v0.1.0
