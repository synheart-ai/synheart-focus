# Contributing to Synheart Focus

We welcome contributions to Synheart Focus! This is a multi-platform project containing SDKs for Python, Flutter/Dart, Android/Kotlin, and iOS/Swift. This document provides guidelines for contributing to the project.

## 🚀 Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/your-username/synheart-focus.git
   cd synheart-focus
   ```
3. **Install dependencies** based on which SDK you're working on (see below)

## 📂 Repository Structure

This is a monorepo containing multiple SDKs:

```
synheart-focus/
├── docs/             # Documentation (Architecture, API Reference)
├── models/           # Model definitions and assets (if applicable)
├── examples/         # Example applications
├── scripts/          # Build and deployment scripts
└── [SDK repos]       # Platform-specific SDK repositories
```

**Platform-specific SDK repositories** (maintained separately):
- `synheart-focus-python` - Python SDK
- `synheart-focus-dart` - Flutter/Dart SDK
- `synheart-focus-kotlin` - Android/Kotlin SDK
- `synheart-focus-swift` - iOS/Swift SDK

## 🧪 Development Setup by SDK

### Python SDK

```bash
cd synheart-focus-python

# Install dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Run tests
pytest

# Run tests with coverage
pytest --cov=src/synheart_focus --cov-report=html
```

### Flutter/Dart SDK

```bash
cd synheart-focus-dart

# Install dependencies
flutter pub get

# Run tests
flutter test

# Run tests with coverage
flutter test --coverage
```

### Kotlin/Android SDK

```bash
cd synheart-focus-kotlin

# Build
./gradlew build

# Run tests
./gradlew test
```

### Swift/iOS SDK

```bash
cd synheart-focus-swift

# Build
swift build

# Run tests
swift test
```

## 📝 Development Workflow

1. **Create a feature branch**:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**:
   - Follow the code style for each platform
   - Add tests for new functionality
   - Update documentation as needed

3. **Run tests**:
   - Ensure all tests pass
   - Check code coverage

4. **Commit your changes**:
   ```bash
   git commit -m "Add: description of your changes"
   ```

5. **Push to your fork**:
   ```bash
   git push origin feature/your-feature-name
   ```

6. **Create a Pull Request**:
   - Reference any related issues
   - Provide a clear description of changes
   - Include test results

## 🎨 Code Style

### Python
- Follow PEP 8
- Use type hints
- Maximum line length: 100 characters
- Use `black` for formatting
- Use `isort` for import sorting

### Dart
- Follow [Effective Dart](https://dart.dev/guides/language/effective-dart)
- Use `dart format` for formatting
- Follow Flutter conventions

### Kotlin
- Follow [Kotlin Coding Conventions](https://kotlinlang.org/docs/coding-conventions.html)
- Use `ktlint` for formatting

### Swift
- Follow [Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
- Use `swiftformat` for formatting

## 🧪 Testing

- Write tests for all new functionality
- Maintain or improve code coverage
- Test on multiple platforms when applicable
- Include edge cases and error conditions

## 📚 Documentation

- Update README.md if adding new features
- Add API documentation for public APIs
- Include usage examples
- Update CHANGELOG.md for user-facing changes

## 🐛 Reporting Bugs

Use the [GitHub issue tracker](https://github.com/synheart-ai/synheart-focus/issues) and include:

- Description of the bug
- Steps to reproduce
- Expected behavior
- Actual behavior
- Platform and version information
- Relevant logs or error messages

## 💡 Feature Requests

We welcome feature requests! Please:

- Check if the feature already exists
- Describe the use case
- Explain the expected behavior
- Consider implementation complexity

## 🏗️ Coordination with Synheart Core (HSI)

synheart-focus is consumed by `synheart-core`'s FocusHead module. When making changes, coordinate with synheart-core to ensure compatibility.

### Dependency Architecture

```
Runtime Dependency (package):
  synheart-core → synheart-focus
  (core depends on focus package)

Schema Validation (no code dependency):
  synheart-focus validates against:
    ../synheart-core/docs/HSI_SPECIFICATION.md
```

### HSI Schema Compatibility Requirements

**Critical APIs** (used by synheart-core FocusHead):
- `FocusEngine.infer(hsi_data, behavior_data)` - Inference interface
- `FocusResult` - Output schema (must map to HSI FocusState)
- `FocusConfig` - Configuration interface

### Making Changes That Affect HSI

When modifying these components, ensure:

1. **API Backward Compatibility**: Don't break FocusEngine interface
   - FocusHead relies on stable API surface
   - Use deprecation warnings for API changes
   - Provide migration path for breaking changes

2. **Schema Compatibility**: FocusResult must map to HSI FocusState
   ```
   FocusResult field      → HSI FocusState field
   ──────────────────────────────────────────────
   focus_score            → focus.score
   cognitive_load         → focus.cognitiveLoad
   clarity                → focus.clarity
   distraction            → focus.distraction
   focus_label            → (categorical mapping)
   ```

3. **Run HSI Schema Validation**:
   ```bash
   # Validate against HSI specification
   python tools/validate_hsi_schema.py --spec ../synheart-core/docs/HSI_SPECIFICATION.md
   ```

   This script checks:
   - FocusResult fields match HSI FocusState schema
   - Output formats are compatible
   - Data types and ranges align

4. **CI Enforcement**: Schema validation runs automatically on every PR
   - GitHub Actions workflow: `.github/workflows/hsi_schema_check.yml`
   - Fails PR if schema incompatibility detected

5. **Coordinate with Core Team**:
   - Notify synheart-core team before releasing breaking changes
   - Update synheart-core integration tests if changing FocusEngine behavior
   - Coordinate version bumps with core team

### Testing HSI Integration

When making changes that affect HSI integration:

1. **Local Testing** (if you have synheart-core checked out):
   ```bash
   # Test standalone
   cd synheart-focus
   pytest  # or flutter test, etc.

   # Test HSI integration
   cd ../synheart-core
   flutter test test/heads/focus_head_test.dart
   ```

2. **Verify FocusHead Compatibility**:
   - Check that FocusHead can initialize FocusEngine
   - Verify data mapping from HSV to FocusEngine works
   - Confirm FocusResult maps correctly to HSV.focus

3. **Cross-Platform Consistency**:
   - Changes must work across Python, Dart, Kotlin, Swift implementations
   - Verify all platforms produce consistent results

### Breaking Change Protocol

If you need to make a breaking change to FocusEngine or FocusResult:

1. **Document Impact**: Describe impact on synheart-core FocusHead
2. **Create Issue**: Open issue in synheart-core repo
3. **Migration Path**: Provide code examples for migration
4. **Deprecation Period**: Give 2+ releases notice before removal
5. **Coordinated Release**: Sync releases with synheart-core team
6. **Update HSI Spec**: If schema changes, update `../synheart-core/docs/HSI_SPECIFICATION.md`

### Example: Adding New Focus Metric

If adding a new output metric (e.g., "mental_fatigue"):

1. ✅ Update FocusResult to include "mental_fatigue"
2. ✅ Update models to output new metric
3. ✅ Run schema validation against HSI spec
4. ❌ **DO NOT** assume HSI will automatically support it
5. ✅ Create issue in synheart-core to discuss HSI integration
6. ✅ Update HSI_SPECIFICATION.md (if approved)
7. ✅ Wait for synheart-core FocusHead update before releasing

## 📄 License

By contributing, you agree that your contributions will be licensed under the MIT License.

## 🙏 Thank You!

Thank you for contributing to Synheart Focus! Your efforts help make cognitive concentration inference accessible to everyone.

---

**Author**: Israel Goytom  
**Organization**: Synheart Research & Engineering

