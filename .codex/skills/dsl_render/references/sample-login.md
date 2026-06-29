# Sample: login render

This is the bundled sample for the `dsl_render` skill in this project.

Use this sample when:

- the user wants a concrete example of rendering DSL into a UI framework
- the current input resembles a login form or card-style page
- the framework guide is being filled or verified against a simple DSL

## Source files

- DSL: `../sample/login/dsl.json`
- DSL specification: `dsl字段说明.md`
- Framework guide: `特定框架的使用说明.md`

## Example prompt

```text
根据 sample/login/dsl.json，按照 references/dsl字段说明.md 和 references/特定框架的使用说明.md 渲染出目标 UI 框架代码
```

## What this sample demonstrates

- the renderer reads DSL and framework rules separately
- the DSL remains framework-agnostic
- framework-specific widgets and file layout come only from the framework guide
- unresolved mappings should be called out explicitly instead of being hidden
