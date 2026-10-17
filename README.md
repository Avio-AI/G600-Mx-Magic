# G600 Mx Magic

AI-assisted fault isolation for the Gulfstream G600 (GVII-G600), grounded in the aircraft's own maintenance data: AMM, SDM, iWDM, IPC, SWPM, FlightSafety G500/G600 MTM and the engine run checklist.

- **CAS**: autocomplete over ~560 G500/G600 CAS messages with color and trigger logic (from the FSI CAS tables).
- **Describe the squawk**: free text plus phase of flight, altitude, speed, SAT, cabin altitude / ΔP / rate, observed readings, maintenance messages / CMC history, recent maintenance and checks already done.
- **AI troubleshooting chat**: Claude searches the manuals itself (CAS logic, BM25 page search, page reads, catalog, plus iWDM component lookup, connector pin-outs and wire tracing). It builds a ranked differential, recommends the most discriminating next check with real designators, pins and wire numbers, and cites every fact. Tap any citation to read the page or open the schematic.
- **Search**: direct keyword search across all manuals, no AI or API key needed.
- **Wiring**: find any device, connector, splice or ground. Open its pin-out (cavity → wire → far-end pin; tap the far end to follow the circuit), trace a wire number end to end, and view the schematic sheets.
- Works offline after "Download all manuals" in Settings. Cases are saved on the device and can be exported as Markdown for the work order.

## Architecture

This repo holds only the app (one static `index.html`) and contains **no manual content**. The manual index lives in a separate **private** repo (`G600-Manual-Database`). The app reads it through the GitHub API with a read-only token entered on the device. AI calls go straight from the browser to `api.anthropic.com` with your own API key. There is no server.

## Setup

1. **Pages**: Settings → Pages → Deploy from branch → `main` / root. The app runs at `https://<owner>.github.io/<repo>/`.
2. **Token**: GitHub → Settings → Developer settings → Fine-grained tokens → new token. Set Repository access to *Only* `G600-Manual-Database` and Permissions to **Contents: Read-only**. (One token can also cover `G550-Manual-Data` if you want to use the same token for both apps.)
3. Open the app → ⚙ Settings: enter the Anthropic API key, the data repo (`Avio-AI/G600-Manual-Database`) and the token.
4. On iPhone, use Share → "Add to Home Screen" for full-screen use. Pinch zoom is disabled. Schematics have their own zoom buttons.

## Notes

- There is **no G600 Fault Isolation Manual** in this library. CAS trigger logic comes from FlightSafety training tables, and the AI builds fault isolation from SDM/FSI system logic and iWDM wiring. The AMM, SDM and current approved data govern.
- iWDM data is the R4 package for s/n 73100–73114. Verify other serials against the current iWDM.
- Keys are stored only in that browser's local storage (untick "Remember" on shared devices). Anyone with your Anthropic key can spend on your account: use a key with a spend limit.
- Decision aid only. The current revision of the approved maintenance data governs all maintenance and return-to-service actions.
