---
name: dsl_render
description: Use when rendering a dsl.json file into a specific UI framework, especially when the framework mapping rules are provided in a separate reference file and the user wants framework-specific output instead of a framework-agnostic DSL.
---

# dsl_render

## Overview

Render a `dsl.json` file into code or project files for a specific UI framework.

The DSL stays framework-agnostic. Framework-specific decisions must come from the separate framework guide file instead of being invented ad hoc.

In this project, the default bundled references are:

- DSL field specification: `references/dsl字段说明.md`
- Target framework guide: `references/特定框架的使用说明.md`
- Bundled example DSL: `sample/login/dsl.json`
- Bundled sample note: `references/sample-login.md`

Always read both reference files before rendering. If the user provides another DSL specification or another framework guide, read those files too and treat them as explicit overrides where they conflict with the bundled defaults.

## When to Use

Use this skill when the task is:

- render `dsl.json` into a concrete UI framework
- map DSL node types and styles into framework widgets, properties, and layout code
- generate framework-specific files from a framework-agnostic DSL
- follow a separate framework instruction file while rendering

Do not use this skill when the user wants to create the DSL from an image or when the user wants to compare rendered output against an original screenshot.

## Required Inputs

Collect or confirm these inputs before rendering:

1. DSL file path:
   the `dsl.json` that will be rendered.
2. DSL field specification path:
   default to `references/dsl字段说明.md`.
3. Framework guide path:
   default to `references/特定框架的使用说明.md`.
4. Output path:
   default to the working directory unless the user specifies another location.

## Workflow

Follow this sequence:

1. Read `references/dsl字段说明.md` completely before interpreting the DSL.
2. Read `references/特定框架的使用说明.md` completely before generating framework output.
3. Read the input `dsl.json`.
4. Validate the DSL structure against the field specification:
   - confirm the file parses as a single JSON object
   - confirm the top-level shape and node fields are compatible with the DSL specification
   - identify unsupported, legacy, or framework-bound fields before rendering
5. Read `references/sample-login.md` when the task is similar to the bundled sample or when a concrete internal example is useful.
6. Map the DSL into framework output according to the framework guide:
   - map node types to framework widgets
   - map layout fields to framework layout constructs
   - map style fields to framework styling constructs
   - map assets, events, and codegen hints only as allowed by the framework guide
7. Write the framework-specific output files.
8. Validate that generated text files are syntactically valid for the chosen output format when the framework guide defines a validation or build command.

## Output Rules

The result must satisfy all of these rules:

- Keep the source `dsl.json` unchanged unless the user explicitly asks to rewrite it.
- Use only the semantics present in the DSL plus the mapping rules present in the framework guide.
- Do not move framework-specific widget names back into the DSL.
- Keep business properties derived from `props`, layout from `layout`, visual styling from `style`, resources from `asset` or `assets`, and generation hints from `codegen`.
- Preserve the DSL hierarchy unless the framework guide explicitly requires a structural adapter layer.
- If the framework guide does not define a required mapping, do not invent hidden behavior. Emit a visible TODO, note, or limitation in the generated output or alongside it.
- Keep output file paths and names consistent with the framework guide.

## Review Standard

Before finishing, check:

- both reference files were read
- the DSL parses successfully
- every rendered widget can be traced back to DSL nodes
- framework-specific constructs come from `references/特定框架的使用说明.md`
- unsupported mappings are called out explicitly
- generated output files are present

## Example Task Shape

Example input:

```text
根据 sample/login/dsl.json，按照 references/dsl字段说明.md 和 references/特定框架的使用说明.md 渲染出目标 UI 框架代码
```

Expected behavior:

- read the DSL specification
- read the framework guide
- inspect `sample/login/dsl.json`
- render framework-specific output from the DSL
- keep the DSL unchanged
- report any unresolved framework mapping gaps explicitly

## Common Mistakes

- skipping the framework guide and inventing a mapping
- modifying the DSL to inject framework-specific widget names
- putting layout semantics into style-only constructs
- silently dropping unsupported DSL nodes or properties
- generating output that cannot be traced back to the DSL hierarchy
