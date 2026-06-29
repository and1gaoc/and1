# Sample: login validation

This is the bundled sample for the `dsl_validate` skill in this project.

Use this sample when:

- the user wants to validate a DSL reconstructed from a login-style page
- the user wants a concrete example of `dsl.json -> HTML -> screenshot -> comparison`
- the current input resembles a simple form, card, or account login UI

## Source files

- Image: `../sample/login/login.png`
- DSL: `../sample/login/dsl.json`
- Field specification: `dsl字段说明.md`

## Example prompt

```text
Render sample/login/dsl.json to HTML, capture the rendered image, then compare it with sample/login/login.png and report similarity plus visible differences.
```

## What this sample demonstrates

- original image and DSL are bundled together inside the skill
- the DSL should be checked against `dsl字段说明.md` before rendering
- validation is based on HTML rendering instead of direct JSON inspection
- the output should include a rendered screenshot
- the output should include an AI-assessed similarity score
- the output should include a concrete difference summary
- a strict `validated-dsl.json` can be emitted as a separate artifact when requested

## Expected validation focus

When validating this sample, pay special attention to:

- overall card position and size
- title position and visual weight
- username and password input alignment
- login button size, color, and corner radius
- footer link alignment and spacing
- background image or panel fidelity if represented through assets

## Interpretation notes

- A strong result should preserve the main page layout and control hierarchy.
- Small differences in font rendering, shadows, or anti-aliasing can be noted as minor drift.
- Missing inferred assets should reduce the score and be called out explicitly.
