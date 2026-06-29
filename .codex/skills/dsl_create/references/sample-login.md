# Sample: login

This is the bundled sample for the `dsl_create` skill in this project.

Use this sample when:

- the user asks for a DSL from a PNG form-like page
- the user wants an example of the expected `dsl.json` structure
- the current input resembles a login panel, card layout, or simple account form

## Source files

- Image: `../sample/login/login.png`
- DSL: `../sample/login/dsl.json`
- Field specification: `dsl字段说明.md`

## Example prompt

```text
根据 sample/login/login.png，按照 references/dsl字段说明.md 生成 dsl.json
```

## What this sample demonstrates

- top-level structure uses `version + meta + page + root + assets`
- root node is a page container
- the visible login card is modeled as a child `ImagePanel`
- text goes into `props`
- coordinates and sizes go into `layout`
- colors, border, font, and radius go into `style`
- code-generation hints stay inside `codegen` and remain framework-agnostic
- inferred resources such as icons and panel background are listed in `assets`

## Canonical output shape

The full sample output lives at:

- `../sample/login/dsl.json`

Key structure:

```json
{
  "version": "1.0",
  "meta": {},
  "page": {
    "id": "login_page",
    "width": 450,
    "height": 560
  },
  "root": {
    "id": "page_root",
    "type": "Panel",
    "children": [
      {
        "id": "login_card",
        "type": "ImagePanel",
        "children": [
          {
            "id": "title_text",
            "type": "Label"
          },
          {
            "id": "username_edit",
            "type": "Edit"
          },
          {
            "id": "password_edit",
            "type": "Edit"
          },
          {
            "id": "login_button",
            "type": "Button"
          },
          {
            "id": "footer_links",
            "type": "HBox"
          }
        ]
      }
    ]
  },
  "assets": []
}
```

## Interpretation notes

- Do not collapse the login page into a single full-image node.
- Keep the card, title, inputs, button, and footer links as separate semantic nodes.
- If a resource does not yet exist on disk, it can still appear in the DSL as an inferred placeholder path.
