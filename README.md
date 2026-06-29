# 手机端原型绘制

一个用于 Codex 的手机端 H5 原型绘制 skill。它可以根据参考截图还原移动端静态 HTML 原型，也可以根据业务规则、字段清单、状态流转和交互要求生成手机端原型。

## 适用场景

- 根据手机页面截图制作静态 HTML 原型
- 根据业务规则生成移动端 H5 页面
- 在一个 HTML 文件中串联多页面原型
- 为原型生成可拖拽的交互提示层
- 根据真实可交互元素自动标记点击/输入位置
- 处理弹窗、外部链接确认、模拟浏览器页等移动端业务链路
- 生成页面业务串联图，展示页面之间如何跳转

## 安装方式

将本仓库复制到你的 Codex skills 目录：

```bash
git clone https://github.com/liyangxu219318-bot/mobile-prototype-drawing.git ~/.codex/skills/mobile-prototype-drawing
```

Windows 用户可以克隆到：

```powershell
git clone https://github.com/liyangxu219318-bot/mobile-prototype-drawing.git "$env:USERPROFILE\.codex\skills\mobile-prototype-drawing"
```

重启 Codex 后，即可通过 `$mobile-prototype-drawing` 或“手机端原型绘制”相关请求触发。

## 使用示例

```text
使用 $mobile-prototype-drawing，根据这张截图做一个手机端静态 HTML 原型。
```

```text
使用 $mobile-prototype-drawing，根据以下业务规则生成一个手机端审批原型：
1. 首页展示客户信息和待办入口
2. 点击入口进入问卷列表
3. 点击填写时先弹出外部链接确认
4. 确认后进入第三方表单页面
```

```text
使用 $mobile-prototype-drawing，基于当前 index.html 生成一张业务串联图，连线要从具体按钮接出。
```

## 核心约定

- 默认只生成或维护一个 `index.html`。
- 多页面原型使用 hash 路由，例如 `#home`、`#list`、`#confirm`。
- 预览截图和流程图统一放入 `preview-screenshots/`。
- 默认不在每次原型修改后单独生成页面截图；最终生成业务串联图时再统一截取所需页面状态。
- 交互提示只基于真实 HTML 交互元素生成，不标记纯视觉按钮。
- 弹窗打开时，交互提示只标记弹窗内真实交互点。
- 模拟浏览器壳里的纯展示元素应保持静态，例如无实际动作的 `...` 不应写成链接。
- 交互蒙版应锚定页面坐标并裁切边界，避免滚动错位或产生横向滚动。
- 业务串联图优先使用橙色直角线，并从具体触发按钮接出。

## 文件结构

```text
mobile-prototype-drawing/
├── SKILL.md
├── README.md
├── LICENSE
├── agents/
│   └── openai.yaml
└── references/
    └── mobile-prototype-rules.md
```

## 许可

本项目使用 MIT License，详见 [LICENSE](LICENSE)。
