# Unity Essentials

This module is part of the Unity Essentials ecosystem and follows the same lightweight, editor-first approach.
Unity Essentials is a lightweight, modular set of editor utilities and helpers that streamline Unity development. It focuses on clean, dependency-free tools that work well together.

All utilities are under the `UnityEssentials` namespace.

```csharp
using UnityEssentials;
```

## Installation

Install the Unity Essentials entry package via Unity's Package Manager, then install modules from the Tools menu.

- Add the entry package (via Git URL)
    - Window → Package Manager
    - "+" → "Add package from git URL…"
    - Paste: `https://github.com/CanTalat-Yakan/UnityEssentials.git`

- Install or update Unity Essentials packages
    - Tools → Install & Update UnityEssentials
    - Install all or select individual modules; run again anytime to update

---

# Mask Map Generator

> Quick overview: Pack Metallic, AO, Detail Mask, and Smoothness into a single RGBA texture. Preview, then save as PNG. Includes auto‑fix for import settings and optional Smoothness invert (Roughness = 1 − Smoothness).

A focused editor utility that combines up to four grayscale inputs into a standard PBR mask map. It streamlines texture preparation for pipelines that expect channel‑packed maps (R/G/B/A). When a channel texture is missing, you can supply a default scalar value instead.

![screenshot](Documentation/Screenshot.png)

## Features
- Channel packing (RGBA)
  - R: Metallic
  - G: Ambient Occlusion
  - B: Detail Mask
  - A: Smoothness (optional "Invert Smoothness" toggle to use Roughness)
- Smart defaults and preview
  - Per‑channel default sliders when a texture is not assigned
  - "Update Preview Texture" to generate and inspect before saving
- One‑click export
  - "Pack and Save Texture" uses a sensible default file name and target folder
  - Saves as PNG inside your project and refreshes the Asset Database
- Import settings auto‑fix and restoration
  - Temporarily sets textures to Read/Write enabled and disables Crunch compression
  - Restores original import settings afterward
- Dimension validation
  - Base size inferred from the first valid input; mismatched textures are ignored with a warning
- Quality of life
  - Default file name derived from the first input (appends/keeps "Mask")
  - Default folder derived from the first input’s folder
  - Progress bars during preview and packing
  - Keyboard shortcut: Tools → Mask Map Packer (Ctrl/Cmd + T)

## Requirements
- Unity Editor 6000.0+ (Editor‑only; no runtime code)
- Textures should have sRGB enabled for typical mask workflows (recommended in footer help)

## Usage
1) Open: Tools → Mask Map Packer (shortcut: Ctrl/Cmd + T)
2) Assign any subset of inputs; leave some empty to use defaults:
   - Metallic → Red channel
   - AO → Green channel
   - Detail Mask → Blue channel
   - Smoothness → Alpha channel (enable "Invert Smoothness" if you have Roughness instead)
3) Optionally adjust the default sliders for any missing inputs
4) Click "Update Preview Texture" to generate a preview
5) Click "Pack and Save Texture" and choose a save location (a default folder/name is suggested)

### Channel mapping
- R = Metallic (texture grayscale or Default Metallic)
- G = Ambient Occlusion (texture grayscale or Default AO)
- B = Detail Mask (texture grayscale or Default Detail)
- A = Smoothness (texture grayscale or Default Smoothness)
  - Invert Smoothness flips A := 1 − A for roughness workflows

### Texture requirements and auto‑fix
- Read/Write must be enabled. Crunch compression must be disabled
- The tool detects non‑compliant inputs and offers to fix them
- For processing, textures are set to Read/Write = true, Crunch = false; the original settings are restored afterward

### Dimensions
- The first valid input defines the target size
- Any input with mismatched dimensions is ignored and cleared with a warning
- If no inputs are valid, a 512×512 default is used and defaults fill all channels

### Naming and folder suggestions
- Default folder: taken from the first assigned texture’s folder
- Default file name: based on the first assigned texture; if it doesn’t end with "Mask", the last word is trimmed and "Mask" is appended (e.g., `Wall_BaseColor` → `Wall_ Mask` → `Wall_Mask`)

## Notes and Limitations
- Color space: Grayscale samples use Texture2D.GetPixel; ensure your import color space matches your intended workflow
- Preview uses the same pipeline as export; large textures may take a moment to generate
- Mismatched inputs are ignored; for best results, provide same‑size textures
- Only PNG export is provided by this tool

## Files in This Package
- `Editor/MaskMapGeneratorEditor.cs` – Editor window (UI, preview, export, menu, shortcut)
- `Editor/MaskMapGenerator.cs` – Core processing (validation, import settings fix/restore, packing, naming)
- `Editor/UnityEssentials.MaskMapGenerator.Editor.asmdef` – Editor assembly definition

## Tags
unity, unity-editor, mask map, channel packing, metallic, ambient occlusion, detail mask, smoothness, roughness, pbr, texture, png, authoring, tools
