---
name: pcb-rebranding
description: Rebrand a KiCad PCB and its project documentation with a supplied logo, product name, or reference-board palette. Use whenever a user asks to rebrand a PCB, replace board silkscreen branding, update PCB README branding, or match another board's colors.
compatibility: Requires read access to the KiCad project, its local agent instructions, and any supplied brand/reference assets. KiCad PCB Editor or pcbnew is needed for board edits and renders.
---

# Rebrand a KiCad PCB

Use the supplied brand assets and the user's reference board to make a consistent, reviewable rebrand across the board and its documentation.

## Workflow

1. **Inspect before editing.** Read the target repository's `AGENTS.md` and README, check the worktree, find the root schematic/PCB/project files and rendered images, and inspect the supplied logo and reference board. Follow that repository's rules for KiCad edits.
2. **Establish the requested scope.** Record the new brand name, logo source, reference palette, which PCB side carries the mark, and whether existing rendered images should be refreshed. Treat an explicit request to match a reference board as approval to copy its visible physical finish where the board files define it.
3. **Keep engineering identity intact.** Change only the requested brand treatment. Preserve board/project filenames, product and firmware identifiers, part references, nets, footprints, routing, licensing, maintainer and upstream links unless the user specifically asks to change them.
4. **Update documentation.** Place a copy of the supplied logo in the target project's image assets; leave the source asset untouched. Make transparent or white logo artwork visible on the documentation background (for example, place white artwork on a dark logo background). Update the README title, opening description and logo/image alt text. Change headings in project-specific agent documentation only where the brand is shown; retain shared contribution instructions and technical references.
5. **Update PCB silkscreen with KiCad.** Find the exact text object and layer. Replace the brand while preserving its side, position, orientation and alignment; resize the text and stroke only as needed to fit and remain legible. Use the KiCad editor or `pcbnew` API, and close KiCad before scripted writes. Never text-edit `.kicad_pcb`, `.kicad_sch` or `.kicad_dru` unless the user explicitly approves that exception after repository instructions prevent the supported route.
6. **Match the reference's physical finish.** Read the reference board's Physical Stackup values instead of guessing from a render. Set the target's front and back solder-mask colors, silkscreen colors if specified, and copper finish to those values using KiCad Board Setup → Board Stackup → Physical Stackup. If the installed `pcbnew` API does not expose stackup setters, use the KiCad GUI; if the GUI is unavailable, explain the repository constraint and ask before taking another route.
7. **Review fonts and cached text.** Before saving a board with custom text, check that its fonts are available. KiCad may regenerate cached text outlines with a substitute font on save, changing many unrelated text outlines. Inspect the diff for that churn and report it; do not silently accept visual changes to unrelated labels.
8. **Refresh and inspect renders.** Regenerate existing front/back board renders after PCB text or palette changes. Open both sides in the 3D Viewer and inspect the silkscreen, clearances and legibility. Enable **Use board stackup colors** in the viewer when checking stackup colors; otherwise it can continue to display its own green default. If renders cannot be refreshed, say so clearly and identify which images are stale.
9. **Review the final diff.** Confirm that board changes are limited to the intended text and stackup metadata, documentation changes are limited to branding, and the circuit/layout did not change. Follow the repository's ERC/DRC and release rules when the user is preparing a hardware release.

## Report back

List the files changed and the exact text, logo, and stackup values applied. Mention any stale images, missing fonts, viewer setting, or other limitation that affects what the user sees. Do not claim the physical manufacturing color changed unless the fabrication order also specifies it; stackup colors describe the PCB and its KiCad rendering.

## OpenRX Gemini example

Read [the case notes](references/openrx-gemini-case-study.md) for the exact paths and edits from the first DroneAid rebrand in this repository.
