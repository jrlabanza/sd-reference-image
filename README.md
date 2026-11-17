# sd-forge-reference-image

NovelAI-style "Vibe Transfer" and "Precise Reference" for Forge Neo, integrated **inside the txt2img / img2img tab** (not a separate tab).

After Forge restart, you'll find a new accordion called **"📸 Reference Image (NovelAI-style)"** in the txt2img tab, sitting alongside ControlNet / ADetailer / Hi-res Fix / etc.

## What's new - November 2026

- **Stack up to three references.** *Stack more references (2 & 3)* adds two more reference images, each with its own mode (Off / Vibe / Precise) and strength (0-1.5) - for example a Vibe for the look plus a Precise for the face. Each extra reference costs VRAM; on 8 GB, one or two is the comfortable limit.
- **Vibe library.** Name the current reference and press **💾 Save vibe** to keep the picture with its mode and strength; pick one from the *Vibe library* list and press **📥 Load** to bring it back (🔄 refreshes the list).

## Strength on SDXL fine-tunes (November 2026)

IPAdapter Plus acts about **three times as strongly on SDXL fine-tunes** (Illustrious, Pony, NoobAI) as on base SDXL. At the old default of 0.7, *Vibe* blew out to a saturated, over-bright picture, which looked like a broken model. Measured on waiIllustrious: *Vibe* is clean and follows the reference at **0.25** (0.35 and up blow out), *Precise* carries the face at **0.5** (0.7 turns greyish). Picking a mode now sets that strength, and the sliders start at 0.25; move them as you like. A vibe saved earlier keeps its saved strength, so lower it if it comes out burnt. On RTX 50 cards the Comfy-Kitchen INT8 attention is swapped to PyTorch during the render, as SageAttention is.

## Modes

- **Vibe (style + composition)** — IPAdapter Plus (`ip-adapter-plus_sdxl_vit-h`). Transfers style, mood, lighting, colour palette from the reference.
- **Precise (face / character)** — IPAdapter Plus Face (`ip-adapter-plus-face_sdxl_vit-h`). Transfers face / character identity from the reference.
- **Off** — disabled (no reference applied).

## How

1. In txt2img, expand the **📸 Reference Image** accordion
2. Drop your reference image
3. Pick a mode
4. Set strength - picking a mode sets its usual value (Vibe 0.25, Precise 0.5 on SDXL fine-tunes; base SDXL takes more)
5. Click the normal Generate button

The script applies the IPAdapter directly to the UNet using Forge's existing IPAdapter machinery (no ControlNet UI fiddling, no preprocessor footguns). Infotext is stamped with `RefImg Mode / RefImg Strength / RefImg Model` so generations are reproducible.

## Why

The standard ControlNet UI requires picking the right preprocessor AND the right model AND the right radio button AND the right weight, and any combination of wrong choices produces silently broken results (black images, no influence, wrong type errors, etc.). This script hides all of that — the only choice is "Vibe vs Precise vs Off" and a single strength slider.

For more advanced setups (InstantID dual-unit face cloning, multi-reference Vibe, ControlNet Tile upscaling), keep using the standard ControlNet panel.
