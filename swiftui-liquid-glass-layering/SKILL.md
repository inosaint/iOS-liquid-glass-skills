---
name: swiftui-liquid-glass-layering
description: Use when implementing or fixing SwiftUI Liquid Glass controls, pills, chips, bars, buttons, or search chrome where text/icons must remain readable above glass. Especially use when glassEffect is applied to the same view as visible labels, content appears dim/blurred/covered, scrollable glass chips drift from labels, or a design needs the split surface/content layering pattern used by location pills.
---

# SwiftUI Liquid Glass Layering

## Core Rule

Keep Liquid Glass surfaces separate from visible text and icons.

Draw glass on a lower, non-semantic layer. Render tappable labels, icons, and text as a sibling foreground layer above it. Do not put `.glassEffect(...)` or glass fallback backgrounds directly on the visible label view when readability matters.

## Before Implementing

Check with the user whether the Liquid Glass implementation should have animation.

If animation is requested or clearly implied, use `$swiftui-motion-microinteractions` when available. Keep this skill focused on glass layering, readability, hit testing, and measurement.

## Preferred Pattern

Use this for compact location pills, recent chips, search pills, and custom glass buttons:

```swift
ZStack {
    glassSurface
        .allowsHitTesting(false)
        .accessibilityHidden(true)

    Button {
        action()
    } label: {
        ControlContent(model: model)
    }
    .buttonStyle(.plain)
}
```

Keep accessibility and action semantics on the visible control, not the hidden glass surface.

## Dynamic Sizing

For dynamic-width controls, size the glass from the same content shape while keeping visible content above it:

```swift
private var glassSurface: some View {
    ControlContent(model: model)
        .hidden()
        .overlay {
            Color.clear
                .nativeCompactLocationPillGlass()
        }
        .accessibilityHidden(true)
}
```

Use this when the glass and content can live in the same component without readability issues.

## Parent-Layer Split

If text is still swallowed, blurred, or dimmed, move glass out of the control component. Put glass surfaces in the parent chrome/background layer and put labels/buttons in a separate foreground layer with a higher `zIndex`.

```swift
ZStack {
    chromeGlassLayer
        .zIndex(0.5)
        .allowsHitTesting(false)

    chromeContentLayer
        .zIndex(0.75)
}
```

This matches the `feels.fyi` location pill structure:
- `ContentView.bottomGlassLayer` draws `locationPillGlassSurface`.
- `locationButtonLayer` draws `locationPillContent`.

## Scrollable Chips

For scrollable chip rows, use one real scroll view for the visible, tappable content. Measure each foreground chip in a shared coordinate space with a `PreferenceKey`, then draw lower glass surfaces at those measured frames in the parent chrome/background layer.

Avoid duplicate lower/upper scroll views. Even with bound scroll positions, layout and gesture timing can drift so text scrolls without the bubble.

## Measured Scroll Layers

For scrollable result rows or chips whose glass is drawn from measured foreground frames:
- Clip the real scroll view to the same viewport used to mask the measured glass layer.
- Keep fixed chrome glass, such as a search field or cancel button, above scroll-result glass in `zIndex`.
- Instantiate a measured glass surface only when its matching foreground item is visible. Opacity alone can still let native Liquid Glass appear as empty pills before text arrives.
- Clear stale measured frames when the query, source data, or presentation state changes.

## Fixed-Size Controls

For fixed-size controls, use a direct clear glass surface below visible content:

```swift
ZStack {
    Color.clear
        .frame(width: width, height: height)
        .nativeCompactLocationPillGlass()

    LabelContent()
}
```

## Checklist

- Put glass/rim surfaces in their own `Color.clear` or hidden sizing layer.
- Put text, icons, and labels in a separate visible layer above glass.
- Wrap related glass surfaces in `GlassEffectContainer` or the app's local helper, such as `nativeLiquidGlassContainer(spacing:)`.
- Promote glass to a parent chrome/background layer when labels blur, dim, or drift.
- Match scroll clipping and measured-glass masks when glass is drawn outside the scroll view.
- Gate measured glass on the same visibility state as the foreground content, not just `.opacity(0)`.
- Use `.allowsHitTesting(false)` on decorative glass layers when tappable content lives elsewhere.
- Mark hidden sizing copies with `.accessibilityHidden(true)`.
- Keep button/action/accessibility semantics on the visible control.
- Ask whether animation is needed before adding motion; delegate motion details to `$swiftui-motion-microinteractions`.

## Avoid

Do not do this for readable controls:

```swift
Text(city.name)
    .padding(.horizontal, 12)
    .background {
        Color.clear.glassEffect(.clear.interactive(), in: .capsule)
    }
```

This can make text appear swallowed by Liquid Glass because the material and visible label are composed in the same layer.
