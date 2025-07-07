# Worker Onboarding - Electron Native Overlay Project

Welcome to the Electron Native Overlay project! This guide will help you get started with your assignment.

## Your Workspace

- **Working Directory**: THIS directory - always start Claude Code here!
- **Branch**: `[branch-name]`
- **Remote**: `https://github.com/mboros1/electron-native-overlay.git`

⚠️ **Critical**: See `assistant/WORKER_SETUP_GUIDE.md` (in this directory) for detailed setup instructions to ensure proper context isolation.

## Getting Started

### Step 1: Review Your Assignment
Your assignment is in the PR description at: [PR URL]

### Step 2: Set Up Your Environment
```bash
# Install Node.js dependencies
npm install

# Install native build tools (if needed)
# macOS: xcode-select --install
# Ubuntu: sudo apt-get install build-essential libx11-dev libxfixes-dev libgl1-mesa-dev
# Windows: npm install --global windows-build-tools

# Install CMake (required for building)
# macOS: brew install cmake
# Ubuntu: sudo apt-get install cmake
# Windows: choco install cmake

# Build the native addon
npm run build

# Run tests
npm test
```

### Step 3: Customize Your CLAUDE.md and Create Private Branch
You've been provided a starter CLAUDE.md. This is your knowledge base that will grow with each assignment:

1. Edit CLAUDE.md to add your task understanding and approach
2. Create your private branch to preserve your growing expertise:
   ```bash
   # Create a private branch for your workspace
   git checkout -b private/[your-name]/[feature-name]
   git add CLAUDE.md
   git commit -m "Initial workspace for [feature-name]"
   git push -u origin private/[your-name]/[feature-name]
   
   # Return to feature branch to continue work
   git checkout feature/[branch-name]
   ```
3. Add CLAUDE.md to git exclusions:
   ```bash
   echo "CLAUDE.md" >> .git/info/exclude
   ```

**CRITICAL**: 
- Never commit CLAUDE.md to the feature branch!
- Always backup to your private branch as you learn
- Your CLAUDE.md should grow richer with each assignment

### Regular Backups
As you discover patterns, solve problems, or learn about the codebase:
```bash
# Quick backup (from feature branch)
git stash push -m "temp" -- CLAUDE.md
git checkout private/[your-name]/[feature-name]
git stash pop
git commit -am "Update: [what you learned]"
git push
git checkout feature/[branch-name]
```

## Project-Specific Guidelines

### Architecture Overview
This project creates a transparent OpenGL overlay window that aligns with HTML elements in React apps. Key components:

- **Native Addon**: Node.js C++ addon using Node-API
- **Platform Layers**: Windows (Win32/DWM), macOS (Cocoa), Linux (X11)
- **OpenGL Rendering**: Hardware-accelerated drawing on transparent windows
- **Build System**: CMake for cross-platform compilation
- **Distribution**: Prebuildify for shipping precompiled binaries

### Code Organization
```
electron-native-overlay/
├── src/
│   ├── overlay_addon.cpp         # Node-API entry point
│   ├── overlay_manager.cpp       # Cross-platform manager
│   ├── windows/                  # Windows-specific implementation
│   ├── macos/                    # macOS-specific implementation
│   └── linux/                    # Linux-specific implementation
├── include/                      # Header files
├── lib/                         # JavaScript API
├── test/                        # Test files
├── CMakeLists.txt              # Build configuration
└── package.json                # NPM package definition
```

### Platform-Specific Notes

**Windows**:
- Uses DWM composition for transparency
- WS_EX_TRANSPARENT for input pass-through
- Requires Windows 10+ with compositor enabled

**macOS**:
- NSBorderlessWindowLevel for overlay
- Default click-through on transparent pixels
- Coordinate system has flipped Y-axis

**Linux**:
- X11 with compositing window manager
- X Shape extension for input handling
- Requires ARGB visual support

## Communication Protocol

### PR Comments
All communication happens through PR comments:

**Progress Updates**:
```markdown
## Progress Update - [Date]

✅ Completed:
- [What you finished]

🔧 In Progress:
- [What you're working on]

❓ Questions:
- [Any blockers or questions]
```

**Requesting Help**:
```markdown
## Need Assistance

**Issue**: [Description]
**What I've Tried**: [Your attempts]
**Context**: [Relevant information]

Could you provide guidance?
```

**Ready for Review**:
```markdown
## Ready for Review

All tasks complete and tested. Key changes:
- [Summary of changes]
- [Files modified]

Ready to merge!
```

## Development Workflow

### Best Practices
1. Check PR for new comments regularly
2. Post progress updates when reaching milestones
3. Ask questions early - don't stay blocked
4. Commit with clear, descriptive messages
5. Save context using `/compact` regularly
6. Test on your assigned platform thoroughly
7. Document any platform-specific quirks you discover

### Before Marking Complete
- [ ] All acceptance criteria met
- [ ] Tests passing on assigned platform
- [ ] Code follows project conventions
- [ ] Documentation updated
- [ ] Platform-specific edge cases handled
- [ ] CLAUDE.md reflects current state
- [ ] Final "Ready for Review" comment posted

## Important Notes

### DO NOT Commit
- This ONBOARDING.md file
- Any ASSIGNMENT.md files
- Temporary or workflow files

### DO Commit
- All code changes
- Documentation updates
- Test files
- Configuration changes
- Platform-specific implementations

## Testing Guidelines

### Unit Tests
```bash
npm test
```

### Integration Tests
```bash
# Test overlay creation and positioning
npm run test:integration

# Platform-specific tests
npm run test:windows   # Windows only
npm run test:macos     # macOS only  
npm run test:linux     # Linux only
```

### Manual Testing
1. Create test React app with target elements
2. Run overlay demo to verify alignment
3. Test input pass-through behavior
4. Verify transparency rendering
5. Check performance and resource usage

## Context Management

### Saving Context
Use the `/compact` command regularly to save your working context:
```
/compact
```

This creates a snapshot in `.claude_context/` that can be restored later.

### Restoring Context
If you need to resume work after a break, your context files help you quickly get back up to speed.

## Knowledge Continuity

### Starting Your Next Assignment
When you receive a new assignment:
1. Check if you have existing private branches with relevant context
2. Copy useful patterns and knowledge from previous CLAUDE.md files
3. Build upon your growing expertise

Your private branches form your personal knowledge base for this project.

## Communication Modes

You must follow these communication modes (check your local `assistant/` directory):

### Sharp Mode
**Use for**: All conversational interactions, discussions, and problem-solving. Provide direct feedback, critical analysis, and honest assessments. Clearly signal uncertainty levels and ask clarifying questions.

### Absolute Mode  
**Use for**: All documentation writing. Write concisely without decorative language, emotional appeals, or unnecessary transitions.

**Location**: These mode definitions are in your project repository at:
- `assistant/sharp_mode.txt`
- `assistant/absolute_mode.txt`

## Platform Resources

### Windows Development
- [DWM Composition](https://docs.microsoft.com/en-us/windows/win32/dwm/composition-ovw)
- [Layered Windows](https://docs.microsoft.com/en-us/windows/win32/winmsg/window-features)
- [WGL Documentation](https://docs.microsoft.com/en-us/windows/win32/opengl/wgl-functions)

### macOS Development
- [NSWindow Programming Guide](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
- [OpenGL Programming Guide for Mac](https://developer.apple.com/library/archive/documentation/GraphicsImaging/Conceptual/OpenGL-MacProgGuide/opengl_intro/opengl_intro.html)

### Linux Development
- [X11 Programming Manual](https://www.x.org/releases/X11R7.7/doc/libX11/libX11/libX11.html)
- [X Shape Extension](https://www.x.org/releases/X11R7.6/doc/libXext/shapelib.html)
- [GLX Documentation](https://www.khronos.org/registry/OpenGL/extensions/GLX/)

## Need Help?

Post a comment on your PR describing your issue. The orchestrator will respond and help unblock you.

---

*Remember: Clear communication and regular updates help ensure smooth collaboration!*