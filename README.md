# MWS UI

MWS 设计体系的组件调试台（Component Playground）。单文件、零依赖，打开浏览器即可实时调参、观察动效，并导出可直接使用的组件代码与 JSON 配置。

- **当前版本**：v0.1.0（见 [CHANGELOG.md](CHANGELOG.md)）
- **形态**：单文件 HTML —— `index.html` 内含全部结构、样式与逻辑，无构建步骤、无第三方依赖
- **已实现**：ComboBox（下拉选择器）、Slider（滑动条）
- **规划中**：Button、Checkbox、单项切换、多项切换、颜色选择

## 特性

### 通用

- 左侧吸顶预览舞台 + 右侧分组控制面板，调参即时生效
- 顶栏组件切换器，在同一页面内切换不同组件的工作区
- 「状态直达」：一键锁定的交互态（折叠、展开、悬停、按下、键盘候选、已选中、禁用），便于逐帧比对视觉
- 配置导出 / 导入 JSON，导出配置可下载为 `.json`，也可从本地文件读取
- 导出独立的 HTML 代码片段，支持复制与下载 `.html`
- 浅色主题，中文界面；窄屏（≤ 920px）自动切换为单列布局

### ComboBox

- 选项内容编辑：增删选项（2–8 项）、恢复默认文案，实时显示选项计数
- 选择状态面板：当前值、索引与提交次数、候选索引、事件日志、禁用项开关
- 展开动效：展开原点（选中项中间 / 从顶部）、4 组缓动预设 + 可拖拽的贝塞尔曲线编辑器、动画时长（120–1200 ms）
- 面板重定位：整块平移 / 框内滚动、独立时长与曲线、实时位移读数
- 伸展空间：自动 / 向下 / 向上、预留边距、控件位置、自动避让，并可展开检测上下可用空间
- 外观：圆角与圆角模式（直角 / 圆弧 / 连续）、4 档面板阴影、5 个颜色字段（含色板与 HEX 输入）、字体预设与自定义字体族、字号、字距、字重
- 键盘操作与 ARIA：触发器支持 ↑/↓/Enter/Space 展开，展开后方向键移动候选，Enter/Space 提交，Esc 关闭，Tab 关闭并移出焦点

### Slider

- 数值：最小值 / 最大值 / 步长 / 当前值，步长实时吸附
- 状态直达与锁定，便于固定某个交互态观察样式
- 外观：滑块直径（12–30 px）、轨道高度（2–10 px）、数值气泡、刻度、禁用态、强调色 / 轨道色 / 滑块面色
- 指针拖拽 + 键盘调节（←/→/↑/↓ 单步、PageUp/PageDown 十倍步长、Home/End 到端点），带 `role="slider"` 与 `aria-valuenow` 等属性

## 快速开始

克隆后直接用浏览器打开即可，无需安装依赖或编译：

```bash
git clone <repo-url>
cd MWSUI
open index.html          # macOS
# start index.html       # Windows
# xdg-open index.html    # Linux
```

如浏览器对 `file://` 下的剪贴板或下载有限制，可启动一个本地静态服务：

```bash
python3 -m http.server 8000
# 浏览器访问 http://localhost:8000
```

## 界面说明

| 区域 | 说明 |
| --- | --- |
| 顶栏 | 品牌标识、组件切换器、版本号 |
| 预览舞台（左） | 当前组件的实时预览，吸顶显示；ComboBox 的控件位置可上下调节 |
| 控制面板（右） | 按「内容 / 选择状态 / 状态直达 / 动效 / 面板重定位 / 伸展空间 / 外观 / 导出 / 配置」分组 |
| 导出 | 生成自包含的组件 HTML 代码，可复制或下载 |
| 配置 | 导出或导入完整配置 JSON，支持下载与读取本地文件 |

## 配置结构

「配置 → 导出配置」生成的 JSON 结构如下（`v` 为配置协议版本，目前为 `1`）：

```json
{
  "app": "MWS UI",
  "v": 1,
  "view": "combobox",
  "combo": {
    "names": ["浅色", "深色", "跟随系统", "高对比度", "自定义"],
    "idx": 2,
    "disabled": false,
    "disabledIdx": 3,
    "easing": 0,
    "custom": [0.16, 1, 0.3, 1],
    "mode": "center",
    "radius": 8,
    "radiusMode": "round",
    "duration": 420,
    "theme": {
      "accent": "#ff6666",
      "sel": "#FFE0E0",
      "text": "#2C2C2A",
      "panel": "#FFFFFF",
      "border": "#8A8A85"
    },
    "font": { "preset": 0, "custom": "", "size": 13, "weight": 400, "spacing": 0 },
    "move": { "on": true, "duration": 320, "curve": "follow", "mode": "slide" },
    "space": { "on": true, "mode": "auto", "margin": 12 },
    "shadow": "sm",
    "stagePos": 50
  },
  "slider": {
    "min": 0,
    "max": 100,
    "step": 1,
    "value": 40,
    "accent": "#FF6666",
    "rest": "#D6D4CE",
    "face": "#FFFFFF",
    "size": 20,
    "track": 4,
    "bubble": false,
    "ticks": false,
    "disabled": false
  }
}
```

### ComboBox 字段

| 字段 | 类型 | 取值 / 范围 | 说明 |
| --- | --- | --- | --- |
| `names` | `string[]` | 2–8 项，单条 ≤ 24 字符 | 选项文案 |
| `idx` | `number` | `0 … names.length - 1` | 当前选中索引 |
| `disabled` | `boolean` | — | 是否启用禁用项 |
| `disabledIdx` | `number` | 同上 | 被禁用的选项索引 |
| `easing` | `number` | `0` expo · `1` spring · `2` ease-in-out · `3` 自定义 | 缓动预设索引 |
| `custom` | `number[4]` | 控制点坐标 | 自定义 `cubic-bezier` |
| `mode` | `string` | `center` / `top` | 展开原点 |
| `radius` | `number` | `0 – 24` | 面板圆角（px） |
| `radiusMode` | `string` | `square` / `round` / `smooth` | 直角 / 圆弧 / 连续圆角 |
| `duration` | `number` | `120 – 1200` | 展开动画时长（ms） |
| `theme` | `object` | 5 个 HEX 颜色 | 强调色、选中底色、主文本、面板底色、边框色 |
| `font.preset` | `number` | `0 – 4` | 字体预设索引（系统 UI / 无衬线 / 等宽 / 衬线 / 自定义） |
| `font.custom` | `string` | — | 自定义 `font-family` |
| `font.size` | `number` | `11 – 18` | 字号（px） |
| `font.weight` | `number` | `300 – 700` | 字重 |
| `font.spacing` | `number` | `-0.5 – 1.5` | 字距（px） |
| `move.on` | `boolean` | — | 是否启用面板重定位 |
| `move.duration` | `number` | `120 – 1200` | 重定位时长（ms） |
| `move.curve` | `string` | `follow` / `spring` / `inout` / `linear` | 重定位曲线 |
| `move.mode` | `string` | `slide` / `scroll` | 整块平移 / 框内滚动 |
| `space.on` | `boolean` | — | 是否自动避让溢出 |
| `space.mode` | `string` | `auto` / `down` / `up` | 伸展方式 |
| `space.margin` | `number` | `0 – 72` | 预留边距（px） |
| `shadow` | `string` | `none` / `sm` / `md` / `lg` | 面板阴影档位 |
| `stagePos` | `number` | `0 – 100` | 控件在预览舞台中的垂直位置（%） |

### Slider 字段

| 字段 | 类型 | 取值 / 范围 | 说明 |
| --- | --- | --- | --- |
| `min` / `max` | `number` | `max > min` | 取值范围 |
| `step` | `number` | `> 0` | 步长 |
| `value` | `number` | `min – max` | 当前值 |
| `accent` | `string` | HEX | 强调色（填充 / 滑块描边 / 气泡） |
| `rest` | `string` | HEX | 轨道底色 |
| `face` | `string` | HEX | 滑块面色 |
| `size` | `number` | `12 – 30` | 滑块直径（px） |
| `track` | `number` | `2 – 10` | 轨道高度（px） |
| `bubble` / `ticks` / `disabled` | `boolean` | — | 数值气泡 / 刻度 / 禁用 |

## 目录结构

```
MWSUI/
├── index.html      # 全部结构、样式与逻辑（单文件）
├── README.md       # 项目说明
├── CHANGELOG.md    # 版本变更记录
└── .gitignore
```

## 开发说明

- **无构建链路**：直接编辑 `index.html` 并刷新浏览器即可，没有热更新或编译步骤。
- **代码分区**：样式与脚本以 `/* ==== 分区名 ==== */` 注释组织，依次为设计令牌、基础布局、组件样式、组件工厂、ComboBox、Slider、导出与配置。
- **可复用工厂**：`mwsSelect`（下拉）与 `mwsRange`（滑动条）既是页面控件，也是被调试对象的实现，新增组件时可按同样方式封装。
- **主题与参数**：颜色通过 CSS 变量（`--color-*`、`--mws-accent`）与运行时注入的 `--sel-*` / `--sld-*` / `--c-*` 覆盖，避免样式硬编码。
- **渐进增强**：`corner-shape: squircle` 在不支持的浏览器上回退为普通圆角。
- **提交与版本**：提交信息遵循 [Conventional Commits](https://www.conventionalcommits.org/)，版本号遵循 [SemVer](https://semver.org/lang/zh-CN/)，每次发布同步更新 `CHANGELOG.md`。

## 浏览器支持

面向现代浏览器（Chrome / Edge / Safari / Firefox 最新两个大版本）。页面不依赖网络资源，可离线使用。

## 作者

MogroWang

## 许可证

暂未指定。如需开源发布，请补充 `LICENSE` 文件。
