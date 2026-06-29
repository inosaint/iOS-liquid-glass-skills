# Choreography

Use choreography when multiple pieces need ordered timing: keyboard arrival, search chrome, delayed rows, staggered children, async loading, or dismissals.

## Principles

- Put timing constants in one small type close to the feature.
- Cancel delayed tasks when the UI state that requested them disappears.
- Hide instantly on dismissal when delayed appearance would otherwise race.
- Use system-provided keyboard timing when moving chrome with the keyboard.

## Staggered Children

Reveal a row first, then reveal children one by one:

```swift
Task { @MainActor in
    visibleIDs = []
    try? await Task.sleep(for: rowDelay)
    guard shouldStillShow else { return }

    isRowVisible = true
    try? await Task.sleep(for: firstChildDelay)

    for item in items {
        guard shouldStillShow else { return }
        visibleIDs.insert(item.id)
        try? await Task.sleep(for: childInterval)
    }
}
```

Cancel that task on query changes, dismissal, or source data replacement.

For split foreground/glass rows, gate every visual layer from the same visibility state. Do not create measured Liquid Glass surfaces before the foreground item is visible; opacity-only hiding can show an empty stack of glass pills while the stagger waits.

## Keyboard Coordination

When chrome moves with the keyboard:
- derive duration and curve from `UIResponder.keyboardWillChangeFrameNotification`
- clamp very short durations
- remove overlays after keyboard/state animation completes, plus a small buffer

This avoids chrome lagging behind the keyboard or disappearing before the keyboard settles.

## Reduced Motion

Skip stagger delays when reduced motion is enabled. Show the final state directly or with a simple fade.
