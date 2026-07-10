---
name: product-display-image-generator
description: Generate the main 3:4 product image for a product in context/products/[product], choosing packaging image generation from clear package photos or AI-created plated/bowl product display from detail.md, then apply the Bình Minh SG premium frame and save only the final framed image under posts/[product]. Use when Codex is asked to create main product display images, realistic plated food/product images, package mockups from product photos, or the main framed image for a product.
---

# Product Display Image Generator

## Core Rules

Use the system `$imagegen` skill for every generated or edited image. Before generation, open and follow:

`C:\Users\Le Tan Tai\.codex\skills\.system\imagegen\SKILL.md`

After the base product image is accepted, use:

`C:\My folder\work\prompt\content-creation-template\.codex\skills\product-premium-frame-generator\SKILL.md`

Do not replace these steps with deterministic compositing, CSS, SVG, PIL-only rendering, or manual overlay work unless the user explicitly asks for that.

All base images must be portrait `3:4`, target composition `1086x1448`, on a clean white background.

## Product Resolution

Resolve the product from the user's request, active file, open tabs, or nearby paths.

- Product source folder: `context/products/[Sản phẩm]`
- Product detail file: `context/products/[Sản phẩm]/detail.md`
- Output folder: `posts/[Sản phẩm]`

If the product cannot be inferred, ask for the product folder name.

If `detail.md` is missing, ask before generating from images only unless the user explicitly requested packaging and package photos are present.

Create `posts/[Sản phẩm]` if it does not exist.

## Naming

Create a slug from the product folder name:

- Lowercase Vietnamese text.
- Remove accents.
- Replace spaces and punctuation with hyphens.
- Collapse repeated hyphens.
- Trim leading and trailing hyphens.

Save exactly one final output under `posts/[Sản phẩm]`:

- Final premium-frame image: `main-[slug]-premium-frame.jpg`

Example: `posts/Cá trích ép trứng Nissi/main-ca-trich-ep-trung-nissi-premium-frame.jpg`

Never overwrite an existing final asset unless the user explicitly asks for replacement. If a file exists, create a sibling version such as `main-[slug]-premium-frame-v2.jpg`.

Do not keep or report base, draft, source, temporary, or PNG quality-check copies after the final framed JPG is created, unless the user explicitly asks to preserve intermediates.

## Mode Decision

Choose exactly one base-image mode before generation.

### Explicit Packaging Request

If the user asks for packaging, package image, bao bì, hộp, thùng, túi, label, or product package:

1. Use clear raster images in `context/products/[Sản phẩm]` as packaging references.
2. Do not require additional detail inspection beyond identifying the product name and output path.
3. Generate a clean package/product mockup that follows the reference photos as closely as possible.
4. Preserve visible package design, logo, label layout, colors, contents, weight text, storage marks, and package material from the references.
5. Do not invent new branding, label colors, claims, certifications, text blocks, or package shape.

If no usable package reference image exists, stop and report that packaging generation needs a clear package/hộp/thùng reference.

### Explicit Real Display Request

If the user asks for a realistic display image, plated image, food presentation, ảnh trưng bày thực tế, đĩa, bát, or says not to use packaging:

1. Read `detail.md`.
2. Generate the product itself as a realistic catalog/product-food presentation.
3. Use a white plate, bowl, tray, or simple white serving surface when appropriate.
4. Do not include packaging, boxes, labels, cartons, barcodes, vacuum packs, brand marks, or printed text.
5. Use AI generation from product facts in `detail.md`; do not rely on package photos as the visual subject.

### Default Request

If the user does not specify packaging or display:

1. Read `detail.md`.
2. Inspect raster images in `context/products/[Sản phẩm]`.
3. If there are clear photos of the real package, box, bag, pouch, tray label, or product container, choose **Explicit Packaging Request** behavior.
4. If there are no clear packaging/container references, choose **Explicit Real Display Request** behavior.

Treat warehouse cartons, shipping boxes, and readable retail/foodservice packaging as packaging references when they clearly identify how the product is sold.

## Base Image Prompt Requirements

For packaging mode, include:

- Input images with their role: package/reference photos.
- Product name from `detail.md` or folder name.
- Requirement to follow the reference images `100%` for package appearance.
- White background, 3:4 portrait, clean catalog lighting.
- No invented text, colors, claims, or extra variants.

For real display mode, include:

- Product facts from `detail.md`: name, category, form, ingredient, color, texture, storage/use clues, and variants.
- Requirement to display all color variants in one image when `detail.md` lists multiple label/color/product variants.
- White background, 3:4 portrait, clean catalog lighting.
- White plate/bowl/tray presentation when the product is food.
- No packaging, boxes, labels, cartons, logos, printed text, people, or clutter.

Use concise prompt structure from `$imagegen`: use case, asset type, primary request, scene/backdrop, subject, style, composition, lighting, constraints, avoid.

## Premium Frame Step

After the base image passes visual inspection:

1. Keep the accepted base image only as a temporary/intermediate file needed for the frame edit.
2. Load the temporary base image and `templates/images/frame-temp-1.jpg` with `view_image`.
3. Use `product-premium-frame-generator` and its `references/premium-frame-prompt.md`.
4. Apply the premium frame while preserving the base image content.
5. Save the final framed image as `posts/[Sản phẩm]/main-[slug]-premium-frame.jpg`.
6. Delete any temporary base/source/draft files created for the frame step after the final JPG passes verification.

When using the premium prompt, reinforce the base-mode invariants:

- Packaging mode: keep the package/reference-derived product unchanged.
- Real display mode: keep the plated/bowl product unchanged and do not add packaging.

## Verification Checklist

Verify after saving:

- Temporary base image was inspected before framing, then removed after final verification unless the user asked to keep intermediates.
- Final framed image exists and is `1086x1448` unless the generation tool returns the same 3:4 ratio with a different exact size; prefer regenerating if exact size is required by the user.
- White-background product presentation was preserved inside the final framed image.
- Correct mode was chosen from the request/default decision tree.
- Packaging mode follows available package photos and does not invent unrelated design.
- Real display mode shows the actual product as food/product presentation and contains no packaging.
- All product variants/colors mentioned in `detail.md` are present in one image when requested or relevant.
- Premium frame includes navy border, `Hàng Sẵn kho`, Bình Minh SG logo, subtle watermark/pattern, and footer.
- Watermark does not obscure important product details.

If a check fails, regenerate only the failed stage with a tighter prompt.

## Output Summary

Report:

- Product processed.
- Chosen mode: packaging or real display.
- Reference/detail inputs used.
- Final output path only.
- Verification result and any unresolved issue.
