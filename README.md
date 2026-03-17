# PicForge - 图锻

> 纯前端 AI 图片后处理工具集 —— 去水印、切网格、编辑块，一站搞定。

专为 [Confirmo](https://confirmo.love/) 自定义形象素材打造。感谢 [yetone](https://github.com/yetone/confirmo-releases) 提供如此优秀的插件！

## 特性一览

| 功能 | 说明 |
|------|------|
| 🧹 **Gemini 水印去除** | 反向 Alpha 混合算法，自适应定位 + 多尺寸模板 + 双通道检测，导入即去除 |
| 🔲 **网格切分** | 自定义目标尺寸与块大小，一键切分为均匀网格 |
| ✏️ **块编辑** | 橡皮擦 / 画笔 / 移动，支持缩放平移与批量变换 |
| 📐 **九宫格切图** | 1:1 裁切 + 3×3 分割，拖拽调整，单块或批量导出 |

## 快速开始

浏览器直接打开 `index.html`，无需安装、无需构建。

```
1. 上传图片（PNG / JPG / WebP，拖拽或点击）
2. 自动去除 Gemini 水印
3. 设置目标尺寸和块大小 → 应用切分
4. 点击块进行预览编辑，可批量应用变换
5. 导出 PNG
```

九宫格切图请打开 `grid9.html`。

## 页面

| 页面 | 文件 | 用途 |
|------|------|------|
| 图片调整工具 | `index.html` | 去水印 + 网格切分 + 块编辑 + 导出 |
| 九宫格切图 | `grid9.html` | 1:1 裁切 + 3×3 九宫格导出 |

## 水印去除原理

Gemini 水印近似白色 Logo 的 Alpha 叠加：

```
watermarked = α × logo + (1 - α) × original
```

去除流程：

1. **Alpha Map 计算** — 从内置背景捕获图（48×48 / 96×96）提取 alpha 遮罩
2. **自适应搜索** — 粗搜（8px 网格）→ 精细搜索（像素级），在右下区域定位水印
3. **双通道检测** — NCC 空间相关性 + Sobel 梯度相关性综合评分
4. **Gain 自动校准** — 遍历 14 组增益值，选择最优去除效果
5. **子像素精修** — ±0.5px 偏移 + ±1% 缩放，双线性插值对齐
6. **反向还原** — `original = (watermarked - α × 255) / (1 - α)`

> 基于 [gemini-watermark-remover](https://github.com/GargantuaX/gemini-watermark-remover)。
> `index.html` 中的 `BG_48_SRC` / `BG_96_SRC` 须与源库 `src/assets/bg_48.png` / `bg_96.png` 保持一致。

## 技术栈

- 纯 HTML + CSS + JavaScript，零依赖、零构建、单文件部署
- HTML5 Canvas API / File System Access API
- 字体：[Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) + [Manrope](https://fonts.google.com/specimen/Manrope)
- 图标：[Remix Icon](https://remixicon.com/)
- 浅色 glassmorphism 主题，CSS 变量驱动

## License

MIT
