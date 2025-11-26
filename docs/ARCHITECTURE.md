# Synheart Focus Architecture

## Overview

Synheart Focus is a multimodal fusion model that estimates cognitive concentration by combining biosignals, behavioral patterns, and contextual information. All processing happens on-device for privacy and real-time performance.

## System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Input Sources                         │
├─────────────────────────────────────────────────────────┤
│  Wear SDK          Phone SDK        Behavior SDK        │
│  (HR, HRV)        (Motion)        (Interactions)        │
└────────────┬──────────────┬──────────────┬──────────────┘
             │              │              │
             └──────────────┴──────────────┘
                            │
                            ▼
                    ┌───────────────┐
                    │  HSI Runtime  │
                    │  (Fusion)     │
                    └───────┬───────┘
                            │
                            ▼
                ┌───────────────────────┐
                │  Synheart Focus       │
                │  Engine               │
                │  (Inference Model)    │
                └───────┬───────────────┘
                        │
                        ▼
            ┌───────────────────────────┐
            │   Focus Outputs           │
            │  - focus_score            │
            │  - focus_label             │
            │  - cognitive_load          │
            │  - deep_focus_block        │
            └───────┬───────────────────┘
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
    Syni Core            Syni Life / SWIP
```

## Components

### 1. Input Layer

#### HSI (Biosignal) Inputs
- **Heart Rate (HR)**: Current heart rate in BPM
- **Heart Rate Variability (HRV)**: RMSSD, stability metrics, variability
- **Stress Index**: Derived stress level (0.0-1.0)
- **Motion Intensity**: Micro-jitter and movement patterns
- **Respiration Proxies**: If available from wearables
- **HSI Embedding**: 64D embedding vector from HSI Runtime
- **Circadian Context**: Time-of-day patterns

#### Behavioral Inputs
- **Task Switch Rate**: Frequency of app/task switching
- **Interaction Burstiness**: Variability in interaction patterns
- **Idle Ratio**: Proportion of idle time
- **Notification Interruptions**: Frequency and impact
- **Interaction Rhythm**: Steady vs scattered patterns

#### Context Inputs
- **Sleep Deficit**: Recent sleep quality/quantity
- **Recovery Score**: Physiological recovery state
- **Circadian Phase**: Current circadian rhythm phase
- **Time Since Last Break**: Break duration and timing
- **Time-of-Day Patterns**: Historical focus patterns

### 2. Processing Layer

#### HSI Runtime Integration
- Receives cleaned, normalized signals
- Produces HSI embedding vector
- Maintains short rolling history (2-5 minutes)
- Provides temporal context

#### Focus Engine
- **Model Type**: Tiny Transformer or CNN-LSTM
- **Input Window**: 30-60 seconds
- **Update Frequency**: Every 60-120 seconds
- **Processing**: On-device inference
- **Output**: Focus state with all metrics

### 3. Output Layer

#### Focus Metrics
- **focus_score**: Continuous estimate (0.0-1.0)
- **focus_label**: Discrete state classification
- **focus_trend**: Short-term trend direction
- **cognitive_load**: Mental workload estimate
- **deep_focus_block**: Sustained focus detection
- **fatigue_risk**: Predictive fatigue indicator

## Data Flow

### Real-Time Processing

1. **Signal Collection**: Wear/Phone/Behavior SDKs collect raw signals
2. **HSI Fusion**: HSI Runtime fuses signals into unified state
3. **Focus Inference**: Focus Engine processes HSI state
4. **State Update**: Focus state emitted to subscribers
5. **Cloud Upload**: (Optional) State uploaded if consent granted

### Window-Based Processing

- **Short Windows**: 30-60 seconds for real-time updates
- **Medium Windows**: 5 minutes for trend analysis
- **Long Windows**: 1 hour for daily summaries

## Model Architecture

### Focus Inference Model

The focus model is a lightweight neural network optimized for on-device inference:

- **Architecture**: Transformer or CNN-LSTM hybrid
- **Input Size**: Variable (depends on HSI features)
- **Output Size**: 6 values (focus metrics)
- **Parameters**: < 3MB
- **Latency**: < 20ms on mid-range devices

### Training Data

- Physiological signals from wearables
- Behavioral patterns from device interactions
- Ground truth labels from user feedback
- Contextual features (time, sleep, recovery)

## Privacy & Security

### On-Device Processing
- All inference happens locally
- No raw biometrics leave the device
- Only derived focus signals are stored

### Data Minimization
- Minimal data collection
- Consent-gated behavioral data
- Encrypted cloud uploads (if enabled)

### Privacy Guarantees
- No content extraction
- No raw signal storage
- User control over data sharing

## Performance Characteristics

### Resource Usage
- **CPU**: < 2% average
- **Memory**: < 15MB peak
- **Battery**: < 0.5%/hr impact
- **Network**: Minimal (only if cloud upload enabled)

### Latency
- **Inference**: < 20ms
- **State Update**: < 100ms end-to-end
- **Cloud Upload**: < 80ms per request

## Integration Points

### HSI Runtime
- Subscribes to HSI state updates
- Receives fused biosignal + behavioral features
- Provides temporal context

### Syni
- Receives focus state for tone adjustment
- Uses focus for strategy selection
- Manages interruptions during deep focus

### Syni Life
- Displays focus score card
- Shows focus trends and insights
- Provides actionable recommendations

### SWIP
- Labels sessions by focus level
- Correlates apps with focus impact
- Generates focus-aware analytics

## Future Enhancements

- **Fatigue Modeling**: Predictive focus decline
- **Personalization**: User-specific baselines
- **Forecasting**: Future focus state prediction
- **Task-Specific Models**: Different models for different activities

---

**Last Updated**: 2025-11-25  
**Author**: Israel Goytom

