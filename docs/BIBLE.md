---
layer: bible
status: living
---
# MindAttic.Webcam — Project Bible

> Single source of truth for what MindAttic.Webcam IS, is NOT, and the rules that keep it coherent.
> README says how to build and run it; this says how to think about it. The finish line lives in
> `docs/RELEASE.md`. Formerly MindAttic.EyeTracker (ET-T12).

## 1. The one sentence {#ET-§1}
Webcam eye tracking and face monitoring with no special hardware: an ordinary webcam or a phone's front
camera tells an app where on the screen a person is looking, and whether they are present.

## 2. The product promise {#ET-§2}
- A nine-dot calibration (plus follow-the-dot and head-movement steps) gives a gaze point about thirty
  times a second, good enough to tell which region of the screen has attention.
- The same gaze reaches a browser page, a Unity game or an Unreal game through one bridge protocol.
- One camera and one face model feed every module (eye tracking, presence, head pose, integrity).
- Video never leaves the device that owns the camera; only gaze points and events are sent.

## 3. What it is NOT {#ET-§3}
- Not a laboratory eye tracker: it cannot tell which word is being read.
- Not an emotion or deception detector. Read `docs/GAZE-AND-DECEPTION.md` before changing any module.
- Not a video recorder or a cloud service: the bridge relays on the local network only.
- Not a game: MindPuck (MindPuck.Web) is the first consumer, Gadfly's exam room the second.

## 4. Architecture canon {#ET-§4}
```
 camera device                       bridge (bridge/server.mjs)          display
 tracker page / EyeTracker  --gaze-->  ws://localhost:8787/gaze  -->  browser page | Unity package | Unreal plugin
 WebcamCore + MediaPipe FaceLandmarker       (https 8788 for phones)       draws calibration targets
```
### 4.1 Modules
| Path / import | Role |
|---|---|
| `src/core.ts` (`@mindattic/webcam/core`) | Platform-neutral types, math and monitor logic (no camera, no DOM) |
| `@mindattic/webcam` | `WebcamCore`: device, permission, frame loop, shared landmarker |
| `/eyetracking` | `EyeTracker`, `runCalibration`, `RemoteGazeSource`, `MouseGazeSource` |
| `/presence`, `/headpose`, `/integrity` | Presence, calibration-free head region, merged integrity event stream |
| `bridge/`, `tracker/`, `demo/` | Relay server, camera page, gaze lab |
| `unity/com.mindattic.eyetracker`, `unreal/MindAtticEyeTracker` | Engine clients (names kept from before the rename) |
| `compat/eyetracker` | `@mindattic/eyetracker` re-export |

### 4.2 Domain model
GazeSample (x, y in 0..1, y down; confidence), LandmarkFrame, calibration (ridge fit in screen space),
integrity event `{type, start, end, confidence, summary}`.

### 4.3 Key services
`EyeTracker.start`, `runCalibration`, `PresenceMonitor`, `HeadPoseMonitor`, `IntegrityMonitor.sink`,
the bridge relay (`docs/PROTOCOL.md`).

## 5. The Laws {#ET-§5}
Inherits `MindAttic.HouseRules.md` by reference.
1. **No frames leave the device.** Only gaze points and events cross the bridge or reach a host. {#ET-LAW-1}
2. **Report behaviour, never infer emotion or deception.** Summaries stay neutral and are for a human
   reviewer. {#ET-LAW-2}
3. **One wire protocol** (`docs/PROTOCOL.md`) for every client; engine clients change in this repo. {#ET-LAW-3}
4. **Tests need no camera.** The pipeline is driven by a synthetic face. {#ET-LAW-4}
5. **Third-party terms travel with the code.** MediaPipe and the face model are Apache-2.0. {#ET-LAW-5}

## 6. Verified state {#ET-§6}
- Library, bridge, tracker page, gaze lab and tests (ET-T2, `6061054`); Unity and Unreal clients (ET-T2, ET-T7).
- Head-geometry gaze model and follow-the-dot calibration (ET-T4, ET-T7).
- Webcam core and presence, head-pose and integrity modules (ET-T13..T16).
- Test suites green across Webcam and the three MindPuck repos on 2026-11-20 (MS-1 check 2).
- Never yet confirmed by a real person with a real camera in a game (MS-1 signoff).

## 7. Active frontier {#ET-§7}
`docs/RELEASE.md`: shared milestone MS-1 "MindPuck real-eye verification".

## 8. Quality bar {#ET-§8}
`npm run build` is clean, behaviour is covered with the synthetic face, PROTOCOL/README document any
wire or API change, and every client that speaks the protocol still matches it.

## 9. Glossary {#ET-§9}
- **Bridge**: the local Node server relaying gaze from trackers to displays.
- **Tracker**: the page that owns a camera. **Display**: a game or page that shows targets and reads gaze.
- **Gaze lab**: the demo page with the accuracy check.
- **One Euro filter**: the smoothing filter (Casiez, Roussel, Vogel).
