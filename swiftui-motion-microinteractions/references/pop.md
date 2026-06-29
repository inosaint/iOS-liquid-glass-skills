# Pop

Use pop for chips, bubbles, badges, compact buttons, pills, and small controls that should appear in place. Pop is not travel.

## In-Place Pop

Use opacity plus centered scale:

```swift
.scaleEffect(isVisible ? 1 : 0.86, anchor: .center)
.opacity(isVisible ? 1 : 0)
.animation(.timingCurve(0.16, 1, 0.3, 1, duration: 0.52), value: isVisible)
```

Do not add vertical offset unless the design explicitly asks for rise/fall motion.

## Split Glass And Content

When Liquid Glass and text are separate layers, animate both with the same pop:

```swift
let scale = reduceMotion ? 1 : (isVisible ? 1 : 0.86)

glassLayer
    .scaleEffect(scale, anchor: .center)
    .opacity(isVisible ? 1 : 0)

contentLayer
    .scaleEffect(scale, anchor: .center)
    .opacity(isVisible ? 1 : 0)
```

If the content is scrollable or measured, measure the untransformed content frame first, then apply the same visual transform to both layers.

## Timing

For a soft bubble pop, start around:
- hidden scale: `0.82` to `0.88`
- duration: `0.42` to `0.56`
- curve: `.timingCurve(0.16, 1, 0.3, 1, duration: ...)`

For reduced motion, use opacity-only or a short fade:

```swift
reduceMotion ? .easeOut(duration: 0.18) : bubblePopAnimation
```

## Avoid

Avoid `.offset(y:)` for in-place pops. It reads as the element coming from below, especially on glass surfaces.

Avoid bottom-leading scale anchors unless the element should visibly grow out of a corner.
