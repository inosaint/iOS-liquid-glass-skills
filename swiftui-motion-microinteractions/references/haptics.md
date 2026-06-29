# Haptics

Use haptics for direct user action feedback, not for passive animation.

## Selection

For lightweight taps that change UI state:

```swift
@MainActor
enum AppHaptics {
    static func selection() {
        let generator = UISelectionFeedbackGenerator()
        generator.prepare()
        generator.selectionChanged()
    }
}
```

Trigger haptics inside the action handler before the state transition so the feedback feels attached to the tap.

## Pairing With Motion

Use one haptic per explicit user action. Do not fire haptics for every staggered child or delayed animation step.

Respect context: opening search, selecting a location, clearing input, and closing search are good candidates for light selection feedback.
