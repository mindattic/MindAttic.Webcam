---
layer: release
status: living
---
# MindAttic.Webcam — Release charter

> The one finish line this repo is working toward. Every open ticket in this repo sits under one of the epics
> below, and every epic is part of the milestone. Work that does not fit is out of scope until this milestone is
> reached (owner, 2026-11-27, MAT-T319).

## 1. What this is
Webcam eye tracking and face monitoring with no special hardware, for browser pages, Unity and Unreal through one bridge; used by MindPuck and by Gadfly's exam room.

## 2. The milestone: MindPuck real-eye verification {#milestone}
- **Milestone:** MS-1, "MindPuck real-eye verification", shared by MindAttic.Webcam, MindPuck.Web,
  MindPuck.Unity and MindPuck.Unreal.
- **Canonical version:** MindPuck.Web. Its `src/core` is the original rules; Unity and Unreal are frozen ports.
- **Published means:** MindPuck.Web served from the Webcam bridge (`npm start`, `/play/`); the Unity and Unreal
  ports as Release builds in `R:\MindAttic\Apps\<repo>` through the shared run menu; `@mindattic/webcam` with its
  licence and third-party notices.
- **The natural stopping point:** the original ask was "the important part is that a user can use their webcam
  or phone cam to repulse a puck". Every automated check is already green; what is missing is a real person
  doing it. When a real person moves the puck to the goal with their own camera in all three schemes, the
  library can be redistributed, and the ports are frozen and playable, the MindPuck family is complete.

## 3. Done when (acceptance checks)
1. signoff — a real person plays MindPuck with their own webcam or phone camera and confirms the accuracy is
   acceptable (MS-1 check 1).
2. build — all four repos' test suites green (MS-1 check 2, passed 2026-11-20).
3. exists — MindAttic.Webcam has LICENSE and THIRD-PARTY-NOTICES (MS-1 check 3).
4. exists — MindPuck.Unity and MindPuck.Unreal Release builds on `R:\MindAttic\Apps` (MS-1 check 4).

## 4. Epics, in priority order
Each epic is the 900 band of its own repo's queue.
| Priority | Epic (ticket) | What it delivers | Features |
|---|---|---|---|
| 900 | ET-T33 Webcam library compliance (MindAttic.Webcam) | Redistributable, neutral library | ET-T34 Licence and notices (990: ET-T30), ET-T35 Neutral look-away wording (980: ET-T29) |
| 900 | MP-T11 Real-eye play on the web (MindPuck.Web) | The canonical game ready for and passing the real-eye check | MP-T12 Pre-signoff smoke (990: MP-T14), MP-T13 Owner real-eye check (980: MP-T15, after MP-T14) |
| 900 | MPU-T11 Unity port frozen (MindPuck.Unity) | Playable from the run menu, no new features | MPU-T12 Run-menu deploy (990: MPU-T10), MPU-T13 Freeze notice (980: MPU-T14) |
| 900 | MIN-T9 Unreal port frozen (MindPuck.Unreal) | Playable from the run menu, no new features | MIN-T10 Run-menu deploy (990: MIN-T8), MIN-T11 Freeze notice (980: MIN-T12) |

MPU-T10 and MIN-T8 also stay part of the Agency deploy containers MAT-T291 and MAT-T292.

## 5. Out of scope (not before this milestone)
- Gadfly-driven Webcam extensions: worker landmarker, targetFps, frame timing (ET-T18, ET-T21..T23),
  blendshapes and iris confidence (ET-T19, ET-T24, ET-T25), gazePoint, 5-dot calibration and face light
  (ET-T20, ET-T26..T28). Gadfly has working local versions; refile against Webcam after MS-1 if Gadfly wants
  them moved into the library.
- New control schemes or game modes in MindPuck; any feature in the Unity or Unreal ports.
- A WebGL Unity build; app-store publishing.
- Open-ended bughunts (ET-T32, MP-T10, MPU-T8, MIN-T6).

## 6. Current verified state
- Webcam: library, bridge, tracker page, gaze lab, Unity and Unreal clients (ET-T2..T7); webcam core and
  presence, head-pose and integrity modules (ET-T13..T16); 49/49 tests at `e12c3f7`.
- MindPuck.Web: three schemes with solver-proven levels and steer anchoring (MP-T2..T7); 37/37 tests at `91f140d`.
- MindPuck.Unity: port at parity (MPU-T2..T5); 44/44 tests at `7a28547`.
- MindPuck.Unreal: port at parity, pars match the web for seed 12345 (MIN-T2..T5); 5/5 automation tests.
- Never yet played by a real person with a real camera.

## 7. Decision record
- 2026-11-27 — MS-1 extended (goal edited) rather than duplicated; MindPuck.Web named canonical. Legion vote on
  the shared charter and wont-do list: **Approve**, 9 of 9 voters, plurality quorum reached, consensus 1.0; no
  dissent.
- Triage (MAA-T24), this repo: 14 open → done 0, duplicate 0, wont-do 12 (the three Gadfly extension stories with their 8 tasks, and the bughunt), kept 2; no new tasks.
