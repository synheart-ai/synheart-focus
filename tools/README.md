# Tools

Development tools for the Synheart Focus SDK.

## Available Tools

### HSI Schema Validation

**validate_hsi_schema.py** (Coming Soon)

Validates that the Focus SDK output schema matches the HSI (Human State Interface) specification from synheart-core.

This tool is used in CI/CD to ensure compatibility between synheart-focus and synheart-core.

**Usage:**
```bash
python tools/validate_hsi_schema.py
```

**Requirements:**
- Python 3.8+
- Access to `../synheart-core/docs/HSI_SPECIFICATION.md`

## Future Tools

As the Focus SDK develops, additional tools may be added:

- **Test data generator**: Generate synthetic focus scenarios for testing
- **Performance benchmarks**: Measure inference latency and resource usage
- **Model validation**: Verify focus model outputs
- **Cross-platform testing**: Test consistency across SDKs

## Contributing

When adding new tools:

1. Create a dedicated directory for complex tools
2. Add a README.md explaining the tool's purpose and usage
3. Update this main README with tool information
4. Ensure tools work across all supported platforms
5. Add CI/CD integration where appropriate

See [CONTRIBUTING.md](../CONTRIBUTING.md) for more details.
