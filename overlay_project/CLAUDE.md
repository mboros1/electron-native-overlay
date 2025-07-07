# Worker CLAUDE.md - Electron Native Overlay

**Branch**: feature/[branch-name]  
**Private Branch**: private/[your-name]/[feature-name]  
**Assignment PR**: #[PR-number]  
**Started**: [Date]

## Project Context

You are working on a cross-platform native overlay library that creates transparent OpenGL windows for Electron applications. The overlay can be positioned anywhere over the Electron window, including webview content. This library will be distributed as a Node.js addon under the Apache 2.0 license.

### Key Requirements
- Per-pixel transparency with alpha channel
- Always-on-top positioning without window decorations
- Input pass-through (clicks go to underlying app)
- Electron integration for window positioning and coordination
- Hardware-accelerated OpenGL rendering
- Precompiled binaries via prebuildify

### Target Platforms
- Windows 10+ (DWM composition)
- macOS 11+ (Cocoa/NSWindow)
- Ubuntu 20.04+ (X11 with compositor)

## Feature Context

[Feature-specific guidance provided by orchestrator - will be customized per worker]

## Architecture Overview

### Core Components

**Node-API Addon Structure**:
```
overlay_addon.cpp       # Entry point, exports JS functions
overlay_manager.cpp     # Platform-agnostic overlay management
overlay_window.h        # Base class for platform implementations
```

**JavaScript API (Electron main process)**:
```javascript
const overlay = require('electron-native-overlay');
const { BrowserWindow } = require('electron');

// Create overlay for Electron window
const win = new BrowserWindow({ /* ... */ });
overlay.createOverlay(id, x, y, width, height, win.getNativeWindowHandle());

// Update position/size
overlay.updateOverlay(id, x, y, width, height);

// Destroy overlay
overlay.destroyOverlay(id);
```

### Platform Implementation Strategy

**Windows**:
- CreateWindowEx with WS_POPUP | WS_EX_LAYERED | WS_EX_TRANSPARENT
- DwmEnableBlurBehindWindow for transparency
- WGL for OpenGL context
- SetWindowPos for positioning

**macOS**:
- NSBorderlessWindowMask window
- NSFloatingWindowLevel for always-on-top
- setOpaque:NO and clear background
- NSOpenGLContext with alpha support

**Linux**:
- 32-bit ARGB visual selection
- X Shape extension for input regions
- GLX for OpenGL context
- Override-redirect or _NET_WM_STATE_ABOVE

## My Task Understanding

[Your interpretation of the assignment - update this section]

## Technical Approach

[Your planned implementation approach - update this section]

## Key Decisions

[Document important decisions as you make them]

### Example Decision Log Format:
```
Date: YYYY-MM-DD
Decision: [What was decided]
Rationale: [Why this approach]
Alternatives Considered: [Other options explored]
```

## Progress Log

[Track your daily progress and learnings]

### Example Progress Format:
```
Date: YYYY-MM-DD
- Implemented [feature/component]
- Discovered [insight/pattern]
- Blocked by [issue] - resolution: [how resolved]
```

## Challenges & Solutions

[Document problems encountered and how you solved them]

### Platform-Specific Challenges

**Windows Challenges**:
- DWM composition requirements
- Coordinate system differences
- High DPI scaling

**macOS Challenges**:
- Flipped coordinate system
- Retina display handling
- App sandbox restrictions

**Linux Challenges**:
- Compositor detection
- Visual selection complexity
- Wayland incompatibility

## Code Patterns

### Cross-Platform Abstraction
```cpp
class OverlayWindow {
public:
    virtual void Create(int x, int y, int w, int h) = 0;
    virtual void Update(int x, int y, int w, int h) = 0;
    virtual void Destroy() = 0;
    virtual void MakeCurrent() = 0;
};
```

### Node-API Pattern
```cpp
napi_value CreateOverlay(napi_env env, napi_callback_info info) {
    // Parse arguments
    // Create platform-specific window
    // Store in manager
    // Return handle
}
```

### OpenGL Setup Pattern
```cpp
void SetupOpenGL() {
    glEnable(GL_BLEND);
    glBlendFunc(GL_SRC_ALPHA, GL_ONE_MINUS_SRC_ALPHA);
    glClearColor(0.0f, 0.0f, 0.0f, 0.0f);
}
```

## Testing Strategies

### Unit Testing
- Mock platform APIs for isolated testing
- Test coordinate transformations
- Verify memory management

### Integration Testing
- Create test Electron app
- Verify overlay positioning relative to BrowserWindow
- Test input pass-through
- Check transparency rendering
- Test with various Electron window configurations

### Platform Testing
- Test on minimum supported OS versions
- Verify compositor requirements
- Check multi-monitor support
- Test high DPI scenarios

## Build System Notes

### CMake Configuration
- Platform detection logic
- Library linking requirements
- Binary output paths

### Prebuildify Setup
- Target Node versions
- Platform/architecture matrix
- Binary packaging structure

## Performance Considerations

- Minimize OpenGL state changes
- Batch rendering operations
- Efficient event handling
- Resource cleanup on destruction

## Security Considerations

- Validate all input coordinates
- Prevent overlay spoofing attacks
- Handle privileged window scenarios
- Safe memory management

## Notes for Future Work

[Anything that might help you or others on similar tasks]

### Useful Commands
```bash
# Build native addon
npm run build

# Run specific platform tests
npm run test:windows
npm run test:macos
npm run test:linux

# Generate prebuilt binaries
npm run prebuildify
```

### Debugging Tips
- Use RenderDoc for OpenGL debugging
- Platform-specific window spy tools
- Console logging from native code
- Memory leak detection tools

---

**IMPORTANT**: This file should NEVER be committed to your feature branch!
- Keep it in your working directory for Claude Code to read
- Optionally back it up to a private branch
- Add to .git/info/exclude to hide from git status