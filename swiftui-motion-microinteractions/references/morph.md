# Morph

Use morph when two states are the same conceptual object: a location pill becoming a search field, a compact button expanding into a bar, or a Liquid Glass surface preserving identity across layouts.

## Shared Identity

Prefer platform identity APIs:

```swift
if #available(iOS 26, *) {
    view.glassEffectID(id, in: namespace)
} else {
    view.matchedGeometryEffect(id: id, in: namespace)
}
```

Use one stable ID per conceptual object. Do not reuse the same ID for unrelated surfaces.

## Layered Morphs

When content and glass are split, morph the surface layer and fade/swap the content layer separately. Keep visible text/icons crisp above glass.

## Avoid

Avoid morphing between different semantics just because the frames line up. If the user would not perceive the two states as the same object, use a reveal or replacement instead.
