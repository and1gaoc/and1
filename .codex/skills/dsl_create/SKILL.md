---
name: dsl_create
description: Use when converting external UI inputs such as PNG or SVG into a structured dsl.json, especially when the user asks for screenshot-to-DSL generation, strict compliance with dsl字段说明.md, or a framework-agnostic JSON intermediate before code generation.
---

# dsl_create

## Overview

Generate a `dsl.json` file from an external UI input source such as `png` or `svg`.

The output must follow the project's DSL field specification instead of inventing a new schema.

In this project, the default bundled references are:

- DSL field specification: `references/dsl字段说明.md`
- Bundled example image: `sample/login/login.png`
- Bundled example DSL: `sample/login/dsl.json`
- Bundled sample note: `references/sample-login.md`

Always load the bundled DSL field specification before generating or editing a DSL file. If the user provides another specification file, read it too and treat it as an explicit override only where it conflicts with the bundled default.

## When to Use

Use this skill when the task is:

- generate `dsl.json` from `png`, `svg`, or similar visual input
- convert a screenshot or exported design image into the project's DSL
- produce a framework-agnostic UI JSON before later code generation
- follow `dsl字段说明.md` strictly while building a UI description file

Do not use this skill when the user wants final C++ UI code directly and does not want the DSL step.

## Required Inputs

Collect or confirm these inputs before generating the file:

1. Source file path:
   `png`, `svg`, or other visual input path.
2. DSL field specification path:
   default to the bundled `references/dsl字段说明.md`.
3. Example reference paths:
   default to the bundled `sample/login` example in this skill.
4. Output path:
   default to `dsl.json` in the working directory unless the user specifies another location.

## Workflow

Follow this sequence:

1. Read `references/dsl字段说明.md` completely before generating or editing any `dsl.json`.
2. Read the example image and example DSL JSON if they exist.
   Read `references/sample-login.md` when the task is similar to the bundled login example or when you want a concrete example of how this project expects the DSL to be organized.
3. Inspect the input source visually or structurally:
   - For `png`: infer page size, containers, text, inputs, buttons, images, links, and major layout relationships.
   - For `svg`: prefer structural information from the file itself when available, then infer UI semantics.
4. Build the DSL using only the field vocabulary, top-level shape, node fields, node types, and placement rules defined in the specification.
5. Keep the DSL framework-agnostic:
   - `codegen` may contain naming or generation hints.
   - `codegen` must not bind the DSL to a concrete UI framework widget type.
6. Write the result to `dsl.json`.
7. Validate that the output is valid JSON.

## Output Rules

The result must satisfy all of these rules:

- Output a single JSON object.
- Use the top-level shape required by the specification.
- Do not invent fields, node types, event names, or codegen keys not present in `references/dsl字段说明.md`.
- If a visual detail cannot be represented by a specified field, omit the invented field and mention the limitation separately, or use only allowed note fields such as `meta.info` or `codegen.notes`.
- Keep field placement correct:
  - business properties in `props`
  - coordinates and size in `layout`
  - colors, fonts, borders, radius in `style`
  - generation hints in `codegen`
- Use stable, readable `id` values.
- Prefer semantic node types such as `Panel`, `Label`, `Button`, `Edit`, `Image`, `Link`.
- Add `assets` entries when the UI depends on external resources.
- If a resource is inferred but not yet extracted, keep the path as a placeholder and make that clear in `meta.info` or `codegen.notes`.
- Prefer the normalized forms recommended by the specification, such as `Label` instead of legacy `Text`, `layout` instead of `rect`, `backgroundColor` instead of `background`, `fontColor` instead of generic `color`, and `inputMode: "password"` instead of `password: true`.

## Review Standard

Before finishing, check:

- page width and height are present when the specification requires them
- `root` exists and all visible nodes are reachable through `children`
- text content is preserved
- coordinates are relative to the parent container
- every emitted field is allowed by `references/dsl字段说明.md`
- the JSON parses successfully

## Example Task Shape

Example input:

```text
根据 sample/login/login.png，按照 references/dsl字段说明.md 生成 dsl.json
```

Expected behavior:

- read the specification file
- inspect the sample image
- use `sample/login/dsl.json` as a structure reference if helpful
- use `references/sample-login.md` as the canonical bundled sample note for this skill
- generate a valid `dsl.json`

## Common Mistakes

- Inventing fields not present in the specification
- Putting layout data into `style`
- Putting framework widget types into `codegen`
- Treating the whole image as one background node when the UI can be decomposed
- Forgetting to validate the final JSON
