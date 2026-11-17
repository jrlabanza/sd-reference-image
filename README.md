# sd-forge-reference-image

NovelAI-style "Vibe Transfer" and "Precise Reference" for Forge Neo, integrated **inside the txt2img / img2img tab** (not a separate tab).

After Forge restart, you'll find a new accordion called **"📸 Reference Image (NovelAI-style)"** in the txt2img tab, sitting alongside ControlNet / ADetailer / Hi-res Fix / etc.

## What's new - November 2026

- **Stack up to three references.** *Stack more references (2 & 3)* adds two more reference images, each with its own mode (Off / Vibe / Precise) and strength (0-1.5) - for example a Vibe for the look plus a Precise for the face. Each extra reference costs VRAM; on 8 GB, one or two is the comfortable limit.
- **Vibe library.** Name the current reference and press **💾 Save vibe** to keep the picture with its mode and strength; pick one from the *Vibe library* list and press **📥 Load** to bring it back (🔄 refreshes the list).

## Known issue (November 2026)

*Vibe* uses `ip-adapter-plus_sdxl_vit-h`, which currently gives blown-out, saturated SDXL images in Forge Neo with any attention backend (an issue in Forge, not in this extension). *Precise* uses the plus-face model. For a usable style reference meanwhile, the ControlNet panel with **CLIP-ViT-H (IPAdapter)** and the plain `ip-adapter_sdxl_vit-h` model works. On RTX 50 cards the extension now switches Comfy-Kitchen INT8 attention to PyTorch during the render, as it does SageAttention.

## Modes

- **Vibe (style + composition)** — IPAdapter Plus (`ip-adapter-plus_sdxl_vit-h`). Transfers style, mood, lighting, colour palette from the reference.
- **Precise (face / character)** — IPAdapter Plus Face (`ip-adapter-plus-face_sdxl_vit-h`). Transfers face / character identity from the reference.
- **Off** — disabled (no reference applied).

## How

1. In txt2img, expand the **📸 Reference Image** accordion
2. Drop your reference image
3. Pick a mode
4. Set strength (defaults: 0.7 — try 0.5-0.7 for Vibe, 0.6-1.0 for Precise)
5. Click the normal Generate button

The script applies the IPAdapter directly to the UNet using Forge's existing IPAdapter machinery (no ControlNet UI fiddling, no preprocessor footguns). Infotext is stamped with `RefImg Mode / RefImg Strength / RefImg Model` so generations are reproducible.

## Why

The standard ControlNet UI requires picking the right preprocessor AND the right model AND the right radio button AND the right weight, and any combination of wrong choices produces silently broken results (black images, no influence, wrong type errors, etc.). This script hides all of that — the only choice is "Vibe vs Precise vs Off" and a single strength slider.

For more advanced setups (InstantID dual-unit face cloning, multi-reference Vibe, ControlNet Tile upscaling), keep using the standard ControlNet panel.
