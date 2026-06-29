# Reveal

Use reveal for loading-to-loaded states, splash exits, empty states, and status transitions where the content should quietly become available.

## Availability Fade

Use fade for weather/status availability:

```swift
.opacity(hasData ? 1 : 0)
.animation(hasDisplayedData ? .easeOut(duration: 0.36) : nil, value: hasData)
```

Suppress the first animation if the view has not displayed real data yet.

## Splash Resolve

A splash can resolve with fade plus tiny scale:

```swift
content
    .opacity(isResolving ? 0 : 1)
    .scaleEffect(isResolving ? 0.96 : 1)
```

Keep brightness/saturation changes subtle; they should support the exit, not become a separate effect.

## Radial Reveal

Use radial masks for presentation effects only when the reveal origin matters. Pair scale with blur carefully; high blur can make text feel smeared.

## Reduced Motion

Prefer opacity-only. Avoid large mask expansion, blur, or scale changes.
