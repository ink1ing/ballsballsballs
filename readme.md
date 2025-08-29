# 经典旋转六边形: 物理+AI+vibecoding (本项目100%由codex cli with gpt5 high完成)

## 项目简介
- 演示站点，点我点我！！https://ink1ing.github.io/ballsballsballs/
- 这是一个基于 Canvas 2D 的刚体/碰撞可视化小实验：一个持续旋转的正六边形容器，内含多枚小球，受重力、弹性、摩擦与自旋耦合影响运动，并与壁面及其他小球发生碰撞。支持鼠标/触控拖拽，采用弹簧力模型生成线性与角向反馈（扭矩）。

## 亮点特性
- 物理细节：
  - 六边形壁面速度来自刚体旋转 v = ω × r，碰撞计算使用接触点相对速度（含自旋）。
  - 球-壁、球-球均使用脉冲式解算；切向采用库仑摩擦上限；更新角动量以体现自旋变化。
  - 数值稳定手段：步长上限、位置穿透回退（slop）、空气/角阻尼可控。
- 交互体验：
  - 局部抓取点（rLocal）+ 弹簧-阻尼模型，既能移动球心也能施加扭矩；启用 Pointer Capture 保持拖拽稳定。
- 画面与性能：
  - 支持高 DPR 渲染；背景网格与渐变填充；在窗口尺寸变化时自适应画布并保持稳定。

## 素材来源
- Logo 图标：来自 @lobehub/icons（lobe-icons）项目的静态 PNG 资源，通过 jsDelivr CDN 引用。
  - OpenAI: packages/static-png/dark/openai.png
  - Claude: packages/static-png/dark/claude-color.png
  - DeepSeek: packages/static-png/dark/deepseek-color.png（带回退）
  - Gemini: packages/static-png/dark/gemini-color.png
- 代码：纯原生 HTML + Canvas + 原生事件，无第三方运行时依赖。

## 两个版本的区别
- origin.html（精简原始版）
  - 特性：四个 Logo 小球（OpenAI / Claude / DeepSeek / Gemini）、默认物理参数（高弹性）、拖拽交互、六边形随视口自适应半径。
  - 去除：所有 UI 控件、预设、主题/配色切换、“推推推”交互、点击跳转逻辑、顶部标题/介绍。
  - 图标：采用 @lobehub/icons 的静态 PNG，图标随小球自旋旋转；未加载时使用渐变球占位。
- index.html（增强演示版）
  - 控件与交互：底部控制栏（固定在底部）、重力/弹性/拖拽强度可调，预设（地球/月球/太空），长按“推”加速旋转。
  - 体验增强：调整窗口大小时暂停物理步进；单击 Logo 打开对应站点；主题（浅/深）切换；底部栏适配安全区。
  - 几何与稳定性：六边形半径固定为像素值；窗口缩放时仅平移中心，小球相对六边形的向量保持不变。

## 集成与运行（建议）
- 静态站点：通过 HTTP(S) 提供（避免 file:// 导致的跨域问题），将 `index.html` 或 `origin.html` 放入站点根目录即可访问。
- 资源稳定：
  - 建议将图标文件本地化托管，或将 CDN 地址固定到特定 tag/commit，避免上游更新引起样式波动。
  - 如保留 CDN，请检查站点 CSP（img-src）与跨域设置（Image.crossOrigin='anonymous'）。
- 触控体验：为父容器/画布增加 `touch-action: none; user-select: none;` 可减少滚动/选中干扰。

## 重构注意事项
- 更新顺序：保持 step 流程为「施力/阻尼 → 积分位置/角度 → 碰撞解算（壁/球）」，避免改序导致能量异常。
- 时间基准：限制 dt 上限；在页面隐藏/恢复（visibilitychange）时暂停/重置时间，避免后台累积导致突变。
- 画布与 DPR：
  - 仅通过设置 canvas 宽高与 2D transform 处理 DPR，避免对 canvas 使用 CSS transform/zoom 造成坐标失真。
  - resize 时重新计算中心与半径；若采用固定半径（增强版策略），请同步平移球心以保持相对位置。
- 事件管理：组件化集成时确保挂载/卸载清理事件与取消 RAF，避免重复循环与内存泄漏。
- 资源管理：
  - 图标建议预加载；提供失败回退（如彩色 → 单色）。
  - 若迁移到 React/Next 并使用组件形式的 @lobehub/icons（如 `<OpenAI size={56} />`），请用独立渲染层，不与 Canvas 物理绘制混用。
- 碰撞/摩擦：
  - 保留法向/切向分解、库仑摩擦上限与角动量更新；务必限制最大冲量/最大力，防止数值爆炸。

---

- 打开方式：直接访问 `origin.html`（原始简洁）或 `index.html`（交互增强）。
- 如需替换图标或调整球数，修改 `initBalls()` 的初始化布局与 ICON 映射即可。

