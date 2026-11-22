# sion

SiON synthesis engine with a Web Audio host and JavaScript/TypeScript entry points. [package.json](package.json) exports `sion`, `sion/sion` (engine classes) and `sion/audio-host` (browser services).

## Independent live-note session

```ts
import { createSiONSession } from "sion"

// Run from a user action that enables audio.
const context = new AudioContext()
await context.resume()
const session = await createSiONSession(context, context.destination)
console.log(session.voices)
// Use session.driver and session.presets following the engine types.
// On teardown: session.dispose(); await context.close()
```

`createSiONSession` creates an independent driver and audio route while sharing engine tables. Dispose it when finished. [SiON.js](SiON.js) implements the session; [SiON.d.ts](SiON.d.ts) describes the driver, presets, voices, audio host and disposal method.

`loadSiON(context?, destination?)` returns engine classes and an optional audio host. Passing a context installs the legacy engine's browser-audio services; omit it to load classes without that playback route. Prefer independent sessions for separate live instruments. The host owns AudioContext startup/shutdown.

[SiONAudioHost](SiONAudioHost.d.ts) handles decoded/PCM sound playback, channel updates and disposal. Engine contracts are in [sion/index.d.ts](sion/index.d.ts) and [sion/index.js](sion/index.js). Harmoneasy's [SiON output](../audiobus/io/outputs/output-sion.ts), factory ID `sion`, supplies app controls.

This package ships JavaScript directly and defines no build or test script. The parent workspace has SiON output integration tests. Repository: [SiON](https://github.com/designerzen/SiON).
