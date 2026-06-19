如果目标是：

```text
MasterGo/Figma
      ↓
  UI DSL
      ↓
Code Generator
      ↓
自研 C++ UI
```

那么 DSL 最好不要绑定某个具体框架（React/Flutter），而是设计成一个**通用 UI 描述协议**。

我比较推荐参考：

```text
Flutter Widget Tree
+
Compose
+
Figma Node
+
游戏UI(UMG/UI Toolkit)
```

最终统一为：

```json
{
  "version": "1.0",
  "type": "Button",
  "id": "btn_login",

  "props": {},
  "layout": {},
  "style": {},

  "binding": {},
  "events": {},

  "children": []
}
```

---

# 一级字段设计

## Root

```json
{
  "version": "1.0",
  "name": "LoginPage",
  "root": {}
}
```

完整页面：

```json
{
  "version": "1.0",
  "name": "LoginPage",
  "root": {
    ...
  }
}
```

---

# Node字段

## 必须字段

```json
{
  "type": "Button",
  "id": "btn_login"
}
```

### type

控件类型：

```text
Panel
Label
Image
Button
Edit
CheckBox
List
Tree
Tab
ScrollView
```

---

### id

唯一标识

```json
{
  "id": "btn_login"
}
```

用于：

```cpp
FindWidget("btn_login");
```

---

# props

业务属性。

## Button

```json
{
  "props": {
    "text": "登录"
  }
}
```

---

## Image

```json
{
  "props": {
    "src": "images/logo.png"
  }
}
```

---

## Edit

```json
{
  "props": {
    "placeholder": "请输入账号"
  }
}
```

---

原则：

```text
props
=
控件特有属性
```

不要放布局和样式。

---

# layout

负责布局。

## 通用

```json
{
  "layout": {
    "x": 0,
    "y": 0,

    "width": 300,
    "height": 40
  }
}
```

---

## Anchor

类似 Unreal UMG

```json
{
  "layout": {
    "anchor": {
      "left": 0,
      "top": 0,
      "right": 1,
      "bottom": 0
    }
  }
}
```

---

## Flex

类似 Flutter

```json
{
  "layout": {
    "flex": 1
  }
}
```

---

## Align

```json
{
  "layout": {
    "align": "center"
  }
}
```

支持：

```text
start
center
end
stretch
```

---

# style

视觉样式。

## 尺寸

```json
{
  "style": {
    "minWidth": 100,
    "maxWidth": 500
  }
}
```

---

## 背景

```json
{
  "style": {
    "backgroundColor": "#409EFF"
  }
}
```

---

## 边框

```json
{
  "style": {
    "border": {
      "width": 1,
      "color": "#DDDDDD"
    }
  }
}
```

---

## 圆角

```json
{
  "style": {
    "radius": 8
  }
}
```

---

## 阴影

```json
{
  "style": {
    "shadow": {
      "offsetX": 0,
      "offsetY": 2,
      "blur": 8
    }
  }
}
```

---

## 文本

```json
{
  "style": {
    "fontSize": 16,
    "fontWeight": 500,
    "fontColor": "#333333"
  }
}
```

---

## 透明度

```json
{
  "style": {
    "opacity": 0.8
  }
}
```

---

# binding

MVVM绑定。

```json
{
  "binding": {
    "text": "vm.userName"
  }
}
```

生成：

```cpp
edit->Bind(VM::UserName);
```

---

# events

事件。

```json
{
  "events": {
    "click": "OnLoginClick"
  }
}
```

生成：

```cpp
btn->OnClick.Bind(
    this,
    &LoginPage::OnLoginClick
);
```

---

# animation

动画独立。

```json
{
  "animation": {
    "enter": "fadeIn",
    "exit": "fadeOut"
  }
}
```

---

# state

状态样式。

类似 CSS。

```json
{
  "state": {
    "normal": {
      "backgroundColor": "#409EFF"
    },

    "hover": {
      "backgroundColor": "#66B1FF"
    },

    "pressed": {
      "backgroundColor": "#337ECC"
    }
  }
}
```

---

# children

所有子控件。

```json
{
  "children": [
    {},
    {},
    {}
  ]
}
```

---

# 推荐的最终结构

这是我认为最适合 C++ UI 引擎长期演进的结构：

```json
{
  "type": "Button",

  "id": "btn_login",

  "name": "LoginButton",

  "props": {},

  "layout": {},

  "style": {},

  "state": {},

  "binding": {},

  "events": {},

  "animation": {},

  "children": []
}
```

---

# 再进一步（大型项目）

如果未来要支持：

* MasterGo
* Figma
* Sketch
* UMG
* Unity UI Toolkit

建议采用下面这一层抽象：

```json
{
  "meta": {},

  "widget": {
    "type": "Button",
    "props": {}
  },

  "layout": {},

  "style": {},

  "children": []
}
```

也就是借鉴 Flutter 的思想：

```text
Widget
  + Layout
  + Style
  + Children
```

核心字段尽量控制在：

```text
type
id
props
layout
style
state
binding
events
children
```

这 9 个字段基本足够覆盖 95% 的桌面端、Web 端、游戏 UI 和移动端 UI 场景，而且生成 C++ 代码时结构最清晰。
