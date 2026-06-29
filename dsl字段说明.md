# DSL 字段说明

## 1. 介绍

本 DSL 用于描述 UI 的结构、布局、样式、资源、状态和事件，作为图片或设计稿到 UI 代码之间的中间表示。

DSL 本身应保持框架无关，不直接绑定 C++ UI 框架、Web 框架或某个具体控件库。与具体框架相关的映射应放在 UI 框架说明、代码生成规则或转换器配置中。

顶层结构：

```json
{
  "version": "1.0",
  "meta": {},
  "page": {},
  "root": {},
  "assets": []
}
```

## 2. 顶层字段

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `version` | string | 是 | DSL 版本号，用于后续兼容升级。当前推荐 `"1.0"`。 |
| `meta` | object | 否 | DSL 来源、输入类型、说明信息等元数据。 |
| `page` | object | 是 | 页面级信息，例如页面 id、类名建议、画布尺寸、背景色。 |
| `root` | object | 是 | UI 根节点，所有控件从该节点递归描述。 |
| `assets` | array | 否 | 页面使用的资源清单，例如图片、SVG、字体等。 |

示例：

```json
{
  "version": "1.0",
  "meta": {
    "source": "login.png",
    "inputType": "png",
    "info": "登录页面"
  },
  "page": {
    "id": "login_page",
    "name": "登录页",
    "className": "LoginPage",
    "width": 450,
    "height": 560,
    "background": "#FFFFFF"
  },
  "root": {
    "id": "login_panel",
    "type": "Panel",
    "layout": {
      "x": 75,
      "y": 91,
      "width": 300,
      "height": 378
    },
    "children": []
  },
  "assets": []
}
```

### 2.1. version字段

DSL 版本号。

```json
{
  "version": "1.0"
}
```

规则：

- 必填。
- 使用字符串，不使用数字。
- 字段含义发生不兼容变化时，应升级版本号。

### 2.2. meta字段

`meta` 用于描述 DSL 来源和辅助信息，不参与 UI 渲染。

```json
{
  "meta": {
    "source": "login.png",
    "inputType": "png",
    "info": "登录页面",
    "generator": "ai",
    "reviewStatus": "draft"
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `source` | string | 否 | 原始输入文件，例如 `login.png`、`login.svg`。 |
| `inputType` | string | 否 | 输入类型，例如 `png`、`svg`、`mastergo`。 |
| `info` | string | 否 | 页面说明或补充描述。 |
| `generator` | string | 否 | 生成来源，例如 `ai`、`manual`、`converter`。 |
| `reviewStatus` | string | 否 | 审核状态，例如 `draft`、`approved`。 |

### 2.3. page字段

`page` 描述页面级信息，用于确定画布尺寸和页面身份。

```json
{
  "page": {
    "id": "login_page",
    "name": "登录页",
    "className": "LoginPage",
    "width": 450,
    "height": 560,
    "background": "#FFFFFF"
  }
}
```

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `id` | string | 是 | 页面唯一标识。 |
| `name` | string | 否 | 页面名称。 |
| `className` | string | 否 | 页面类名建议。该字段只是命名提示，不绑定具体框架。 |
| `width` | number | 是 | 页面宽度，单位为像素。 |
| `height` | number | 是 | 页面高度，单位为像素。 |
| `background` | string | 否 | 页面背景色，例如 `#FFFFFF`。 |

### 2.4. root字段

`root` 是 UI 根节点，本质上也是普通节点。

```json
{
  "root": {
    "id": "login_panel",
    "name": "登录面板",
    "type": "Panel",
    "layout": {
      "width": 300,
      "height": 380,
      "align": "center"
    },
    "style": {
      "backgroundImage": "images/bg_login.png",
      "radius": 8,
      "opacity": 0.9
    },
    "children": []
  }
}
```

规则：

- `root.type` 通常为 `Panel`、`ImagePanel` 或 `Group`。
- 所有 UI 控件都通过 `children` 递归挂载。
- `root` 可以有布局、样式、资源、状态和事件字段。

## 3. UI 节点通用字段

每个 UI 节点推荐使用以下结构：

```json
{
  "id": "loginButton",
  "name": "登录按钮",
  "type": "Button",
  "props": {},
  "layout": {},
  "style": {},
  "state": {},
  "events": {},
  "binding": {},
  "animation": {},
  "asset": {},
  "codegen": {},
  "children": []
}
```

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `id` | string | 是 | 节点唯一标识。 |
| `name` | string | 否 | 节点名称，通常来自图层名或人工命名。 |
| `type` | string | 是 | 控件类型，例如 `Panel`、`Label`、`Button`、`Edit`。 |
| `props` | object | 否 | 控件业务属性，例如文本、placeholder、输入模式。 |
| `layout` | object | 否 | 布局信息，例如坐标、尺寸、边距、对齐。 |
| `style` | object | 否 | 视觉样式，例如颜色、字体、边框、圆角。 |
| `state` | object | 否 | 状态样式，例如 hover、pressed、disabled。 |
| `events` | object | 否 | 事件绑定，例如 click、change、submit。 |
| `binding` | object | 否 | 数据绑定信息。 |
| `animation` | object | 否 | 动画信息。 |
| `asset` | object | 否 | 节点直接引用的资源。 |
| `codegen` | object | 否 | 代码生成提示和建议，不描述具体框架控件。 |
| `children` | array | 否 | 子节点列表。 |

### 3.1. type字段

`type` 表示 UI 节点的语义类型。

| 类型 | 说明 |
|---|---|
| `Panel` | 普通容器。 |
| `ImagePanel` | 带背景资源的容器。 |
| `Group` | 逻辑分组，不一定生成独立控件。 |
| `Label` | 静态文本。 |
| `Text` | 文本控件，兼容旧写法，推荐统一为 `Label`。 |
| `Image` | 图片控件。 |
| `Icon` | 图标控件，通常引用 SVG 或小图。 |
| `Button` | 按钮。 |
| `Edit` | 输入框。 |
| `Link` | 链接文本或链接按钮。 |
| `CheckBox` | 复选框。 |
| `RadioButton` | 单选框。 |
| `Slider` | 滑块。 |
| `ProgressBar` | 进度条。 |
| `ListView` | 列表。 |
| `ScrollView` | 滚动容器。 |
| `HBox` | 水平布局容器。 |
| `VBox` | 垂直布局容器。 |
| `Shape` | 基础图形，例如矩形、圆形。 |
| `Line` | 线段。 |
| `Arrow` | 箭头线。 |
| `Table` | 表格。 |

### 3.2. props字段

`props` 表示控件自身的业务属性，不放布局和视觉样式。

#### 3.2.1 Label / Text

```json
{
  "type": "Label",
  "props": {
    "text": "WELCOME"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `text` | string | 文本内容。 |

#### 3.2.2 Button / Link

```json
{
  "type": "Button",
  "props": {
    "text": "登 录",
    "icon": "assets/login.svg"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `text` | string | 按钮文本。 |
| `icon` | string | 按钮图标路径。 |

#### 3.2.3 Edit

```json
{
  "type": "Edit",
  "props": {
    "placeholder": "请输入用户名",
    "text": "vincent_chow",
    "icon": "icons/user.png",
    "inputMode": "normal"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `placeholder` | string | 占位文本。 |
| `text` | string | 默认文本。 |
| `icon` | string | 输入框图标。 |
| `inputMode` | string | 输入模式，例如 `normal`、`password`。 |
| `password` | boolean | 兼容字段，推荐转换为 `inputMode: "password"`。 |

#### 3.2.4 Image / Icon

```json
{
  "type": "Image",
  "props": {
    "src": "assets/login_card_bg.png",
    "assetId": "loginCardBg"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `src` | string | 图片路径。 |
| `assetId` | string | 对应 `assets` 中的资源 id。 |

### 3.3 layout字段

`layout` 描述位置和尺寸。

```json
{
  "layout": {
    "x": 75,
    "y": 91,
    "width": 300,
    "height": 378
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `x` | number | 相对父节点的 x 坐标。 |
| `y` | number | 相对父节点的 y 坐标。 |
| `width` | number | 宽度。 |
| `height` | number | 高度。 |
| `align` | string | 对齐方式，例如 `left`、`center`、`right`、`stretch`。 |
| `marginTop` | number | 上边距。 |
| `marginLeft` | number | 左边距。 |
| `marginRight` | number | 右边距。 |
| `marginBottom` | number | 下边距。 |
| `paddingLeft` | number | 左内边距。 |
| `paddingRight` | number | 右内边距。 |
| `paddingTop` | number | 上内边距。 |
| `paddingBottom` | number | 下内边距。 |
| `spacing` | number | 子节点之间的间距，常用于 `HBox` / `VBox`。 |

兼容字段：

```json
{
  "rect": [75, 91, 300, 378]
}
```

`rect` 等价于：

```json
{
  "layout": {
    "x": 75,
    "y": 91,
    "width": 300,
    "height": 378
  }
}
```

### 3.4 style字段

`style` 描述视觉样式。

```json
{
  "style": {
    "backgroundColor": "#3D9BFF",
    "fontSize": 16,
    "fontColor": "#FFFFFF",
    "radius": 4
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `backgroundColor` | string | 背景色。 |
| `background` | string | 兼容字段，推荐转换为 `backgroundColor`。 |
| `backgroundImage` | string | 背景图片路径。 |
| `color` | string | 兼容字段，文本或图标颜色，推荐按语义转换为 `fontColor`。 |
| `fontColor` | string | 文本颜色。 |
| `fontSize` | number | 字号。 |
| `fontWeight` | number | 字重，例如 300、400、500、700。 |
| `fontFamily` | string | 字体。 |
| `letterSpacing` | number | 字符间距。 |
| `align` | string | 文本对齐，例如 `left`、`center`、`right`。 |
| `borderColor` | string | 边框颜色。 |
| `borderWidth` | number | 边框宽度。 |
| `radius` | number | 圆角半径。 |
| `opacity` | number | 透明度，范围 0 到 1。 |
| `shadow` | object | 阴影信息。 |

阴影示例：

```json
{
  "style": {
    "shadow": {
      "offsetX": 0,
      "offsetY": 4,
      "blur": 12,
      "color": "rgba(0,0,0,0.18)"
    }
  }
}
```

### 3.5. asset

`asset` 描述单个节点直接依赖的资源。

```json
{
  "type": "ImagePanel",
  "asset": {
    "path": "assets/login_card_bg.png",
    "format": "png",
    "scaleMode": "center",
    "assetId": "loginCardBg"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `path` | string | 资源路径。 |
| `format` | string | 资源格式，例如 `png`、`svg`。 |
| `scaleMode` | string | 缩放模式，例如 `stretch`、`repeat`、`center`。 |
| `assetId` | string | 对应 `assets` 中的资源 id。 |

### 3.6. assets

`assets` 是页面级资源清单。

```json
{
  "assets": [
    {
      "id": "loginCardBg",
      "path": "assets/login_card_bg.png",
      "type": "image",
      "format": "png",
      "scaleMode": "center"
    }
  ]
}
```

| 字段 | 类型 | 必填 | 说明 |
|---|---|---|---|
| `id` | string | 是 | 资源唯一标识。 |
| `path` | string | 是 | 资源路径。 |
| `type` | string | 是 | 资源类型，例如 `image`、`icon`、`font`。 |
| `format` | string | 否 | 文件格式，例如 `png`、`svg`、`jpg`。 |
| `scaleMode` | string | 否 | 缩放模式，例如 `stretch`、`repeat`、`center`。 |

### 3.7 state字段

`state` 描述控件在不同状态下的样式。

```json
{
  "state": {
    "hover": {
      "backgroundColor": "#57A8FF"
    },
    "pressed": {
      "backgroundColor": "#2C7FE0"
    },
    "disabled": {
      "opacity": 0.5
    }
  }
}
```

| 状态 | 说明 |
|---|---|
| `normal` | 默认状态。 |
| `hover` | 鼠标悬停。 |
| `pressed` | 按下。 |
| `disabled` | 禁用。 |
| `focused` | 聚焦。 |
| `selected` | 选中。 |

### 3.8 events字段

`events` 描述控件事件和业务动作名称。

```json
{
  "events": {
    "click": "OnLoginClicked"
  }
}
```

| 事件 | 说明 |
|---|---|
| `click` | 点击。 |
| `doubleClick` | 双击。 |
| `change` | 值变化。 |
| `input` | 输入。 |
| `focus` | 获得焦点。 |
| `blur` | 失去焦点。 |
| `submit` | 提交。 |

### 3.9 binding字段

`binding` 描述节点属性和数据模型之间的绑定关系。

```json
{
  "binding": {
    "text": "vm.userName",
    "enabled": "vm.canLogin"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| 任意属性名 | string | 绑定表达式，例如 `vm.userName`。 |

### 3.10 animation字段

`animation` 描述进入、退出或状态切换动画。

```json
{
  "animation": {
    "enter": "fadeIn",
    "exit": "fadeOut"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `enter` | string | 进入动画名称。 |
| `exit` | string | 退出动画名称。 |
| `hover` | string | 悬停动画名称。 |
| `pressed` | string | 按下动画名称。 |

### 3.11 codegen字段

`codegen` 描述代码生成阶段可参考的提示和建议。该字段仍然是 DSL 的一部分，但必须保持框架无关，不能直接写入某个具体 UI 框架的控件类名。

```json
{
  "codegen": {
    "variableName": "m_loginButton",
    "semanticRole": "primaryAction",
    "reuse": false,
    "notes": "主按钮，生成代码时建议暴露为成员变量"
  }
}
```

| 字段 | 类型 | 说明 |
|---|---|---|
| `variableName` | string | 变量名建议，例如 `m_loginButton`。只是命名提示，不表示具体框架类型。 |
| `semanticRole` | string | 语义角色建议，例如 `primaryAction`、`container`、`title`、`decorativeIcon`。 |
| `reuse` | boolean | 是否建议复用已有组件或模板。 |
| `expose` | boolean | 是否建议对外暴露该节点，便于业务逻辑访问。 |
| `order` | number | 生成代码时的顺序建议。 |
| `notes` | string | 其他生成建议或注意事项。 |

不推荐在通用 DSL 中使用：

| 字段 | 原因 |
|---|---|
| `targetComponent` | 直接绑定目标 UI 框架控件类型，不属于框架无关 DSL。 |
| `ngv2Widget` | 直接绑定 NGV2 控件类型，应放在框架映射规则中。 |
| `codeGen.target` | 直接绑定目标控件类型，应迁移到框架说明或生成器配置。 |

如果需要从旧样例兼容这些字段，应由转换器读取旧字段，再在框架映射阶段处理，不建议保留在新的标准 DSL 中。

### 3.12 children字段

`children` 描述子控件。

```json
{
  "type": "Panel",
  "id": "login_panel",
  "children": [
    {
      "type": "Label",
      "id": "title",
      "props": {
        "text": "WELCOME"
      }
    }
  ]
}
```

规则：

- `children` 是数组。
- 子节点坐标默认相对父节点。
- 容器类控件通常有 `children`，叶子节点可以没有。

## 4. 推荐完整示例

```json
{
  "version": "1.0",
  "meta": {
    "source": "login.png",
    "inputType": "png",
    "info": "登录页面",
    "reviewStatus": "approved"
  },
  "page": {
    "id": "login_page",
    "name": "登录页",
    "className": "LoginPage",
    "width": 450,
    "height": 560,
    "background": "#FFFFFF"
  },
  "root": {
    "id": "login_panel",
    "name": "登录面板",
    "type": "ImagePanel",
    "layout": {
      "x": 75,
      "y": 91,
      "width": 300,
      "height": 378
    },
    "asset": {
      "path": "assets/login_card_bg.png",
      "format": "png",
      "scaleMode": "center"
    },
    "codegen": {
      "variableName": "m_loginPanel",
      "semanticRole": "container",
      "expose": true,
      "notes": "登录页面主容器"
    },
    "children": [
      {
        "id": "titleText",
        "type": "Label",
        "props": {
          "text": "WELCOME"
        },
        "layout": {
          "x": 58,
          "y": 43,
          "width": 184,
          "height": 40
        },
        "style": {
          "fontSize": 30,
          "letterSpacing": 4,
          "fontColor": "#FFFFFF",
          "align": "center"
        },
        "codegen": {
          "variableName": "m_titleText",
          "semanticRole": "title"
        }
      }
    ]
  },
  "assets": [
    {
      "id": "loginCardBg",
      "path": "assets/login_card_bg.png",
      "type": "image",
      "format": "png"
    }
  ]
}
```
