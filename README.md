# Synheart Focus

**Cognitive concentration inference engine — transforming biosignals and digital behavior into real-time focus intelligence**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Platform Support](https://img.shields.io/badge/platforms-Python%20%7C%20Dart%20%7C%20Kotlin%20%7C%20Swift-blue.svg)](#-sdks)

Synheart Focus is the cognitive concentration layer of Synheart — estimating moment-to-moment focus levels by fusing biosignals, behavioral interaction patterns, and circadian context. It powers Syni, Syni Life, SWIP, and any mind-aware application built on Synheart.

## 🚀 Features

- **🧠 Real-Time Focus Inference**: Continuous focus score estimation (0.0-1.0)
- **📊 Multimodal Fusion**: Combines HRV, stress, motion, and behavioral patterns
- **⚡ On-Device Processing**: Low-latency inference (< 20ms) with < 3MB model footprint
- **🎯 Focus Labels**: Discrete states (focused, distracted, scattered, fatigued)
- **📈 Cognitive Load Estimation**: Predicts mental workload and fatigue risk
- **🔒 Privacy-First**: No raw biometrics stored; only interpreted signals
- **🌐 Multi-Platform**: Python, Flutter/Dart, Android/Kotlin, iOS/Swift

## 📦 SDKs

All SDKs provide **identical functionality** with platform-idiomatic APIs. Each SDK is maintained in its own repository:

### Python SDK
```bash
pip install synheart-focus
```
📖 **Repository**: [synheart-focus-python](https://github.com/synheart-ai/synheart-focus-python)

### Flutter/Dart SDK
```yaml
dependencies:
  synheart_focus: ^0.1.0
```
📖 **Repository**: [synheart-focus-dart](https://github.com/synheart-ai/synheart-focus-dart)

### Android SDK (Kotlin)
```kotlin
dependencies {
    implementation("ai.synheart:focus:0.1.0")
}
```
📖 **Repository**: [synheart-focus-kotlin](https://github.com/synheart-ai/synheart-focus-kotlin)

### iOS SDK (Swift)
**Swift Package Manager:**
```swift
dependencies: [
    .package(url: "https://github.com/synheart-ai/synheart-focus-swift.git", from: "0.1.0")
]
```
📖 **Repository**: [synheart-focus-swift](https://github.com/synheart-ai/synheart-focus-swift)

## 📂 Repository Structure

This repository serves as the **source of truth** for shared resources across all SDK implementations:

```
synheart-focus/
├── docs/                          # Technical documentation
│   ├── ARCHITECTURE.md            # System architecture
│   ├── API_REFERENCE.md           # API documentation
│   └── INTEGRATION.md             # Integration guides
│
├── models/                        # ML model definitions (if applicable)
│   └── README.md                  # Model documentation
│
├── examples/                      # Cross-platform example applications
├── scripts/                       # Build and deployment scripts
└── CONTRIBUTING.md                # Contribution guidelines for all SDKs
```

**Platform-specific SDK repositories** (maintained separately):
- [synheart-focus-python](https://github.com/synheart-ai/synheart-focus-python) - Python SDK
- [synheart-focus-dart](https://github.com/synheart-ai/synheart-focus-dart) - Flutter/Dart SDK
- [synheart-focus-kotlin](https://github.com/synheart-ai/synheart-focus-kotlin) - Android/Kotlin SDK
- [synheart-focus-swift](https://github.com/synheart-ai/synheart-focus-swift) - iOS/Swift SDK

## 🎯 Quick Start

### Python (Recommended for Testing)

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

### Flutter/Dart

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

### System Flow

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
         HSI Focus Head
                │
      ┌─────────┴───────────┐
      ▼                     ▼
    Syni                Syni Life / SWIP / Platform
```

## 📚 Documentation

- [Architecture Guide](docs/ARCHITECTURE.md) - Detailed system architecture
- [API Reference](docs/API_REFERENCE.md) - Complete API documentation
- [Integration Guide](docs/INTEGRATION.md) - Integration with HSI, Syni, and other services
- [Model Card](docs/MODEL_CARD.md) - Model details and performance metrics

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

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Related Projects

- [Synheart Emotion](https://github.com/synheart-ai/synheart-emotion) - Physiological emotion inference
- [Synheart Behavior](https://github.com/synheart-ai/synheart-behavior) - Digital behavioral signal capture
- [Synheart Core SDK](https://github.com/synheart-ai/synheart-core-sdk) - Unified SDK for all Synheart features
- [Synheart Wear](https://github.com/synheart-ai/synheart-wear) - Wearable device integration

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

