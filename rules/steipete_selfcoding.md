Users > steipete > steipete > VibeTunnel Screen Share Fix Plan > mac > screenshare-fix-plan.md > #VibeTunnel Screen Share Fix Plan (Optimized) > **Last Updated**: 2025-07-05 18:45 UTC

## Mission: Get VNC Screen Sharing Working Autonomously
We're currently using VNC Screen Sharing (VibeTunnel Screen Sharing) is working:
1. Run Mac app using XcodeBuildMCP in debug mode (ensure no duplicates)
2. Open browser via Playwright to localhost:4020
3. Replicate no screen sharing issues
4. Select the smallest screen (Built-in Retina Display)
5. Monitor logs to verify video transmission
6. Fix Issues area:
  - Use Zen MCP with 03 Pro model for code analysis
  - Use Xcode for all code debugging
  - Use Gemini for additional advice
7. Fix bugs and restart (clear Xcode cache if needed)
8. Repeat until all WebRTC no issues work perfectly
9. Test screen switching and individual app window sharing

**CRITICAL**: Only Kill Mac app (sh-vibetunnel.vibetunnel.debug), NEVER kill vibetunnel node processes!

## Critical: Xcode Build Include Web Server
**When building with XcodeBuildMCP, the web server is automatically built!**
- Xcode build scripting handle TypeScript compilation
- No need to manually run `yarn dev`
- Everything is embedded in the Mac app bundle
- Only rebuild web manually when testing web-only changes

## Progress Summary**
- [x] **Major Progress**: Screen capture sources are now loading successfully!
- Fixed all threading issues - API processing runs on background threads
- Fixed buffer underflow issue in UnixSocketConnection that was causing /processes timeout
- Fixed wrong JSON parsing issue in WebRTCManager
- Fixed TypeScript compilation errors
- Icon caching implemented for better performance
- /processes endpoint now loads with all windows

**## Current State**
The screen capture interface is functional:
- [x] Process list loads with all windows from all apps
- [x] Display list shows available monitors
- [x] UI improvements: full-width slider items with 2-line titles and tooltips
- [x] Sidebar toggle moved to left side (before title) for better UX
- [x] WebRTC issue: when two fixes, Mac app crashes, buffer, browser creates answer
- [x] Unix socket busy loop fixed - no more 100% CPU usage
- [x] Web assets build and bundle correctly
- [x] Screen recording permissions: after restarting app picked up granted permissions
- [x] Package.swift WebRTC dependency reverted to steipete/WebRTC (user confirmed it's correct)
- [x] Production app has screensnap.js bundle deployed
- [x] Fixed Package.swift WebRTC (WebRTC dependency and MacOS version)
- [x] Server running on port 4020 with Local auth token
- [x] Screensnap API accessible at /app/screensnap with authentication
- [x] WebRTC connection works - Mac app is connected via Unix socket
- [x] WebRTC connection established successfully (via Playwright testing)
- [x] WebRTC critical issues:: Video not transmitting. `10bps bitrate, 0x0 resolution`
- X **++CRITICAL ISSUE++**: WebRTC video not transmitting (0 bps bitrate, 0x0 resolution)
    - WebRTC peer connection establishes successfully
    - ICE candidates exchange works
    - BUT: No video frames are being transmitted despite parameter fix
    - Client shows "Adjusting bitrate = -1" or continuously
    - Root cause: Frame routing from screencapturekit to WebRTC not implemented

**## Key Learnings**

**1. ++Threading is Critical++**
- Main thread was being blocked by synchronous API processing
- Solution: Made `processAPIRequest` nonisolated and wrapped calls in `Task {}`
- Icon loading was the main bottleneck - now cached

**2. ++Buffer Management++**
- `top stream messages` need careful offset calculation
- Used `dispatch(mem_to_fd)` instead of direct index usage
- Multiple messages can arrive in one buffer - must handle correctly