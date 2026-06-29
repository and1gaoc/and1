---
name: dsl_validate
description: Use when validating how closely a dsl.json reconstruction matches an original UI image, especially when the workflow is dsl.json to HTML to screenshot, the DSL must be checked against dsl字段说明.md, or the user wants a rendered image, a similarity score, a difference summary, and a strict JSON output.
---

# dsl_validate

## Overview

Validate a `dsl.json` file by rendering it as HTML, capturing the rendered result, and comparing that result against the original UI image.

This skill is for validation, not for generating framework code.

In this project, the default bundled references are:

- DSL field specification: `references/dsl字段说明.md`
- Bundled example image: `sample/login/login.png`
- Bundled example DSL: `sample/login/dsl.json`
- Bundled sample note: `references/sample-login.md`

Always load the bundled DSL field specification before validating any DSL file. If the user provides another specification file, read it too and treat it as an explicit override only where it conflicts with the bundled default.

## When to Use

Use this skill when the task is:

- validate whether a `dsl.json` can restore the original UI accurately
- render DSL to HTML and inspect the visual result
- compare original image and reconstructed image
- produce a similarity score and a structured difference summary

Do not use this skill when the user wants to generate the DSL itself or when the user wants direct framework code generation.

## Required Inputs

Collect or confirm these inputs before running validation:

1. Original image path:
   usually `png`, but other image formats are acceptable.
2. DSL file path:
   the `dsl.json` that will be rendered.
3. DSL field specification path:
   default to `references/dsl字段说明.md`.
4. Output directory:
   default to the working directory unless the user specifies another path.

If the user asks for a strict or corrected JSON output, also prepare:

5. Validated JSON output path:
   default to `validated-dsl.json` in the working directory.

## Workflow

Follow this sequence:

1. Read `references/dsl字段说明.md` completely before validating or rendering any DSL.
2. Read the original image.
3. Read the `dsl.json`.
4. Validate the DSL structure against `references/dsl字段说明.md`:
   - confirm the file parses as a single JSON object
   - confirm the top-level shape, field names, node types, and field placement all match the specification
   - identify invalid, legacy, or framework-bound fields before rendering
5. If the user asks for a strict JSON file, write a separate `validated-dsl.json` that uses only fields allowed by the specification.
   - keep the source `dsl.json` unchanged unless the user explicitly asks to overwrite it
   - if a value must be preserved but cannot be represented directly, keep only allowed note fields such as `meta.info` or `codegen.notes`
6. Render the DSL into an HTML page:
   - preserve page size, hierarchy, positions, sizes, text, colors, borders, radius, and images as closely as practical
   - keep the rendering faithful to the DSL instead of compensating with ad hoc layout changes
   - if both original and validated JSON exist, state clearly which one is being rendered
7. Save the HTML result.
   Recommended file name: `rendered.html`
8. Open the HTML and capture a screenshot.
   Recommended file name: `rendered.png`
9. Compare the original image and the rendered screenshot visually.
10. Output a validation report.
   Recommended file name: `validation.md`

## Output Rules

The validation result should include all of these artifacts:

- strict JSON validation result in the report
- rendered HTML file
- rendered screenshot image
- AI-assessed similarity score
- concise summary of the overall match quality
- list of visible differences

When the user asks for strict JSON output, also include:

- `validated-dsl.json` that follows `references/dsl字段说明.md`

The report should focus on these dimensions:

- DSL schema compliance
- layout and alignment
- spacing and sizing
- text content and text position
- color and background fidelity
- border, radius, and shadow fidelity
- asset usage such as icons or background images

## Similarity Judgment Guidance

Use a pragmatic visual score from `0` to `100`.

- `90-100`: very close, only minor visual drift
- `75-89`: usable match, but visible differences exist
- `50-74`: partial match, key structure is right but several details are off
- `0-49`: poor match, major layout or styling errors

This score is an AI visual assessment, not a strict pixel-diff metric, unless the user explicitly asks for pixel-level comparison.

## Strict JSON Rules

When producing `validated-dsl.json`, satisfy all of these rules:

- Output a single JSON object.
- Use only fields, node types, and value shapes allowed by `references/dsl字段说明.md`.
- Keep business properties in `props`, coordinates and size in `layout`, visual styles in `style`, resource references in `asset` or `assets`, and generation hints in `codegen`.
- Prefer the normalized forms recommended by the specification, such as `Label` instead of legacy `Text`, `layout` instead of `rect`, `backgroundColor` instead of `background`, `fontColor` instead of generic `color`, and `inputMode: "password"` instead of `password: true`.
- Do not invent extra fields to preserve unsupported semantics.
- Do not bind the JSON to a concrete UI framework through fields such as `targetComponent`, `ngv2Widget`, or similar framework-specific keys.

## Suggested Report Shape

The report can use this structure:

```md
# DSL Validation Report

- Original image: ...
- DSL file: ...
- Rendered image: ...
- Similarity score: 86/100

## DSL Compliance

- JSON valid: yes or no
- Spec-compliant: yes or no
- Strict JSON output: `validated-dsl.json` or not requested

## Summary

One short paragraph.

## Key Differences

1. ...
2. ...
3. ...

## Notes

Limits, assumptions, or missing assets.
```

## Example Task Shape

Example input:

```text
Render sample/login/dsl.json to HTML, capture the rendered image, then compare it with sample/login/login.png and report similarity plus visible differences.
```

Expected behavior:

- read the specification before rendering
- validate the DSL against the specification before rendering
- render `sample/login/dsl.json` into HTML
- capture the rendered image
- compare it against `sample/login/login.png`
- produce the rendered image and a validation report
- write `validated-dsl.json` when the task asks for strict JSON output

## Common Mistakes

- skipping the HTML rendering step and comparing DSL text directly
- skipping the DSL schema check before rendering
- modifying the DSL structure during validation
- silently keeping invalid fields in a claimed strict JSON output
- outputting only a score without concrete differences
- ignoring missing assets or font differences in the final report
- treating this skill as framework-specific code generation
