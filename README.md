# EOS 主题皮肤仓库

EOS 桌面端的官方与社区皮肤合集。应用内「设置 → 外观 → 皮肤 → 从 GitHub 导入」填入本仓库地址即可一键安装，无需手动下载打包。

- **仓库地址**：`eosaios/eos-themes`
- **皮肤协议版本**：schemaVersion 1
- **贡献方式**：Fork → 在 `skins/` 下按目录规范新增皮肤 → 提 PR

---

## 一、皮肤是什么

皮肤 = 一组设计 token（颜色变量）+ 可选壁纸，改变整个工作台的配色与观感。它与「亮/暗偏好」正交：每个皮肤提供浅色与深色两套 token，应用按当前偏好取用。

## 二、目录结构

仓库中每个皮肤一个目录，**目录名必须等于皮肤 id**：

```
skins/
└── eos.sakura/              # 皮肤 id（小写字母/数字/./-，3-128 字符）
    └── 1.0.0/               # 语义化版本（严格 semver：MAJOR.MINOR.PATCH）
        ├── theme.json       # 清单（必需）
        ├── tokens/
        │   ├── light.json   # 浅色 token（必需）
        │   └── dark.json    # 深色 token（可选，缺省回落浅色）
        └── assets/          # 预览图 / 壁纸（可选）
            └── preview.png
```

## 三、theme.json 清单

```json
{
  "schemaVersion": 1,
  "id": "eos.sakura",
  "name": "樱花",
  "description": "柔粉暖调：浅色是樱瓣与晨雾，深色是暮紫与花影。",
  "version": "1.0.0",
  "author": "eosaios",
  "license": "MIT",
  "appearance": {
    "light": "tokens/light.json",
    "dark": "tokens/dark.json"
  },
  "preview": "assets/preview.png",
  "wallpaper": "assets/wallpaper.png",
  "i18n": { "zh-CN": "assets/README.zh.md" }
}
```

字段说明：

| 字段 | 必需 | 说明 |
|---|---|---|
| `schemaVersion` | ✅ | 协议版本，当前为 `1` |
| `id` | ✅ | 唯一标识；建议 `eos.` / `com.xxx.` 前缀防撞名 |
| `name` | ✅ | 显示名 |
| `description` | ✅ | 一句话介绍（卡片 tooltip） |
| `version` | ✅ | 严格 semver；应用按最高版本择优 |
| `author` | ✅ | 作者名 |
| `license` | ✅ | 许可证 |
| `appearance.light` | ✅ | 浅色 token 文件路径 |
| `appearance.dark` | ⬜ | 深色 token 文件路径 |
| `preview` | ⬜ | 卡片预览图（png/jpg/webp） |
| `wallpaper` | ⬜ | 壁纸图（v1 单图亮暗共用） |
| `i18n` | ⬜ | 各语言说明文档 |

## 四、Token（核心）

`tokens/light.json` 与 `dark.json` 是扁平的 `键: 颜色` JSON。键名映射为应用内 CSS 变量（`camelCase` → `--tw-kebab-case`，如 `surfaceSecondary` → `--tw-surface-secondary`）。

### Level A：核心表面（12 键，两套变体都**必填**）

| 键 | 语义 |
|---|---|
| `canvas` | 画布底色（侧栏/窗口底） |
| `surface` | 主表面（舞台/卡片） |
| `surfaceSecondary` | 次表面（输入框/嵌套卡） |
| `overlay1` / `overlay2` | 悬浮/按压叠层 |
| `border1` / `border2` / `border3` | 三档发丝线（弱/中/强） |
| `text` / `text2` / `text3` / `text4` | 四级文字色 |

### Level B：品牌与语义色（9 键，可缺省，缺省回落内置默认）

| 键 | 语义 |
|---|---|
| `brand` / `brandHover` / `brandActive` | 品牌色三态 |
| `brandSoft` / `brandBorder` | 品牌淡底 / 描边 |
| `info` / `success` / `danger` / `warning` | 语义色 |

### 颜色格式

只接受 `#RRGGBB`、`#RGB`、`rgba(r, g, b, a)`、`#RRGGBBAA`。

### 配色建议

- 深色变体的 `border*` / `overlay*` 用**浅色半透明**（如 `rgba(233,226,199,0.12)`），浅色变体用**深色半透明**
- `text` 与 `surface` 的对比度保证可读（≥ 12:1 更稳），`text4` 是最弱层级（占位符）
- `brandSoft` ≈ 品牌色 12% 透明度，`brandBorder` ≈ 34%
- 先定 `canvas`（最深的底）与 `surface`（内容面），再派生其余

### 完整示例（浅色）

```json
{
  "canvas": "#f4ecef",
  "surface": "#fbf7f9",
  "surfaceSecondary": "#efe6ea",
  "overlay1": "rgba(112,64,82,0.06)",
  "overlay2": "rgba(112,64,82,0.10)",
  "border1": "rgba(112,64,82,0.12)",
  "border2": "rgba(112,64,82,0.18)",
  "border3": "rgba(112,64,82,0.36)",
  "text": "#3c2b33",
  "text2": "#5f4a55",
  "text3": "#8c7580",
  "text4": "#b39aa4",
  "brand": "#c26d8d",
  "brandHover": "#cf819e",
  "brandActive": "#a95b7a",
  "brandSoft": "rgba(194,109,141,0.12)",
  "brandBorder": "rgba(194,109,141,0.34)",
  "info": "#9a8fb5",
  "success": "#8fae8b",
  "danger": "#c05f6a",
  "warning": "#d9b476"
}
```

## 五、壁纸（可选）

`wallpaper` 指向一张 png/jpg/webp，铺在窗口底（壳层「薄纱」表面下透出）。建议：

- 横构图，≥ 1920×1080，≤ 4MB
- 中右部适当留内容（舞台区薄纱下能隐约透出图案）
- 单文件 ≤ 8MB、整包 ≤ 20MB、条目数 ≤ 200（应用侧硬校验）

## 六、本地测试

1. 把皮肤版本目录打成 zip（**zip 根直接是 `theme.json` 等文件**，不要套外层目录）
2. EOS 桌面端 → 设置 → 外观 → 皮肤 → 「导入皮肤包」，或直接把 zip / 解压后的文件夹拖进窗口
3. 卡片上点击即时预览，保存后生效

打包命令（在版本目录内）：

```bash
cd skins/eos.sakura/1.0.0
zip -r ../eos.sakura-1.0.0.eos-theme . -x ".*"
```

## 七、发布到本仓库

1. Fork 本仓库
2. `skins/<你的id>/<版本>/` 按上述规范添加
3. PR 标题：`skin: <id> <版本>`
4. 合并后，应用内「从 GitHub 导入」即可安装

> 校验清单：目录名 == id == theme.json.id；version 是严格 semver；两套 token 齐全 Level A；声明的文件都存在；颜色格式合法；整包 ≤ 20MB。

## 八、License

本仓库皮肤默认 MIT（各皮肤 `theme.json.license` 可另行声明）。
