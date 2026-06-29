# DSL Validation Report

- Original image: `D:\and1\samples\register\register.png`
- DSL file: `D:\and1\samples\register\dsl.json`
- Rendered HTML: `D:\and1\samples\register\rendered.html`
- Rendered image: `D:\and1\samples\register\rendered.png`
- Similarity score: 83/100

## DSL Compliance

- JSON valid: yes
- Spec-compliant: yes
- Invalid fields: none
- Strict JSON output: not requested

## Summary

The DSL reconstructs the registration form structure well: page size, title, input rows, underlines, checkbox position, label, and button placement are all close to the original. The largest visible difference is background fidelity, because the DSL references `assets/register_bg.png` as a placeholder and the rendered result falls back to a flat dark background instead of the original blurred/gradient background image.

Image metric reference: mean absolute RGB error is 14.19, RMSE is 25.42, and 71.73% of pixels are within RGB distance 32. This metric is lower than the semantic match because background texture accounts for much of the pixel drift.

## Key Differences

1. The rendered background is flat `#171A24`; the original has a blurred dark image/gradient with a visible lighter band near the upper-middle area.
2. The title and body text render slightly crisper and heavier than the screenshot, likely because browser font antialiasing differs from the original source.
3. The button color and dimensions are close, but the rendered button is more uniform because the original screenshot has subtle antialiasing and compression at edges.
4. The checkbox and text row placement is close; the rendered checkbox border appears slightly sharper than the original.
5. The input underlines align closely at y=122, y=183, and y=243, but the rendered line color is cleaner than the source image.

## Notes

- `assets/register_bg.png` is listed in the DSL but does not exist yet, so the render used the DSL `backgroundColor` fallback.
- The rendered screenshot was captured from `rendered.html` via a temporary local HTTP preview and cropped to the original 450x399 canvas.
