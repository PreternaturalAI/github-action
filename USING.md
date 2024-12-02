# Preternatural GitHub Actions Usage Guide

This document catalogs the available Preternatural GitHub Actions and their usage examples.

## Table of Contents
- [Build Action](#build-action)
- [Archive & Notarize Action](#archive--notarize-action)

## Action Source Files

- [Build Action Source](preternatural-build/action.yml) - Implementation of the build action
- [Archive Action Source](preternatural-archive/action.yml) - Implementation of the archive and notarize action

## Build Action

The Build Action runs Preternatural build commands on repositories with specified Xcode configurations. The xcactivitylog steps are optional and will not stop the workflow if they fail.

### Build Action Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `xcode-version` | Xcode version to use for building | No | `latest-stable` |
| `platforms` | Target platforms to build for | No | `["macOS"]` |
| `configurations` | Build configurations to use | No | `["debug", "release"]` |
| `derived_data_path` | Path to the derived data folder | No | `DerivedData/ProjectBuild` |
| `reuse_derived_data` | Option to reuse derived data | No | `true` |

### Build Action Examples

#### Basic Build
```yaml
steps:
  - uses: PreternaturalAI/preternatural-build-action@v1
```

#### Multi-Platform Build
```yaml
steps:
  - uses: PreternaturalAI/preternatural-build-action@v1
    with:
      platforms: '["iOS", "macOS", "tvOS"]'
```

#### Debug-Only Build with Custom Derived Data
```yaml
steps:
  - uses: PreternaturalAI/preternatural-build-action@v1
    with:
      configurations: '["debug"]'
      derived_data_path: 'CustomDerivedData'
```

#### All Platforms Release Build
```yaml
steps:
  - uses: PreternaturalAI/preternatural-build-action@v1
    with:
      platforms: '["iOS", "macOS", "tvOS", "watchOS", "visionOS"]'
      configurations: '["release"]'
```

#### Build with Specific Xcode Version
```yaml
steps:
  - uses: PreternaturalAI/preternatural-build-action@v1
    with:
      xcode-version: '14.3.1'
      platforms: '["macOS"]'
```

#### Build with Reuse Derived Data Disabled
```yaml
steps:
  - uses: PreternaturalAI/preternatural-build-action@v1
    with:
      reuse_derived_data: false
```

#### Build with xcactivitylog Steps
```yaml
steps:
  - uses: PreternaturalAI/preternatural-build-action@v1
    with:
      xcode-version: 'latest-stable'
      platforms: '["macOS"]'
      configurations: '["debug", "release"]'
      derived_data_path: 'DerivedData/ProjectBuild'
      reuse_derived_data: true
```

## Archive & Notarize Action

The Archive & Notarize Action creates and notarizes a macOS application using the Preternatural CLI.

### Archive Action Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `xcode-version` | Xcode version to use | Yes | `latest-stable` |
| `notarization_username` | App Store Connect Username | Yes | - |
| `notarization_password` | App Store Connect Password | Yes | - |
| `notarization_team_id` | App Store Connect Team ID | No | - |
| `build_certificate_base64` | Base64-encoded Apple certificate | Yes | - |
| `p12_password` | Password for the P12 certificate | Yes | - |

### Archive Action Examples

#### Basic Archive and Notarize
```yaml
steps:
  - uses: PreternaturalAI/preternatural-archive-action@v1
    with:
      notarization_username: ${{ secrets.NOTARIZATION_USERNAME }}
      notarization_password: ${{ secrets.NOTARIZATION_PASSWORD }}
      build_certificate_base64: ${{ secrets.BUILD_CERTIFICATE }}
      p12_password: ${{ secrets.P12_PASSWORD }}
```

#### Archive with Team ID and Specific Xcode Version
```yaml
steps:
  - uses: PreternaturalAI/preternatural-archive-action@v1
    with:
      xcode-version: '15.2'
      notarization_username: ${{ secrets.NOTARIZATION_USERNAME }}
      notarization_password: ${{ secrets.NOTARIZATION_PASSWORD }}
      notarization_team_id: 'YOUR_TEAM_ID'
      build_certificate_base64: ${{ secrets.BUILD_CERTIFICATE }}
      p12_password: ${{ secrets.P12_PASSWORD }}
```

#### Archive with Custom Team Setup
```yaml
steps:
  - uses: PreternaturalAI/preternatural-archive-action@v1
    with:
      xcode-version: '15.2'
      notarization_username: ${{ secrets.NOTARIZATION_USERNAME }}
      notarization_password: ${{ secrets.NOTARIZATION_PASSWORD }}
      notarization_team_id: ${{ secrets.TEAM_ID }}
      build_certificate_base64: ${{ secrets.ENTERPRISE_CERTIFICATE }}
      p12_password: ${{ secrets.ENTERPRISE_CERT_PASSWORD }}
```

#### Archive for Enterprise Distribution
```yaml
steps:
  - uses: PreternaturalAI/preternatural-archive-action@v1
    with:
      notarization_username: ${{ secrets.ENTERPRISE_USERNAME }}
      notarization_password: ${{ secrets.ENTERPRISE_PASSWORD }}
      notarization_team_id: ${{ secrets.ENTERPRISE_TEAM_ID }}
      build_certificate_base64: ${{ secrets.ENTERPRISE_CERTIFICATE }}
      p12_password: ${{ secrets.ENTERPRISE_CERT_PASSWORD }}
```

## Features

### Build Action Features
- Derived data caching
- Detailed build logs
- Multi-platform support
- Various project structure support

### Archive Action Features
- Automatic certificate installation
- Notarization process handling
- Artifact uploading
- Team ID support
- Secure secrets handling

## Requirements

- macOS runner
- GitHub Actions environment
- For notarization:
  - Valid Apple Developer account
  - App Store Connect credentials
  - Valid certificate and provisioning profile
