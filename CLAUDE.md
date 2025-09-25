# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Common Development Commands

### Script Commands
- `scripts/build` - Build the macOS app in Debug mode
- `scripts/br` - Build and run the app (copies to /Applications and launches)
- `scripts/run` - Run the app without building (app must be built first)
- `scripts/ide` - Open the project in Xcode
- `scripts/lint` - Run shellcheck on all shell scripts in the project

## Architecture Overview

Swift Quit is a macOS menu bar application that automatically quits apps when their last window is closed. The codebase is structured as follows:

### Core Components

1. **AppDelegate.swift**: Application lifecycle management
   - Initializes accessibility permissions check
   - Sets up Swindler state for window monitoring
   - Manages menu bar UI and settings window
   - Handles app launch configuration (hidden/visible, menu bar icon)

2. **SwiftQuit.swift**: Core business logic
   - Window closing detection via Swindler's `WindowDestroyedEvent`
   - App termination logic with 2-second delay
   - Settings management (UserDefaults)
   - Excluded/included apps list management
   - Support for two modes: exclude specific apps OR include only specific apps

3. **ViewController.swift**: Settings UI controller
   - Manages the preferences window interface

### Key Dependencies

- **Swindler**: Window management framework for monitoring window events
- **AXSwift**: Accessibility API wrapper (required for window monitoring)
- **LaunchAtLogin**: Helper for managing launch at login functionality
- **PromiseKit**: Asynchronous programming support

### Important Implementation Details

- The app requires accessibility permissions to monitor window events
- System apps (Finder, Spotlight, NotificationCenter) are always excluded
- Uses a 2-second delay before quitting apps to handle edge cases
- Settings are persisted in UserDefaults with keys:
  - `SwiftQuitSettings`: Dictionary containing app preferences
  - `SwiftQuitExcludedApps`: Array of app bundle paths

### Build Output

Built app is located at: `.DerivedData/Swift Quit/Build/Products/Debug/Swift Quit.app`
