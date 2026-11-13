# Cruis’n World PC recompilation handoff

## Project shape

The PC effort recompiles Cruis’n World’s MIPS guest instructions into native C with **N64Recomp**. **N64ModernRuntime** supplies host-side lifecycle, guest memory, scheduling, input and platform services. **RT64** provides the rendering backend; the project’s recompiled graphics RSP emits RDP work for the renderer, while separately generated audio RSP code handles guest audio tasks. SDL2-compat and Apple’s Metal/Xcode toolchain are part of the local macOS runtime/build environment.

This document records a development state. It does not claim that the source export is complete, reproducible, legally cleared, or a finished/open-source game.

## What was verified locally

The known-good local checkpoint was `checkpoints/skip_startup_20261105/cruisn_world_recomp_known_good`, SHA-256 `ac7c8c5203adc4115f363e46b9c179fc69f25d5cb705f5a6dc5ebfc1477f50af`. In an isolated live test on November 11, it reached startup and menus; allowed vehicle and course selection; entered gameplay; responded to acceleration and steering; resumed after Pause/Continue; followed tested End Cruise and Main Menu transitions; and exited cleanly.

Braking, full audio behavior, a completed race, full Championship progression, and other modes were not verified. The executable is not included. The exact source revision, runtime modifications, generated inputs, and toolchain corresponding to that binary have not been established, and rights to distribute the binary are unresolved.

A newer experimental build, SHA-256 `c3434ebce06378590fd870bae2e041d34eb941d1bd5cda8a763680d0b58dc1c4`, has unresolved pause highlight/selection behavior and Championship End Cruise behavior. It is not evidence that those paths work reliably.

## Known limitations and experiments

- 3D frontend vehicle models appear too dark.
- Intermittent road/track seams have been observed in native gameplay, including 4:3. No cause or renderer fix is established.
- Widescreen settings sidebars and menu artwork can shift, stretch, or pop during transitions. The gameplay HUD has separate working widescreen behavior; frontend polish is incomplete.
- USA/Exotica work is research and prototype integration, not complete playable vehicle imports. A 196-face La Bomba reconstruction is partial geometry; its moving donor transform/attachment was not established.
- Trilogy soundtrack selection/control is experimental. Media rights and release behavior remain unreviewed.

## Source and generation state

The local project uses N64Recomp for guest-code translation, N64ModernRuntime for host runtime integration, RT64 for rendering, Splat and project scripts for disassembly/symbol preparation, and RSPRecomp for graphics/audio microcode. The checked-in generation notes identify a supported-ROM expectation and scripts, but several critical inputs and exact tool correspondences remain incomplete or unverified.

The guest C generator’s `recompile.sh` removes `build/recomp/functions` before regenerating it. Do not run it in a valuable worktree without first reviewing that behavior and isolating its output. It applies game-specific instrumentation to generated code. Graphics RSP currently has substantial additional instrumentation without one verified ordered recipe; the audio RSP command is not integrated into a complete project recipe. Tool binaries are not conclusively tied to their source revisions. See the private generation map and RSP notes in the owner’s staging records; they are not included in this research hub.

The local build also depends on hand-maintained ROM-specific address/split/RSP configuration, generated guest functions, generated RSP outputs, vendored runtime modifications, and external toolchains. The exact November 5 binary source set is unknown. No clean build was run for this handoff.

## What this public research hub contains

The separate PC source candidate is a 68-file, helper-only snapshot: build/safety utilities, selected notices, and explanatory documents. It does not contain the runtime, renderer, game-specific build graph, ROM-specific configs, generated guest/RSP code, or full vendor modifications, so it **cannot build the game**. Three host-side integration files (controller bindings, overlay geometry, and one focused test) are held separately in a private review subset, not in this hub or the PC public-source candidate.

The original local development tree contains substantial additional runtime and generated material. Some files contain guest addresses, game-specific tables, asset identifiers, renderer adaptations, or unresolved third-party/game-content questions. They were not copied here. No ROM, extracted asset, generated guest function, executable checkpoint, save, or private capture is included.

The known-good executable is not distributed because its exact corresponding source has not been established, generated/game-derived content and runtime modifications remain under review, and applicable dependency/source obligations are unresolved. This is a source/documentation handoff, not a player release.

## Continue the work

Start with the [project status](DEVELOPMENT_STATUS.md), [research index](RESEARCH_INDEX.md), and [roadmap](ROADMAP.md). The most useful PC next step is to establish a reviewable source boundary and a safe, output-directed regeneration procedure, including exact runtime/vendor state and both RSP outputs, before claiming reproducibility. See also the [contributor guide](CONTRIBUTING.md), [licensing scope](LICENSING_SCOPE.md), and [third-party notices](THIRD_PARTY_NOTICES.md).
