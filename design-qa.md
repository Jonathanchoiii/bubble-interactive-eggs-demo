# Bubble Interactive Eggs Demo — Design QA

## Evidence

- Source visual truth:
  - `qa/reference-heart-sent.png` — 628 × 1048 px, sent state.
  - `qa/reference-heart-start.png` — 362 × 368 px, focused tracing start state.
- Browser-rendered implementation:
  - `qa/heart-sent-centered-mobile.jpg` — 430 × 720 px app canvas, sent state.
  - `qa/heart-tracing-start-focus.jpg` — 362 × 362 px normalized focus crop, untouched tracing state.
  - `qa/heart-receiving.png` — 1280 × 720 px browser capture with the 430 × 720 app canvas centered, receiving animation.
- Combined comparison: `qa/visual-comparison.png`.
- Browser viewport: 1280 × 720 CSS px; native app canvas: 430 × 720 CSS px; device scale factor: 1.
- Density normalization:
  - The 628 × 1048 sent reference was contained at 430 px width for comparison with the 430 × 720 app canvas.
  - The tracing implementation was cropped to the same top-left interaction region, then normalized to 362 × 362 px. The 362 × 368 reference was center-cropped by 6 px vertically in the comparison page.

## Full-view comparison

The sent-state comparison shows the user screenshot’s delivery pill left of the envelope center. In the revised implementation, the pill center and the 430 px app-canvas center both measure 640 px in the browser, for a 0 px delta. Envelope asset, pink background, typography, copy, radius, and shadow treatment remain consistent with the existing experience.

The full implementation includes the existing heading area that is outside the user’s cropped sent-state reference. This is a framing difference, not a design change.

## Focused-region comparison

The tracing-start comparison uses the same upper-left heart region. The malformed pink/red zero-length stroke visible in the user screenshot is absent. The path now starts at `M 77 204`, aligned to the illustrated fingertip, while preserving the closed heart contour.

The receiving-state capture confirms the animated pink heart is visible above the open envelope. Computed stacking order is heart `z-index: 5`, open envelope `z-index: 4`, and tucked-envelope replacement `z-index: 6`.

## Required fidelity surfaces

- Fonts and typography: unchanged from the existing build; delivery copy remains “回应已送达” with the same size, weight, letter spacing, and pill treatment.
- Spacing and layout rhythm: delivery pill is mathematically centered; cheer controls are inset 20 px and now sit at the app-canvas edges without overflow.
- Colors and visual tokens: existing pink, blue, white, opacity, border, and shadow tokens are unchanged.
- Image quality and asset fidelity: original raster heart and envelope assets are preserved without resampling in the running experience.
- Copy and content: birthday “改用滑动” control is removed from display; all other requested copy is unchanged.

## Comparison history

1. Earlier P1: “回应已送达” was visibly left-shifted.
   - Fix: centered with equal left/right constraints and removed the conflicting GSAP horizontal percentage transform.
   - Post-fix evidence: `qa/heart-sent-centered-mobile.jpg`; measured center delta is 0 px.
2. Earlier P1: zero-length rounded stroke rendered as a malformed red/pink start dot, and the visual trace started to the right of the illustrated fingertip.
   - Fix: hide progress before tracing starts and rotate the existing closed path to the fingertip-aligned point.
   - Post-fix evidence: `qa/heart-tracing-start-focus.jpg`; computed initial progress opacity is 0.
3. Earlier P1: animated heart was behind the open envelope.
   - Fix: raise the moving heart above the open-envelope layer while keeping the tucked state above both.
   - Post-fix evidence: `qa/heart-receiving.png`.
4. Earlier P2: both cheer controls and their halos overflowed the canvas.
   - Fix: move left and right control anchors inward by 20 px.
   - Post-fix evidence: measured left and right offsets are both 0 px inside the 430 px app canvas.

## Interaction and runtime checks

- Traced the heart from the revised fingertip start through 91.2% progress; completion triggered the receive, tuck, seal, send, and sent states.
- Verified the heart-over-envelope receiving frame and the final centered delivery pill.
- Verified both cheer controls, including halos, remain fully inside the canvas.
- Verified the birthday slide-switch override is present and hidden.
- Browser console warnings/errors: none.
- JavaScript syntax and whitespace checks: passed.

## Findings

No actionable P0, P1, or P2 findings remain for the requested changes.

## Follow-up polish

No P3 follow-up is required for this pass.

final result: passed
