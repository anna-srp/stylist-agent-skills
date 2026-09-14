# Virtual Try-On Edit Prompts

Use the prompt matching the actual Image 2 reference. Keep Image 1 first and Image 2 second.

## Single item

```text
STRICT IMAGE EDIT. IMAGE 1 is the immutable base photo, the only target person, and the only permitted canvas and composition. IMAGE 2 is a wardrobe reference only. Start from IMAGE 1 and edit only the fashion slot supplied by IMAGE 2. Put the single fashion item visible in IMAGE 2 onto the person in IMAGE 1 in its correct real-world position. Outerwear must be visibly worn over inner clothing. If only one shoe is pictured, create its matching mate and put the pair on both feet. A bag must be held or worn naturally; accessories must be attached to their correct body locations. The output must contain no detached, displayed, floating, or floor-placed product. Preserve IMAGE 1's exact person identity, face, hair, skin tone, body proportions, pose, hands, background, lighting, crop, framing, and every fashion item outside the replaced slot. Never copy a person, face, body, pose, background, layout, or composition from IMAGE 2. Use IMAGE 2 directly as the sole visual source for the selected item without redesigning, simplifying, or recoloring it. Final check: only the person and scene from IMAGE 1 remain, with the selected item physically worn or carried. Do not change anything else.
```

## Complete look

```text
STRICT IMAGE EDIT WITH FIXED IMAGE ROLES. IMAGE 1 is the immutable base photo, the only target person, and the only permitted canvas and composition. IMAGE 2 is a wardrobe reference only; any person in IMAGE 2 is only a clothing display model and must never appear in the output. Start from IMAGE 1. Identify every selected wearable or carryable fashion item in IMAGE 2, including tops, bottoms, one-piece garments, layers, outerwear, shoes, bags, belts, jewelry, scarves, hats, and sunglasses, then transfer all selected items onto the person in IMAGE 1. Outerwear must be visibly outermost. Shoes must be one matching pair on both feet. Bags and accessories must be worn or carried naturally. Every selected item must be physically attached to the IMAGE 1 person; leave no product detached, floating, displayed beside the person, or on the floor. Do not omit, duplicate, or merge selected items, and do not retain an original IMAGE 1 garment in a slot supplied by IMAGE 2. Preserve IMAGE 1's exact person identity, face, hair, skin tone, body proportions, pose, hands, background, lighting, crop, and framing. Never copy a person, face, body, pose, background, layout, composition, or incidental styling from IMAGE 2. Use IMAGE 2 directly as the visual source without redesigning, simplifying, or recoloring the look. Final check: only the person and scene from IMAGE 1 remain; every selected item from IMAGE 2 is present once, outerwear is worn, both feet have matching shoes, and nothing remains detached. Do not change anything else.
```

## Structural completion gate

Before publishing the result, verify every selected product appears exactly once and every incidental product appears zero times. A selected bottom must be continuous from waistband through its intended hem. A dress or jumpsuit must remain one continuous garment and replace conflicting original top and bottom slots. Reject missing layers, mismatched or fused shoes, floating bags, copied display models, or a full look that changes only the top.
