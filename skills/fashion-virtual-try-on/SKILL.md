---
name: fashion-virtual-try-on
description: Create an AI visual preview of one fashion product or a complete selected look on a user's photo, or on a generated model when requested. Use for try this on, show me wearing this, virtual try-on, AI 试穿, or preview the whole look. Do not use for product search, fit scoring, or claims about real-world size and fit.
---

# Fashion Virtual Try-On

Create a useful image-editing preview while preserving identity, product fidelity, and privacy.

## Inputs

For a user-photo preview require:

- the current user photo or a photo they explicitly chose to reuse;
- exact product reference images;
- whether the target is one item or a complete look.

If no user photo exists, offer a generated-model preview and never present the model as the user. A text-only image made from a product description is a `concept preview`, not a product try-on.

Prefer clear, well-lit person photos where the requested clothing area is visible. Do not ask the user to expose more of their body than the requested item needs. Prefer product references with a readable silhouette and accurate color.

User photos and signed input URLs are private and session-only by default. Inspect them for the current task, never quote the URLs, and never save, reuse, or publish inputs without explicit consent.

## Edit

Use an image-editing capability that receives the person and product references together. Pass the person as Image 1 and the product or look as Image 2. Image 1 controls identity, pose, background, crop, and lighting. Image 2 is a wardrobe reference only.

Classify Image 2 as a single item or a complete look, then read [references/edit-prompts.md](references/edit-prompts.md) and use the matching prompt. Do not replace the reference images with a text description.

For a complete look, every selected garment, layer, shoe, bag, and accessory must be physically worn or carried by the Image 1 person. Outerwear must remain outermost and shoes must appear as a matching pair. Do not copy any display model, face, body, pose, background, or incidental styling from Image 2.

## Verify and deliver

Compare the result with both inputs. Reject it when identity changes materially, a selected garment is missing, an original garment remains in a replaced slot, the product category or color is wrong, anatomy is visibly broken, or an unselected item appears. A full-look output that changes only the top is incomplete.

When the image tool starts an asynchronous job, treat its acknowledgement as progress rather than a finished try-on. Use the platform's supported wait, yield, or completion-event path and deliver the exact new image produced by that job. Do not call the image generator again while the original job is pending, and never mark the request complete with only a progress sentence. Finish with the generated image or a clear terminal failure. Treat an insufficient-credit response as terminal for the current request and do not spend repeated calls retrying it.

Retry a failed structural edit at most once using the same inputs and roles. If it still fails, report the limitation rather than presenting an incorrect image.

State clearly that the result is an AI preview and does not guarantee sizing, fit, fabric behavior, or exact real-world appearance. Offer the selected product links or one targeted refinement. Never make the result public until the user explicitly asks to share it.
