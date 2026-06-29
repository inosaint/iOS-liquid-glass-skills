---
name: swiftui-motion-microinteractions
description: Design, implement, and review SwiftUI/iOS motion microinteractions. Use when working on chip/button pops, staggered reveals, keyboard-coordinated search chrome, matched-geometry or Liquid Glass morphs, splash/status fades, haptic-coupled taps, reduced-motion behavior, or when animation feels like it travels, drifts, swims, blurs, or fights layout.
---

# SwiftUI Motion Microinteractions

## Core Rules

Start from intent. Decide whether the motion should be a pop, reveal, morph, choreography, or feedback cue before choosing modifiers.

Keep motion semantic and local:
- Pop means appear in place with opacity and centered scale.
- Travel means visible movement across space; use it only when the design wants relocation.
- Reveal means fade content or state in without pretending the element has mass.
- Morph means preserve identity between two related surfaces.
- Choreography means coordinate timing across system UI, async state, and staged children.

Respect reduced motion. Prefer opacity-only or immediate state changes when `accessibilityReduceMotion` is true.

Avoid animating layout unless layout change is the point. If a view is measured, scrolled, or split into separate glass/content layers, apply the same animation vocabulary to every visual layer.

## Pattern Router

- For in-place chips, pills, bubbles, badges, buttons, or small cards: read [pop.md](references/pop.md).
- For delayed rows, staggered children, keyboard timing, and task-driven sequences: read [choreography.md](references/choreography.md).
- For shared identity transitions, matched geometry, and Liquid Glass surface morphs: read [morph.md](references/morph.md).
- For loading-to-loaded, splash, empty-state, or availability changes: read [reveal.md](references/reveal.md).
- For tap feedback and haptic timing: read [haptics.md](references/haptics.md).

## Workflow

1. Inspect existing motion before adding new timing:
   ```bash
   rg -n "withAnimation|\\.animation\\(|transition\\(|scaleEffect|offset\\(|matchedGeometryEffect|glassEffectID|Task\\.sleep|accessibilityReduceMotion|AppHaptics" app
   ```
2. Name the intended motion class.
3. Reuse local timing curves when the surrounding UI already has a rhythm.
4. Keep state changes explicit and cancel delayed tasks on dismissal.
5. Add reduced-motion behavior for any scale, blur, morph, travel, or stagger.
6. Verify that text/icons remain readable and that animations do not alter hit targets unexpectedly.

## Feels.fyi Seeds

The first patterns came from `feels.fyi`:
- Recent searched location chips: pop, stagger, split glass/content synchronization.
- Search chrome: keyboard choreography and delayed overlay dismissal.
- Location pill to search field: Liquid Glass/shared-identity morph.
- Splash resolve: fade and subtle scale.
- Weather scene availability: loaded-state fade.
- Tap actions: selection haptic before state transition.
