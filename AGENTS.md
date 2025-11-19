# AGENTS.md

This document provides essential information for AI agents working with the Iris Rev 8 QMK keymap repository.

## Project Overview

This is a custom QMK firmware keymap for the Keebio Iris Rev 8 split ergonomic keyboard. The project features a sophisticated layer system with RGB lighting, tap dance functionality, and custom macros.

## Key Components

### Core Files
- `keymap.c` - Main keymap implementation with all layers, tap dances, and RGB configuration
- `config.h` - QMK configuration settings specific to this keyboard
- `rules.mk` - Build configuration and feature flags
- `build.sh` - Automated build script using Podman containerization
- `Containerfile` - Docker/Podman container definition for reproducible builds
- `entry.sh` - Container entry script for QMK compilation

### Build System
This project uses containerized builds with Podman:
1. Run `./build.sh` to compile firmware
2. Resulting UF2 file appears in `./build-volume/build-output/`
3. Flash firmware by copying UF2 to both keyboard halves

## Keymap Architecture

### Layer Structure
- **QWERTY (Layer 0)** - Base typing layer
- **NUM (Layer 1)** - Function keys and numpad 
- **SYM_NAV (Layer 2)** - Symbols and navigation combined
- **MEDIA_MOUSE (Layer 3)** - Media controls and mouse keys combined
- **GAMING (Layer 4)** - Dedicated gaming layout
- **MACRO (Layer 5)** - Custom macros and utilities

### Special Features
- **Tap Dance**: Numbers 1-5 double-hold to switch layers
- **Momentary Layers**: Left brace for Fn, right brace for Mouse
- **RGB Lighting**: Per-layer color schemes with 68 LEDs total
- **Custom Macros**: TURBO and JIGGLER functionality
- **Split Keyboard**: Uses EE_HANDS for handedness detection

### Important Conventions
- Uses `_______` for transparent keys instead of `KC_TRNS` for readability
- Layer aliases defined for cleaner code (e.g., `QWERTY_LAYER`)
- LED index mapping documented in code comments
- RGB lighting layers correspond to keymap layers

## Development Guidelines

### When Modifying Keymaps
1. Maintain consistent visual layout with ASCII art comments
2. Use existing color schemes for new layers
3. Follow tap dance naming convention: `TD_X_LY` where X is character, LY is layer
4. Update RGB lighting if adding new layers

### When Adding Features
1. Check `rules.mk` for required feature flags
2. Add custom keycodes to `custom_keycodes` enum
3. Implement in `process_record_user()` function
4. Consider RGB feedback for new features

### Build Configuration
- RGBLIGHT_ENABLE must be `yes` for RGB layers
- RGB_MATRIX_ENABLE must be `no` (conflicts with RGBLIGHT)
- LTO_ENABLE enabled for firmware size optimization
- MOUSEKEY_ENABLE required for mouse functionality

## QuantumDuck Integration

The `QuantumDuck/` subdirectory contains a tool for converting Ducky Scripts to QMK SEND_STRING() macros. This can be useful for creating complex macro sequences.

## RGB LED Mapping

The keyboard has 68 LEDs with detailed index mapping in the code comments. Each layer has specific color:
- QWERTY: Purple
- NUM: Red  
- SYM_NAV: Azure/Green
- MEDIA_MOUSE: Orange/Yellow
- GAMING: Blue
- MACRO: White

## Common Tasks

### Adding New Layer
1. Add enum to `iris_layers`
2. Add layer alias define
3. Create keymap in `keymaps[][MATRIX_ROWS][MATRIX_COLS]`
4. Add RGB light layer definition
5. Update `MY_LIGHT_LAYERS` array
6. Update `layer_state_set_user()` function

### Adding Tap Dance
1. Add enum to `tap_dance_codes`
2. Implement `finished` and `reset` functions
3. Add to `tap_dance_actions` array
4. Use `TD()` macro in keymap

### Building Firmware
Simply run `./build.sh` - the script handles:
- QMK repository cloning/updating
- Container building
- Compilation
- UF2 file placement

## Troubleshooting

- If RGB doesn't work: ensure RGBLIGHT_ENABLE=yes and RGB_MATRIX_ENABLE=no
- If build fails: check Containerfile for missing dependencies
- If layers don't switch: verify SPLIT_LAYER_STATE_ENABLE in config.h
- For flashing issues: ensure both halves are in bootloader mode

## Repository Context

This is a personal keyboard configuration focused on productivity and gaming. The layer system is designed for efficient access to symbols, navigation, and media controls while maintaining a clean base layer.