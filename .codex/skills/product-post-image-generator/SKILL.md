---
name: product-post-image-generator
description: Generate final Bình Minh SG product post images from posts/[product]/data/prompt-template-1.json, posts/[product]/data/input-3-image.jpg, and posts/post-product-template-1.png. Use when Codex is asked to create product banner images, product post visuals, template-1 images, img-1/img-2 variants, or verify required fixed fields and logo correctness for a product folder under posts/.
---

# Product Post Image Generator

## Core Rule

Use the system `$imagegen` skill for every image generation or image edit. Before generating images, open and follow:

`C:\Users\Le Tan Tai\.codex\skills\.system\imagegen\SKILL.md`

Do not replace the AI image step with deterministic compositing, CSS, SVG, PIL-only rendering, or manual layout recreation unless the user explicitly asks for that.

## Required Inputs

Resolve the product folder from the user's request.

- Product folder: `posts/[product]`
- Product data folder: `posts/[product]/data`
- JSON prompt: `posts/[product]/data/prompt-template-1.json`
- Product input image: `posts/[product]/data/input-3-image.jpg`
- Template input: `posts/post-product-template-1.png`

If the product folder is not provided and cannot be inferred from the active file, current context, or nearby paths, ask the user for the product folder name.

If any required input is missing, stop and report the missing path. Do not generate from partial inputs unless the user approves a fallback.

## Naming

Save only final image files in `posts/[product]/`.

Create the output stem by slugifying the product folder name or product name:

- Lowercase Vietnamese text.
- Remove accents.
- Replace spaces and punctuation with hyphens.
- Collapse repeated hyphens.
- Trim leading and trailing hyphens.

Use these filenames:

- `posts/[product]/[slug]-img-1.jpg`
- `posts/[product]/[slug]-img-2.jpg`

Example: `posts/Trứng cá hồi size trung/trung-ca-hoi-size-trung-img-1.jpg`

Do not keep draft, preview, intermediate, alternate, or failed-generation files in the product folder. If a generation tool creates temporary artifacts, delete or ignore them after saving the final selected result.

## Workflow

1. Read `prompt-template-1.json`.
   - Use structured JSON parsing or direct inspection.
   - Identify product name, variable product information, design zones, fixed fields, brand assets, and prompt guidance.
2. Inspect the two image inputs before generation:
   - `posts/[product]/data/input-3-image.jpg`
   - `posts/post-product-template-1.png`
3. Generate `img-1`.
   - Use the JSON prompt as the primary design specification.
   - Use `input-3-image.jpg` as the product/package source.
   - Use `post-product-template-1.png` as the fixed layout/template reference.
   - Preserve the template's required layout, fixed fields, and Bình Minh SG branding.
   - Save the final accepted image as `[slug]-img-1.jpg`.
4. Generate `img-2`.
   - Use the same product, required brand rules, fixed fields, and logo requirements.
   - Creatively change the context, composition, styling, mood, angle, photography setup, or supporting scene so it is visibly different from `img-1`.
   - Keep the result professional, premium, B2B-friendly, and appropriate for Bình Minh SG product posts.
   - Save only the final accepted image as `[slug]-img-2.jpg`.
5. Verify both outputs after saving.

## Generation Prompt Requirements

For `img-1`, explicitly include:

- The full JSON prompt/spec as the source of truth.
- The product input image must be used for the product/package appearance.
- The template image must be used as the layout reference.
- Canvas ratio must follow the JSON/template, normally 1080 x 1350 px or 4:5.
- All `FIXED`, `FIXED_style`, and required brand fields in the JSON must remain present and visually faithful.
- The Bình Minh SG logo must match the required logo description and placement; do not invent a wrong logo, recolor it incorrectly, omit text, distort the fish/globe mark, or separate the mark from its text.

For `img-2`, add a short creative direction such as:

- alternate premium seafood retail scene,
- different food styling surface,
- different camera angle,
- more editorial Japanese restaurant mood,
- cleaner studio commercial composition,
- or another distinct professional B2B product-post direction.

Do not change mandatory brand/logo/fixed requirements while making `img-2` creative.

## Verification Checklist

After each generated image, inspect the saved file with visual tools and verify:

- File exists at the required output path.
- Image is not blank, corrupt, or obviously low quality.
- The product/package from `input-3-image.jpg` is represented clearly.
- The layout follows `post-product-template-1.png` and `prompt-template-1.json`.
- All JSON fields marked `FIXED`, `FIXED_style`, or otherwise required are present.
- Bình Minh SG logo is visible, correctly placed, and faithful to the required fish/globe/text mark.
- Logo text reads `BÌNH MINH SG` and is not replaced by unrelated text.
- Required product name, headline, badge, USP/icons, commitment/footer elements, and application/product imagery are not missing when specified by the JSON.
- Text is not badly garbled, cropped, overlapping, or unreadable in key brand/product areas.
- `img-2` is meaningfully different from `img-1` in scene, composition, or style while still obeying fixed requirements.

If a file fails verification, regenerate only the failed output with a tighter prompt focused on the failed checklist item. Save the corrected final to the same required filename.

## Output Summary

After completion, report:

- Product folder processed.
- Required inputs found.
- Final output paths.
- Verification result for each image.
- Any failed checks, regenerations, or unresolved issues.
