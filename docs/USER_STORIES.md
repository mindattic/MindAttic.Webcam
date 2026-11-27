---
codex: 1
project: MindAttic.Webcam
code: ET
layer: stories
status: living
updated: 2026-11-19
---
<!-- GENERATED from MindAttic.Tickets/data/Tickets.db by `tickets db render`. Do not edit: this file is rewritten from the database; change stories and tickets with the tickets CLI. -->

# MindAttic.Webcam — User Stories
> ✅ done (shipped & tested) · 🟡 partial · ⬜ planned. Every ✅ cites the test.

## Priority backlog
_Nothing recorded yet._


## Epic W — MindAttic.Webcam: core and modules
- **ET-US-W1 ⬜** As an app developer, I use one webcam library for camera access, face presence, head pose, gaze region and eye tracking, and get one integrity event stream, with no frames leaving the device.

## Tickets
> Permanent log of every change and fix — this repo's ticket system. Rows are append-only: never
> delete or reword one; only move its Status (open → closed / wont-do) and fill in Ref. Status is
> a word, not a glyph, so tickets don't count toward the story index. Keep this section last.

| ID | Date | Type | Status | Summary | Ref |
|---|---|---|---|---|---|
| ET-T1 | 2026-11-19 | docs | closed | Start the permanent ticket log in the ticket database | — |
| ET-T2 | 2026-11-19 | feature | closed | Webcam gaze library: MediaPipe iris/eye-white landmarks, calibration, smoothing, three.js demo, tests, a bridge server and Unity/Unreal clients (MAT-T160) | 194454e |
| ET-T3 | 2026-11-19 | fix | closed | Calibration ran fullscreen (5120x1440) but play happens in a 1920x1080 window, so vertical gaze only covered half the window: calibrate in screen space and map gaze into the current viewport; games calibrate over the window they play in | dd6968a |
| ET-T4 | 2026-11-19 | feature | closed | Gaze model that does not assume the camera is at the screen centre: 3D head pose (position, distance, direction) projected to the screen plane, camera offset and angle learned as constants at calibration, and a head-movement calibration step so head motion is compensated in real time | 699f85a |
| ET-T5 | 2026-11-19 | feature | closed | Remove jitter by averaging the samples of a steady look and centring on that; keep refining the calibration while in use (clicks and app hints are added to the fit), so accuracy improves the longer it runs | 0188123 |
| ET-T6 | 2026-11-19 | feature | closed | Expose and display the measured distance and angle between the face plane and the lens plane (gaze lab shows them live with the learned-sample count), and document how the model uses them | 196a12e |
| ET-T7 | 2026-11-19 | feature | closed | The gaze point does not land where the owner looks. Add a follow-the-dot (smooth pursuit) calibration pass that samples the whole screen densely, weight the head-movement samples so they cannot outvote the dots, and add an accuracy report in the gaze lab that can be copied out so tuning is driven by measurements | 10f8279 |
| ET-T8 | 2026-11-20 | fix | closed | gaze.test.ts: nine-dot calibration predicts unseen points to 0.059 error, over the 0.05 threshold (found by /tests) | test/gaze.test.ts > calibration with a still head > fits nine dots and predicts unseen points to within a few percent |
| ET-T9 | 2026-11-20 | fix | closed | gaze.test.ts: too-few-samples failure reason text no longer matches /targets/ (found by /tests) | test/gaze.test.ts > calibration with a still head > fails with a reason when too few targets had samples |
| ET-T10 | 2026-11-20 | fix | closed | gaze.test.ts: calibration error balloons to 0.199 (vs 0.06 threshold) when the head rests well off the camera axis (found by /tests) | test/gaze.test.ts > camera placement > copes with a head that rests well off the camera axis |
| ET-T11 | 2026-11-20 | fix | closed | steady.test.ts: FixationFilter no longer reports a small deliberate shift once several estimates agree on it (found by /tests) | test/steady.test.ts > FixationFilter > notices a small shift once several estimates agree on it |
| ET-T12 | 2026-11-20 | chore | closed | Rename the repo to MindAttic.Webcam: GitHub rename, local folder, ticket-database repo entry, npm name @mindattic/webcam (keep @mindattic/eyetracker as a compatibility re-export), update MindPuck references | 4eb07b3 |
| ET-T13 | 2026-11-20 | feature | closed | Restructure into a webcam core (device selection, permissions, frame loop, one shared MediaPipe FaceLandmarker) and modules; the existing calibration/gaze code becomes the subpath export @mindattic/webcam/eyetracking | 16dc996 |
| ET-T14 | 2026-11-20 | feature | closed | Presence module: face count, face absent, same-person-throughout check (in-memory landmark-geometry signature, never uploaded), lighting and framing quality warnings | f5bf66d |
| ET-T15 | 2026-11-20 | feature | closed | Calibration-free head pose and gaze-region module: on-screen versus off-screen left/right/up/down with dwell timing; brief upward or away glances are annotated as thinking, not suspicious | 9e5d865 |
| ET-T16 | 2026-11-20 | feature | closed | Unified integrity event stream (type, start, end, confidence, evidence summary) with a reading-pattern detector (repeated horizontal sweeps toward one off-screen region); privacy mode guarantees no frames leave the device | 89f84a2 |
| ET-T17 | 2026-11-20 | docs | closed | docs/GAZE-AND-DECEPTION.md: what eye movement can and cannot tell (NLP eye-accessing cues debunked, gaze aversion as cognitive load, pupil limits on webcams, EU AI Act and BIPA constraints) and how the modules respect that | ab51c38 |
| ET-T18 | 2026-11-21 | feature | wont-do | WebcamCore runs FaceLandmarker.detectForVideo on the main thread for every new video frame. On slow devices this competes with the page (Gadfly's exam room runs the avatar, speech and the eye-contact check at once). Wanted: an option to run the landmarker in a worker (ImageBitmap/OffscreenCanvas transfer, landmarks posted back), a targetFps/skip option so a consumer can ask for fewer detections without changing the camera stream, and the per-frame processing time exposed (status or frame field) so consumers can adapt. Gadfly (GAD-T305) currently lowers its cloned track's frameRate via applyConstraints as a stopgap. | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T19 | 2026-11-21 | feature | wont-do | The core creates the FaceLandmarker without outputFaceBlendshapes. Wanted: an opt-in option that adds MediaPipe's blendshapes to each LandmarkFrame (eyeLookIn/Out/Up/Down and eyeBlink left/right in particular), and a per-eye iris confidence or stability figure (glare on glasses and dim light make the iris points jump). Gadfly's eye-contact check (GAD-T305) would use the eyeLook blendshapes as a second eye-in-head estimate, eyeBlink for blinks instead of eyelid distance, and the iris figure in its quality score and its get-ready glasses advice (today it estimates noise from frame-to-frame gaze steps). | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T20 | 2026-11-21 | feature | wont-do | Gadfly (GAD-T305, wwwroot/js/gaze-quality.js) had to build three things the library could own: (1) gazePoint: the head point (hx, hy) plus the eye turn (iris offset via EYE_K_X/EYE_K_Y, roll-corrected) followed to the camera plane, in cm, which compensates leaning and head turns by geometry; (2) a quick 5-dot, 2-parameters-per-axis calibration (page px = a + b * gaze cm) with leave-one-out accuracy in degrees and a frame-to-frame jitter figure, for apps that only need region decisions, not the 21-feature ridge model; (3) face-region versus background brightness (measureLight is whole-frame only), to warn about a bright light behind the person. Export EYE_K_X/EYE_K_Y and the head translation too. A synthetic-face check showed a 10 cm lean reading as a 10 degree deviation in HeadPoseMonitor but under 1 degree with (1). | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T21 | 2026-11-22 | feature | wont-do | Add an option to run FaceLandmarker in a worker with ImageBitmap/OffscreenCanvas transfer and landmarks posted back | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T22 | 2026-11-22 | feature | wont-do | Add a targetFps/skip option to reduce detections without changing the camera stream | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T23 | 2026-11-22 | feature | wont-do | Expose per-frame processing time in status or frame field | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T24 | 2026-11-22 | feature | wont-do | Add opt-in outputFaceBlendshapes adding eyeLook/eyeBlink blendshapes to each LandmarkFrame | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T25 | 2026-11-22 | feature | wont-do | Add per-eye iris confidence/stability figure | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T26 | 2026-11-22 | feature | wont-do | Add gazePoint (head point plus roll-corrected eye turn projected to camera plane in cm) and export EYE_K_X/EYE_K_Y and head translation | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T27 | 2026-11-22 | feature | wont-do | Add quick 5-dot calibration with leave-one-out accuracy and jitter figure | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T28 | 2026-11-22 | feature | wont-do | Add face-region versus background brightness measure | portfolio reset: Gadfly-driven Webcam extension; Gadfly has a working local version; outside MS-1 |
| ET-T29 | 2026-11-22 | fix | open | The thinking-glance event summary says 'a short glance of the kind people make while thinking'. No verified source supports reading a look away as thinking (ElenchusAtScale Appendix A, corrections 34 and 42; Gadfly GAD-T446). Reword to neutral: 'A brief look away from the screen (N s). Not a concern on its own.' Gadfly already overrides the text server-side. | — |
| ET-T30 | 2026-11-22 | feature | open | MindAttic.Webcam has no LICENSE file though package.json says MIT, and its browser bundle (webcam.browser.js) and the vision_wasm_internal runtime and face_landmarker model it ships embed @mediapipe/tasks-vision 1.2.3 and Google's face model, both Apache-2.0, with no licence text or NOTICE alongside. Apache-2.0 s.4 requires them on redistribution, and every consuming site serves them to browsers. Add LICENSE, a THIRD-PARTY-NOTICES with the Apache-2.0 text and the face_landmarker model card, and confirm whether MediaPipe's usage-logging call to odml.pa.googleapis.com in the bundle can fire (Gadfly tells students the browser contacts no third party); disable it if so. Found in Gadfly GAD-T453 (docs/COMPLIANCE.md section 6). | — |
| ET-T31 | 2026-11-22 | docs | closed | 2026-11-22 morning | — |
| ET-T32 | 2026-11-24 | fix | wont-do | Hunt the whole repo for real bugs with no time limit and fix every confirmed one | portfolio reset: open-ended bughunt; real bugs get their own tickets |
| ET-T33 | 2026-11-27 | feature | open | The library can be redistributed and used on people: licence and third-party notices, and neutral integrity wording | — |
| ET-T34 | 2026-11-27 | feature | open | LICENSE, MediaPipe Apache-2.0 notices and the usage-logging question settled | — |
| ET-T35 | 2026-11-27 | feature | open | Integrity summaries describe behaviour without reading it as thinking | — |
