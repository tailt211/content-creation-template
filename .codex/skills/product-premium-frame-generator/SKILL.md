---
name: product-premium-frame-generator
description: Generate Premium Import Frame branded product images for Binh Minh SG product post assets. Use when Codex is asked to create, edit, apply, or batch-generate premium frame images for files inside posts/[product]/data, especially when using templates/images/frame-temp-1.jpg, excluding input-3-image.jpg, and requiring the system imagegen skill.
---

# Product Premium Frame Generator

## Core Rule

Use the system `$imagegen` skill for every image generation or edit. Before generating images, open and follow:

`C:\Users\Le Tan Tai\.codex\skills\.system\imagegen\SKILL.md`

Do not replace this with deterministic image compositing, CSS, SVG, PIL-only framing, or manual overlay work unless the user explicitly changes the requirement.

## Workflow

1. Resolve the product data folder from the user's request:
   - Expected folder: `posts/[product]/data`
   - If the product name is not provided and cannot be inferred, ask the user for the product folder.
2. Confirm the template exists:
   - `templates/images/frame-temp-1.jpg`
3. Find target images in `posts/[product]/data`:
   - Include common raster images: `.jpg`, `.jpeg`, `.png`, `.webp`
   - Exclude `input-3-image.jpg`
   - Exclude previously generated frame outputs if they clearly use names such as `*-premium-frame.*`, `premium-frame-*.*`, or are inside a generated output folder.
4. For each target image, call imagegen with both inputs:
   - `TEMPLATE_FRAME_IMAGE = templates/images/frame-temp-1.jpg`
   - `TARGET_IMAGE = [target image path]`
   - Load both local images with `view_image` before using the built-in `image_gen` edit flow.
   - Use the prompt in `references/premium-frame-prompt.md`, replacing `[tên_file_ảnh_cần_chỉnh]` with the target file path or filename.
5. Save outputs without overwriting originals:
   - Default output folder: `posts/[product]/`
   - Default output path: `posts/[product]/[original-stem]-premium-frame.jpg`
   - If the user requests another location or naming convention, follow it.
6. Verify each output:
   - Main content of the target image remains unchanged.
   - Navy premium border, top-left `Hàng Sẵn kho` tag, top-right Bình Minh SG logo, central watermark, light logo pattern, and footer are present.
   - Logo is not redrawn, separated, recolored incorrectly, or distorted.
   - Background watermark pattern uses evenly spaced repeated watermarks, while excluding the large central watermark from spacing checks.
   - Watermarks remain subtle and do not obscure important labels, cartons, people, warehouse details, or product details.
7. If any output fails verification, regenerate with a tighter prompt focused only on the failed issue.

## Output Summary

After completion, report:

- Product folder processed
- Number of target images found and generated
- Output paths
- Any skipped files and why
- Any generation or verification issues

## Reference

Read `references/premium-frame-prompt.md` only when preparing the imagegen prompt.
