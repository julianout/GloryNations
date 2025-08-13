# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is Helios Launcher (formerly Electron Launcher), a custom Minecraft launcher built with Electron. It's designed to allow users to join modded servers without manually installing Java, Forge, or other mods. The launcher handles authentication, asset management, automatic Java validation, and self-updates.

## Development Commands

```bash
# Install dependencies
npm install

# Start the application in development mode
npm start

# Build installers for distribution
npm run dist        # Build for current platform
npm run dist:win    # Build for Windows
npm run dist:mac    # Build for macOS
npm run dist:linux  # Build for Linux

# Run linting
npm run lint
```

## Architecture Overview

### Main Process (`index.js`)
- Entry point for the Electron application
- Handles window creation, IPC communication, and Microsoft authentication flows
- Manages auto-updater functionality
- Creates the main browser window with custom frame

### Renderer Process Architecture
The renderer process is organized into several key modules:

- **Authentication** (`app/assets/js/authmanager.js`): Manages Microsoft and Mojang authentication
- **Configuration** (`app/assets/js/configmanager.js`): Handles launcher settings and user preferences
- **Distribution** (`app/assets/js/distromanager.js`): Manages server distribution configurations from remote JSON
- **Process Builder** (`app/assets/js/processbuilder.js`): Constructs and launches Minecraft with appropriate JVM arguments
- **Discord Integration** (`app/assets/js/discordwrapper.js`): Handles Discord Rich Presence
- **Language Support** (`app/assets/js/langloader.js`): Internationalization system using TOML files

### UI Scripts (`app/assets/js/scripts/`)
Each major UI view has its own script:
- `landing.js`: Main launcher interface
- `login.js`: Login interface
- `settings.js`: Settings panel
- `overlay.js`: Modal overlays
- `uibinder.js`: Binds UI elements and handles transitions

### Key Technologies
- **Electron**: Desktop application framework
- **electron-builder**: For creating installers
- **electron-updater**: Auto-update functionality
- **helios-core**: Core launcher functionality library
- **EJS**: Template engine for UI rendering
- **jQuery**: DOM manipulation in renderer process

## Code Style Guidelines

- **Indentation**: 4 spaces
- **Line endings**: Windows (CRLF)
- **Quotes**: Single quotes preferred
- **Semicolons**: No semicolons (enforced by ESLint)
- **Variables**: Use `const`/`let`, never `var`
- Scripts in `app/assets/js/scripts/` have relaxed linting rules for `no-unused-vars` and `no-undef`

## Distribution Configuration

The launcher uses a distribution.json file to configure available servers, mods, and game configurations. The remote distribution URL is configured in `app/assets/js/distromanager.js` and currently points to `https://cdn.inoxia.me/distribution.json`.

## Platform-Specific Notes

- **Windows**: Uses NSIS installer, requires code signing for production
- **macOS**: Builds both x64 and arm64 DMG installers
- **Linux**: Builds AppImage format
- The launcher automatically handles platform-specific Java installations and paths

## Debugging

For Visual Studio Code debugging, use the launch configurations provided in the README:
- "Debug Main Process": Debug Electron's main process
- "Debug Renderer Process": Debug the renderer process (requires Debugger for Chrome extension)

Console can be opened with `Ctrl+Shift+I` in the running application.