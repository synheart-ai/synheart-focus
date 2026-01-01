# Synheart Focus

**Cognitive concentration inference engine — transforming biosignals and digital behavior into real-time focus intelligence**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Platform Support](https://img.shields.io/badge/platforms-Dart%20%7C%20Python%20%7C%20Kotlin%20%7C%20Swift-blue.svg)](#-sdks)

Synheart Focus is the cognitive concentration layer of Synheart — estimating moment-to-moment focus levels by fusing biosignals, behavioral interaction patterns, and circadian context. It powers Syni, Syni Life, SWIP, and any mind-aware application built on Synheart.

## 🚀 Features

- **🧠 Real-Time Focus Inference**: Continuous focus score estimation (0.0-1.0)
- **📊 Multimodal Fusion**: Combines HRV, stress, motion, and behavioral patterns
- **⚡ On-Device Processing**: Low-latency inference (< 20ms) with < 3MB model footprint
- **🎯 Focus Labels**: Discrete states (focused, distracted, scattered, fatigued)
- **📈 Cognitive Load Estimation**: Predicts mental workload and fatigue risk
- **🔒 Privacy-First**: No raw biometrics stored; only interpreted signals
- **🌐 Multi-Platform**: Dart/Flutter, Python, Kotlin, Swift
- **🏗️ HSI-Compatible**: Output schema validated against Synheart Core HSI specification

## 📦 SDKs

All SDKs provide **identical functionality** with platform-idiomatic APIs. Each SDK is maintained in its own repository:

### Dart/Flutter SDK
```yaml
dependencies:
  synheart_focus: ^0.1.0
```
📖 **Repository**: [synheart-focus-dart](https://github.com/synheart-ai/synheart-focus-dart)

### Python SDK
```bash
pip install synheart-focus
```
📖 **Repository**: [synheart-focus-python](https://github.com/synheart-ai/synheart-focus-python)

### Kotlin SDK
```kotlin
dependencies {
    implementation("ai.synheart:focus:0.1.0")
}
```
📖 **Repository**: [synheart-focus-kotlin](https://github.com/synheart-ai/synheart-focus-kotlin)

### Swift SDK
**Swift Package Manager:**
```swift
dependencies: [
    .package(url: "https://github.com/synheart-ai/synheart-focus-swift.git", from: "0.1.0")
]
```
📖 **Repository**: [synheart-focus-swift](https://github.com/synheart-ai/synheart-focus-swift)

## 🏗️ Relationship with Synheart Core (HSI)

Synheart Focus serves **two deployment modes**:

### 1. **Standalone SDK** (Direct Integration)
Use synheart-focus directly for focus-only applications:

```python
from synheart_focus import FocusEngine, FocusConfig

engine = FocusEngine.from_config(FocusConfig())
focus_state = engine.infer(hsi_data, behavior_data)
print(f"Focus Score: {focus_state.focus_score}")
```

**Use when:** Your app only needs focus estimation, not full human state intelligence.

### 2. **Via Synheart Core** (HSI Integration)
Use focus as part of a complete Human State Interface with emotion, behavior, and context:

```dart
import 'package:synheart_core/synheart_core.dart';

// Initialize synheart-core (includes focus capability)
await Synheart.initialize(
  userId: 'user_123',
  config: SynheartConfig(
    enableWear: true,
    enableBehavior: true,
  ),
);

// Enable focus interpretation layer
await Synheart.enableFocus();

// Get focus updates (powered by synheart-focus under the hood)
Synheart.onFocusUpdate.listen((focus) {
  print('Focus Score: ${focus.score}');
  print('Cognitive Load: ${focus.cognitiveLoad}');
});
```

**Use when:** You want focus as part of a unified human state representation (HSV).

### Architecture & Dependencies

```
┌─────────────────────────────────────────────────────┐
│          Synheart Core (HSI Runtime)                │
│                                                     │
│  FocusHead Module                                   │
│    └─► depends on synheart-focus package           │
│         (runtime dependency)                        │
└─────────────────────────────────────────────────────┘
                      ▲
                      │
                      │ runtime: package dependency
                      │ schema: validates against HSI spec
                      │
┌─────────────────────────────────────────────────────┐
│          synheart-focus (this repo)                 │
│                                                     │
│  • Standalone focus inference SDK                   │
│  • NO code dependency on synheart-core              │
│  • Output schema validated against:                 │
│    ../synheart-core/docs/HSI_SPECIFICATION.md       │
│                                                     │
│  FocusEngine → FocusResult                          │
│                                                     │
└─────────────────────────────────────────────────────┘
```

**Key Principles:**
- ✅ **Standalone**: synheart-focus works independently, no core dependency
- ✅ **HSI-Compatible**: Output schema matches HSI FocusState specification
- ✅ **Schema Validation**: CI enforces compatibility with HSI spec
- ✅ **Used by Core**: synheart-core's FocusHead uses synheart-focus as implementation
- ✅ **Backward Compatible**: Existing standalone users unaffected

## 📂 Repository Structure

This repository serves as the **source of truth** for shared resources across all SDK implementations:

```
synheart-focus/                    # Source of truth repository
├── models/                        # ML model definitions and assets
│   └── README.md                  # Model documentation
│
├── docs/                          # Technical documentation
│   ├── ARCHITECTURE.md            # System architecture
│   ├── API_REFERENCE.md           # API documentation
│   ├── INTEGRATION.md             # Integration guides
│   └── MODEL_CARD.md              # Model details and performance
│
├── tools/                         # Development tools
│   ├── validate_hsi_schema.py     # HSI schema validation (CI)
│   └── README.md                  # Tools documentation
│
├── examples/                      # Cross-platform example applications
│   └── README.md                  # Examples documentation
├── scripts/                       # Build and deployment scripts
│   └── README.md                  # Scripts documentation
├── .github/workflows/             # CI/CD including HSI schema checks
├── CHANGELOG.md                   # Version history for all SDKs
└── CONTRIBUTING.md                # Contribution guidelines for all SDKs
```

**Platform-specific SDK repositories** (maintained separately):
- [synheart-focus-dart](https://github.com/synheart-ai/synheart-focus-dart) - Dart/Flutter SDK
- [synheart-focus-python](https://github.com/synheart-ai/synheart-focus-python) - Python SDK
- [synheart-focus-kotlin](https://github.com/synheart-ai/synheart-focus-kotlin) - Kotlin SDK
- [synheart-focus-swift](https://github.com/synheart-ai/synheart-focus-swift) - Swift SDK

## 🎯 Quick Start

### Dart/Flutter

```dart
import 'package:synheart_focus/synheart_focus.dart';

// Initialize
final focusEngine = FocusEngine.initialize(
  config: FocusConfig(),
);

// Subscribe to updates
focusEngine.onUpdate.listen((focusState) {
  print('Focus Score: ${focusState.focusScore}');
  print('Label: ${focusState.focusLabel}');
});

// Provide inputs and get focus state
final hsiData = HSIData(
  hr: 72,
  hrvRmssd: 45,
  stressIndex: 0.3,
  motionIntensity: 0.1,
);

final behaviorData = BehaviorData(
  taskSwitchRate: 0.2,
  interactionBurstiness: 0.15,
  idleRatio: 0.1,
);

final focusState = await focusEngine.infer(hsiData, behaviorData);
```

### Python

```python
from synheart_focus import FocusEngine, FocusConfig

# Initialize engine
config = FocusConfig()
engine = FocusEngine.from_config(config)

# Subscribe to focus updates
def on_focus_update(focus_state):
    print(f"Focus Score: {focus_state.focus_score}")
    print(f"Label: {focus_state.focus_label}")
    print(f"Cognitive Load: {focus_state.cognitive_load}")

engine.subscribe(on_focus_update)

# Provide HSI inputs
hsi_data = {
    "hr": 72,
    "hrv_rmssd": 45,
    "stress_index": 0.3,
    "motion_intensity": 0.1
}

behavior_data = {
    "task_switch_rate": 0.2,
    "interaction_burstiness": 0.15,
    "idle_ratio": 0.1
}

# Infer focus state
focus_state = engine.infer(hsi_data, behavior_data)
```

### Kotlin

```kotlin
import ai.synheart.focus.*

val config = FocusConfig()
val engine = FocusEngine.fromConfig(config)

// Subscribe to updates
engine.subscribe { focusState ->
    println("Focus Score: ${focusState.focusScore}")
    println("Label: ${focusState.focusLabel}")
    println("Cognitive Load: ${focusState.cognitiveLoad}")
}

// Provide HSI inputs
val hsiData = mapOf(
    "hr" to 72,
    "hrv_rmssd" to 45,
    "stress_index" to 0.3,
    "motion_intensity" to 0.1
)

val behaviorData = mapOf(
    "task_switch_rate" to 0.2,
    "interaction_burstiness" to 0.15,
    "idle_ratio" to 0.1
)

// Infer focus state
val focusState = engine.infer(hsiData, behaviorData)
```

### Swift

```swift
import SynheartFocus

let config = FocusConfig()
let engine = try FocusEngine.fromConfig(config: config)

// Subscribe to updates
engine.subscribe { focusState in
    print("Focus Score: \(focusState.focusScore)")
    print("Label: \(focusState.focusLabel)")
    print("Cognitive Load: \(focusState.cognitiveLoad)")
}

// Provide HSI inputs
let hsiData: [String: Any] = [
    "hr": 72,
    "hrv_rmssd": 45,
    "stress_index": 0.3,
    "motion_intensity": 0.1
]

let behaviorData: [String: Any] = [
    "task_switch_rate": 0.2,
    "interaction_burstiness": 0.15,
    "idle_ratio": 0.1
]

// Infer focus state
let focusState = try engine.infer(hsiData: hsiData, behaviorData: behaviorData)
```

## 🏗️ Architecture

### Standalone Mode

Synheart Focus is a **multimodal fusion model** that combines:

### Inputs

1. **HSI (Biosignal) Inputs**:
   - Heart rate (HR)
   - Heart rate variability (HRV - RMSSD, stability, variability)
   - Stress index
   - Motion intensity / micro-jitter
   - Respiration proxies (if available)
   - HSI embedding vector
   - Short rolling history (2-5 minutes)
   - Circadian context

2. **Behavioral Inputs** (from Synheart Behavior SDK):
   - Task switch rate
   - Interaction burstiness
   - Idle ratio
   - Notification interruptions
   - Steady vs scattered interaction rhythm

3. **Context Inputs**:
   - Sleep deficit
   - Recovery score
   - Circadian phase
   - Time since last break
   - Time-of-day patterns

### Outputs

For every time window (30-60 seconds, updated every 1-2 minutes):

| Output | Description | Range |
|--------|-------------|-------|
| `focus_score` | Continuous focus estimate | 0.0 → 1.0 |
| `focus_label` | Discrete state | focused, distracted, scattered, fatigued |
| `focus_trend` | Short-term trend | increasing, decreasing, stable |
| `cognitive_load` | Workload estimate | low, normal, high |
| `deep_focus_block` | Sustained focus flag | true/false |
| `fatigue_risk` | Focus decline likelihood | 0.0 → 1.0 |

### System Flow (Standalone)

```
Wear SDK / Phone / Behavior SDKs
                │
                ▼
              HSI
     (cleaned signals + embeddings)
                │
                ▼
        Synheart Focus Engine
      (Tiny Transformer or CNN-LSTM)
                │
                ▼
           FocusResult
                │
                ▼
          Your Application
```

### HSI Integration Mode

When used via Synheart Core:

```
Synheart Core SDK
├── Wear Module (collects HR/RR from wearable)
├── Phone Module (device motion, screen state)
├── Behavior Module (interaction patterns)
│   └── HSI Runtime (processes biosignals, multimodal fusion)
│       └── FocusHead Module
│           └── synheart-focus FocusEngine
│               [Multimodal Fusion Model]
│                       │
│                  FocusResult
│                       │
│            mapped to HSV.focus
│                       │
│                       ▼
│         Complete Human State Vector
│         ├─ Focus (score, cognitive load, clarity)
│         ├─ Emotion (stress, calm, engagement)
│         ├─ Behavior (interaction patterns)
│         └─ Context (activity, environment)
│                       │
│         ┌─────────────┴───────────┐
│         ▼                         ▼
│       Syni              Syni Life / SWIP / Platform
```

## 📚 Documentation

- [Architecture Guide](docs/ARCHITECTURE.md) - Detailed system architecture
- [API Reference](docs/API_REFERENCE.md) - Complete API documentation
- [Integration Guide](docs/INTEGRATION.md) - Integration with HSI, Syni, and other services
- [Model Card](docs/MODEL_CARD.md) - Model details and performance metrics
- [Contributing Guide](CONTRIBUTING.md) - How to contribute (covers all SDKs)
- [Changelog](CHANGELOG.md) - Version history for all SDKs

## 🎯 Use Cases

### By Syni (AI Agent)
- Focus-aware tone adjustment
- Strategy selection based on focus state
- Interruption management during deep focus

### By Syni Life (Daily User App)
- Focus score card
- Deep focus block detection
- Daily and hourly focus trends
- Actionable insights ("Your focus is declining; take a 2-minute break.")

### By SWIP (Digital Wellness)
- Labeling digital sessions as focused, neutral, fragmented
- Focus-aware app scoring
- Session-level focus curves

### By Synheart Platform (Developer Portal)
- Developer dashboards
- Cognitive performance analytics
- Aggregated state insights

## ⚡ Performance

- **Inference Latency**: < 20ms on-device
- **Model Footprint**: < 3MB
- **Battery Impact**: Minimal (< 0.5%/hr)
- **Update Frequency**: Every 60-120 seconds
- **Cloud Aggregation**: < 15 seconds for daily summaries

## 🔒 Privacy & Security

- **No Content Captured**: No text, URLs, messages, or screen content
- **Only Timing + Biosignal Features**: Derived features only, no raw data
- **On-Device Processing**: All inference happens locally
- **Consent-Gated**: All behavioral and focus data requires explicit consent
- **No Data Retention**: Raw biometric data is not retained after processing
- **No Network Calls**: No data is sent to external servers
- **Privacy-First Design**: No built-in storage - you control what gets persisted
- **Non-Clinical**: Not a judgment or productivity metric; cannot diagnose impairment

## 📊 Benchmarks

- **Focus Score Accuracy**: High correlation with known behavioral patterns
- **Missing Samples**: < 5% per day
- **Inference Latency**: 95th percentile < 30ms
- **State Update Accuracy**: Within 1 window

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## 📄 License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Projects & Dependencies

### Consumed By

- **[Synheart Core SDK](https://github.com/synheart-ai/synheart-core-sdk)** - Unified SDK for all Synheart features
  - Uses synheart-focus as FocusHead implementation
  - Runtime dependency: synheart-core → synheart-focus
  - Schema validation: synheart-focus validates against HSI spec

### Related SDKs

- **[Synheart Emotion](https://github.com/synheart-ai/synheart-emotion)** - Physiological emotion inference
  - Similar architecture: standalone SDK used by synheart-core EmotionHead
  - Also validates against HSI specification

- **[Synheart Behavior](https://github.com/synheart-ai/synheart-behavior)** - Digital behavioral signal capture
  - Provides behavioral inputs for focus estimation
  - Used by: Behavior Module in synheart-core

- **[Synheart Wear](https://github.com/synheart-ai/synheart-wear)** - Wearable device integration
  - Provides biosignal inputs (HR, HRV) for focus estimation
  - Used by: Wear Module in synheart-core

### Dependency Architecture

```
Runtime Dependencies (package):
  synheart-core → synheart-focus (FocusHead implementation)
  synheart-focus → (standalone, no dependencies on core)

Schema Validation (no code dependency):
  synheart-focus ← validates against HSI_SPECIFICATION.md
```

**Key Principle:**
- synheart-focus remains a **standalone SDK**
- Can be used independently without synheart-core
- synheart-core uses it as implementation layer for FocusHead
- Output schema validated against HSI specification for compatibility

## 🔗 Links

- **Synheart AI**: [synheart.ai](https://synheart.ai)
- **Documentation**: [docs.synheart.ai](https://docs.synheart.ai)
- **Issues**: [GitHub Issues](https://github.com/synheart-ai/synheart-focus/issues)
- **Discussions**: [GitHub Discussions](https://github.com/synheart-ai/synheart-focus/discussions)

## 👥 Authors

- **Israel Goytom** - _Initial work_, _RFC Design & Architecture_
- **Synheart AI Team** - _Development & Research_

---

**Made with ❤️ by the Synheart AI Team**

_Technology with a heartbeat._

