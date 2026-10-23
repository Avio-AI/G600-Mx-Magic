# Mx Magic

**One app for the fleet: https://avio-ai.github.io/G600-Mx-Magic/** — tap the model name in the top bar to switch aircraft.

| Type | App code | Data (private) | Tabs |
|---|---|---|---|
| G600 | `g600.html` (this repo) | `Avio-AI/G600-Manual-Database` | Describe, Search, MEL, Wiring |
| G550 | `/G550-troubleshooter/index.html` | `Avio-AI/G550-manual-data` | Describe, Search, MEL |
| G650 | `g600.html` on a MEL-only profile | `Avio-AI/Gulfstream-MEL-Data` `g650/` | MEL |
| G700 / G800 | `g600.html` on a MEL-only profile | `Avio-AI/Gulfstream-MEL-Data` `g700/` | MEL |

`index.html` is the fleet shell: it loads the chosen aircraft's app into the page with its profile (storage prefix, data repo / folder, local PDF folder). Each type keeps its own settings and cases. `fleet/` redirects here.

The G600 app below can still be opened on its own at `/G600-Mx-Magic/g600.html`.

## G600

AI-assisted fault isolation for the Gulfstream G600 (GVII-G600), grounded in the aircraft's own maintenance data: AMM, SDM, iWDM, IPC, SWPM, FlightSafety G500/G600 MTM and the engine run checklist.

- **CAS**: autocomplete over ~560 G500/G600 CAS messages with color and trigger logic (from the FSI CAS tables).
- **Describe the squawk**: free text plus phase of flight, altitude, speed, SAT, cabin altitude / ΔP / rate, observed readings, maintenance messages / CMC history, recent maintenance and checks already done.
- **AI troubleshooting chat**: Claude searches the manuals itself (CAS logic, BM25 page search, page reads, catalog, plus iWDM component lookup, connector pin-outs and wire tracing). It builds a ranked differential, recommends the most discriminating next check with real designators, pins and wire numbers, and cites every fact. Tap any citation to read the page or open the schematic.
- **Search**: direct keyword search across all manuals, no AI or API key needed.
- **Wiring**: find any device, connector, splice or ground. Open its pin-out (cavity → wire → far-end pin; tap the far end to follow the circuit), trace a wire number end to end, and view the schematic sheets.
- Works offline after "Download all manuals" in Settings. Cases are saved on the device and can be exported as Markdown for the work order.

## Architecture

This repo holds only the apps (static `index.html` fleet shell + `g600.html`) and contains **no manual content**. The manual index lives in a separate **private** repo (`G600-Manual-Database`). The app reads it through the GitHub API with a read-only token entered on the device. AI calls go straight from the browser to `api.anthropic.com` with your own API key. There is no server.

## Setup

1. **Pages**: Settings → Pages → Deploy from branch → `main` / root. The app runs at `https://<owner>.github.io/<repo>/`.
2. **Token**: GitHub → Settings → Developer settings → Fine-grained tokens → new token. Set Repository access to *Only* `G600-Manual-Database` and Permissions to **Contents: Read-only**. One token can cover all the data repos: add `G550-manual-data` and `Gulfstream-MEL-Data`.
3. Open the app → ⚙ Settings: enter the Anthropic API key, the data repo (`Avio-AI/G600-Manual-Database`) and the token.
4. On iPhone, use Share → "Add to Home Screen" for full-screen use. Pinch zoom is disabled. Schematics have their own zoom buttons.

## Notes

- There is **no G600 Fault Isolation Manual** in this library. CAS trigger logic comes from FlightSafety training tables, and the AI builds fault isolation from SDM/FSI system logic and iWDM wiring. The AMM, SDM and current approved data govern.
- iWDM data is the R4 package for s/n 73100–73114. Verify other serials against the current iWDM.
- Keys are stored only in that browser's local storage (untick "Remember" on shared devices). Anyone with your Anthropic key can spend on your account: use a key with a spend limit.
- Decision aid only. The current revision of the approved maintenance data governs all maintenance and return-to-service actions.
