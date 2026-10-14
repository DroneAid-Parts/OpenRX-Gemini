# OpenRX Gemini DroneAid rebrand: case notes

These notes record the rebrand applied to `OpenRX-Gemini` on 2026-10-14. Use this as an example; other boards may have different reference designs, naming rules and KiCad versions.

## Brand asset and documentation

- Source logo: `../DroneAid dark.svg` (relative to the `OpenRX-Gemini` directory).
- Project copy: `images/droneaid-logo.svg`. It is the supplied SVG with a rounded dark `#10131A` background rectangle added so its white paths remain visible on a light README page.
- `README.md`: added the logo banner, renamed the heading to `DroneAid OpenRX Gemini`, and updated the opening description to attribute the product to DroneAid.
- `AGENTS.md`: changed the project heading to `DroneAid OpenRX Gemini`.
- `CONTRIBUTING.md`, board/project filenames, firmware target identifiers, technical specifications, maintainer/upstream references and license were retained.

## Board artwork

- File: `hardware/OpenRX-Gemini.kicad_pcb`.
- Replaced the `OPEN` graphic text on `B.SilkS` with `DroneAid` using the pcbnew API.
- Preserved its position `(110.14, 101.54) mm`, 180° orientation, alignment and mirrored side. Reduced text size from `1.7 mm` to `1.0 mm` and stroke thickness from `0.34 mm` to `0.18 mm` to fit the longer name.
- The installed KiCad build lacked the board's `Tokyo` font. On save it substituted Noto Sans and regenerated cached outlines for board text; the PCB diff therefore includes unrelated render-cache changes that should be reviewed visually.

## Board finish

- Matched the practice board's stackup: `F.Mask` and `B.Mask` are `Blue`; `copper_finish` is `ENIG`.
- These three metadata values were directly patched only after explicit user approval because the installed pcbnew Python binding did not expose stackup color setters and the GUI was unavailable in that session. For future boards, use KiCad Board Setup → Board Stackup → Physical Stackup whenever possible.
- The KiCad 3D Viewer must have **Use board stackup colors** enabled to display the stackup colors. Its own default can otherwise remain green.

## Render status

`images/front.png` and `images/back.png` were not regenerated during this pass. They still show the old green render, and the back render still shows the old `OPEN` artwork; refresh both before using the README images as current product renders.
