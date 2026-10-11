# SnipScratch

**Capture fast. Make your point. Keep moving.**

SnipScratch is an **S3D Labs** project with an open-source vision: a Windows-first utility for fast screen capture, quick annotation, and smooth screen recording. The goal is to make capturing and explaining what is on your screen feel immediate, with a small floating toolbar and dependable hotkeys close at hand.

> **Status: planning and architecture exploration.** This repository currently contains the project README only. There is no application, installer, or working integration yet. Features below are goals, not claims of shipped functionality. An open-source license will be selected before implementation is released.

## The experience we want

1. Invoke a hotkey or the floating toolbar.
2. Capture a region, window, or entire screen.
3. Add an arrow, a short note, a highlight, or an opaque redaction.
4. Copy the result and paste it where the conversation is happening.

Saving to a file should be just as easy, but the everyday path is **clipboard first**. SnipScratch should stay responsive and unobtrusive, with useful defaults and minimal friction.

### Planned capabilities

- **Fast access:** configurable global hotkeys, a compact floating toolbar, and a tray entry point.
- **Flexible capture:** region, window, fullscreen, and scrolling screenshots; multi-monitor and mixed-DPI handling.
- **Quick annotation:** arrows, text, shapes, highlights, crop, undo/redo, and flattened opaque redaction on exported images.
- **Simple output:** immediate clipboard copy, local image export, and a clear preview before sharing.
- **Smooth recording:** region/window/display recording, configurable frame rate, hardware encoding where available, and optional microphone/system audio.
- **Lightweight interaction:** low capture latency, efficient idle behavior, keyboard accessibility, and recording work kept off the interface thread.

Recording research will start with reliable 60 fps on suitable hardware and evaluate higher frame rates. Actual performance will depend on resolution, GPU, encoder, and capture source; benchmarks will guide supported targets.

## Proposed technical approach

The starting proposal is **C# and .NET with WPF**, using an MVVM interface and separate services for capture, annotation, recording, and output. The framework and supported Windows versions remain subject to prototype results.

| Area | Proposed responsibility |
| --- | --- |
| Desktop shell | WPF toolbar, tray, settings, hotkeys, and accessible controls |
| Capture service | Windows capture APIs, selection overlays, monitor/DPI coordinates, and clipboard delivery |
| Annotation model | Editable shapes/text, undo history, rendering, and final export |
| Recording backend | Capture/encode lifecycle, frame timing, audio synchronization, and native resource management |
| Output service | Local files, clipboard, and explicit user-controlled export |

### Reuse and integration research

**ShareX:** investigate selective reuse of screenshot and editor components instead of importing the entire application. Review the capture library, editor boundaries, UI dependencies, and maintenance costs at a pinned revision. The modern [ShareX ImageEditor](https://github.com/ShareX/ShareX.ImageEditor) uses Avalonia, so it is not assumed to be a drop-in WPF control; an adapter, separate editor window, or original implementation may be preferable.

**OBS Studio / libobs:** evaluate libobs as a recording engine behind a narrow native interoperability layer. Prototype initialization, capture modules, encoder discovery, frame pacing, audio, teardown, and packaging before choosing a backend. The [official frontend documentation](https://docs.obsproject.com/frontends) describes building applications around libobs. SnipScratch's interface would remain focused on quick capture and recording.

**Krita:** draw inspiration from its creative workflow and consider optional interoperability through image export or opening a capture in a separately installed Krita. Embedding the full Krita application is outside the proposed scope. Any future copied functionality or plugin work requires its own technical and license review.

## Acknowledgments and upstream licenses

Thank you to the creators, maintainers, and contributors whose work makes these tools possible:

| Project and creators | Official links | Upstream license |
| --- | --- | --- |
| **ShareX Team and contributors** | [Website](https://getsharex.com/) · [Source](https://github.com/ShareX/ShareX) | [GNU GPLv3](https://github.com/ShareX/ShareX/blob/master/LICENSE.txt); confirm notices for each selected component |
| **OBS Project, original creator Lain Bailey, and contributors** | [Website](https://obsproject.com/) · [Source](https://github.com/obsproject/obs-studio) | GNU GPLv2 or later; see [COPYING](https://github.com/obsproject/obs-studio/blob/master/COPYING) and the [libobs header notice](https://github.com/obsproject/obs-studio/blob/master/libobs/obs.h) |
| **Krita developers and contributors, supported by the Krita Foundation** | [Website](https://krita.org/) · [Source](https://invent.kde.org/graphics/krita) | [GNU GPLv3 for Krita as a whole](https://krita.org/en/about/license/); individual parts may use more permissive licenses |

**These acknowledgments recognize inspiration and candidates for investigation. They do not claim that any upstream code has already been incorporated, or that these projects endorse or are affiliated with SnipScratch.** Their names and trademarks belong to their respective owners.

### SnipScratch licensing: decision pending

No final SnipScratch license has been chosen, and this repository deliberately has no `LICENSE` file yet. Public visibility and an open-source intention do not themselves grant a general license to reuse this repository's material.

The initial compatibility review identifies **GPLv3 as a possible path**, not a decision: OBS's GPLv2-or-later grant permits use under GPLv3 alongside GPLv3 components. GPLv2-only code would require separate scrutiny and cannot simply be treated as GPLv3-compatible.

Copying GPL code or linking libobs into a distributed combined application generally brings GPL obligations for that work, including corresponding source and required notices. A native wrapper or dynamic linking does not automatically remove those obligations. Credit alone is insufficient. Inspiration or ordinary file exchange with a separately installed program is different from incorporating its code, but the actual integration must still be reviewed.

Before importing code or distributing binaries, maintainers will pin revisions, inspect file-level grants and dependency licenses, record provenance and modifications, and choose compatible distribution terms. Plugins, codecs, bundled libraries, and assets need their own review. The chosen license and third-party notices will be added before implementation releases.

## Roadmap

- [ ] **Foundation:** settle initial scope, supported Windows versions, dependency inventory, license choice, and architecture decisions.
- [ ] **Capture prototype:** hotkeys, toolbar, region/window/fullscreen capture, clipboard output, and mixed-DPI validation.
- [ ] **Annotation MVP:** arrows, text, highlights, crop, undo/redo, opaque redaction, and image export.
- [ ] **Recording spike:** compare backend options; measure 60 fps stability, higher-frame-rate potential, dropped frames, audio sync, and resource usage.
- [ ] **Expanded capture:** scrolling capture, edge-case handling, and workflow refinements.
- [ ] **First usable release:** packaging, accessibility, reliability testing, performance results, documentation, and complete license notices.

Order may change as prototypes uncover tradeoffs. There are no release dates yet.

## Contributing

Early contributions are welcome through GitHub issues: workflow examples, interface ideas, architecture proposals, performance test scenarios, and component/license research are especially useful now.

For substantial changes, open an issue first so scope can be agreed upon. Keep proposals focused and distinguish experiments from supported behavior. For future code contributions, identify upstream sources and exact licenses, preserve notices, and discuss dependency additions before copying or importing code. Implementation contribution terms will be documented after the project license is chosen.

When reporting capture or recording problems once prototypes exist, include Windows version, monitor scaling, capture mode, resolution, frame rate, GPU/encoder, reproduction steps, and expected behavior. Use sanitized examples without private screen content.

There are currently no build or installation instructions because implementation has not started.
