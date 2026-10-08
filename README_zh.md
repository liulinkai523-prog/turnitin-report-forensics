# 🔍 Turnitin 报告真伪与元数据取证分析器 (Turnitin Report Forensics)

> **专为 Turnitin 查重报告与 AI 写作报告打造的纯前端、零上传法医级核验工具。**  
> 在浏览器本地深度比对时间链时序、PDF 底层元数据、提交编号合法性，精准识别 PS 篡改、伪造与再导出痕迹。

[English Documentation](README.md) | [🇨🇳 简体中文文档](README_zh.md)

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)
[![隐私保护: 100% 本地运算](https://img.shields.io/badge/Privacy-100%25%20Zero--Upload-emerald.svg)](#-隐私与安全模型)
[![官方在线体验](https://img.shields.io/badge/Official%20App-linkai.codes%2Fturnitin--check%2F-4cc4ea.svg)](https://linkai.codes/turnitin-check/)
[![零依赖纯前端](https://img.shields.io/badge/Tech-Vanilla%20JS%20%2B%20PDF.js-cyan.svg)](#-快速上手与部署)

---

## 🌐 官方在线体验（免安装即开即用）

无需安装任何软件或依赖，直接访问官方线上版本：  
👉 **[https://linkai.codes/turnitin-check/](https://linkai.codes/turnitin-check/)**

- **线上地址**：[https://linkai.codes/turnitin-check/](https://linkai.codes/turnitin-check/)
- **作者个人主页**：[https://linkai.codes](https://linkai.codes)
- **100% 隐私承诺**：所有 PDF 文件解析均在本地浏览器内存中完成，**绝不向任何服务器上传任何字节**。

---

## 🎯 背景与痛点：为什么需要这个工具？

在学术与留学圈中，Turnitin 原创性报告与 AI 写作报告是衡量学术诚信的标准。然而，市面上存在大量**篡改、伪造甚至黑产代查**的乱象：
- **篡改百分比**：中介使用 Adobe Acrobat、Canva 或修图工具强行涂改重复率或 AI 检出率；
- **拼凑报告**：将真实的官方封面与修改后的正文进行拼接；
- **伪造时间戳**：为了掩盖延期提交，伪造下载与提交时间；
- **生成虚假收据**：使用格式不合规的伪造提交编号（Submission ID）出具假回执。

传统的肉眼核对极易被视觉伪装蒙蔽。**Turnitin Report Forensics** 绕过视觉表象，直接下钻到 PDF 底层二进制流与 XMP 元数据，实现静态取证排查。

---

## ✨ 核心特性

- **🛡️ 纯前端零上传（Zero-Upload）**：完全基于 HTML5 `FileReader` 与本地内存 ArrayBuffer 解析，作业和隐私绝不泄露给任何第三方。
- **⏱️ 时间链时序一致性审计**：自动交叉比对 `CreationDate`（创建时间）、`ModDate`（修改时间）、正文标注提交时间及下载时间，精准捕捉时序倒置与二次编辑痕迹。
- **🧬 PDF 生成引擎与字体指纹**：检测底层 `Producer`（如正版流水线的 iText / Apache FOP），拦截 `Adobe Acrobat Pro`、`macOS Quartz` 等外部修改工具留下的指纹。
- **🆔 提交编号区间校验**：核对 9~10 位数字提交 ID 是否符合时间范围规律及字符规范。
- **📊 多文件交叉指纹比对**：支持同时拖入多份报告，一键比对文件结构指纹相似度、页数偏差与元数据漂移。
- **🌐 完善的中英双语界面**：提供完整中文与英文界面，一键平滑切换。

---

## 🔬 常见篡改特征与核验对照表

| 取证维度 | 官方报告正规特征 | 常见异常 / 篡改嫌疑指标 |
| :--- | :--- | :--- |
| **生成引擎 (Producer)** | Turnitin 专用导出管线 | 包含 `Adobe Acrobat Pro`、`macOS Quartz`、`Canva`、`Word` |
| **时间链时序** | `创建时间 ≈ 修改时间 ≈ 报告下载时间` | `修改时间` 显著晚于 `创建时间`（被外部软件二次保存） |
| **对象底层流** | 规范的线性化单一流结构 | 包含多个 `%EOF` 增量更新段（存在图层追加或覆盖） |
| **提交编号 (ID)** | 符合时序单调递增规律的整型数值 | 位数截断、非数字干扰符、或与生成年代严重脱节 |
| **页数比例关系** | 提交页数 + 2 (封面封底) = PDF 总页数 | 实际总页数与记录不符（可能存在抽页或插页） |

---

## 🚀 本地运行与二次部署

### 1. 本地免安装运行
本项目为纯静态单文件工程，无需 Node.js 或构建步骤：

```bash
git clone https://github.com/liulinkai523-prog/turnitin-report-forensics.git
cd turnitin-report-forensics

# 直接双击 index.html 在浏览器中打开，或者用任意静态服务器启动：
npx serve .
# 或 Python：
python -m http.server 8000
```

### 2. 部署到自己的 GitHub Pages
1. Fork 或推送本项目到你的 GitHub 仓库。
2. 进入仓库 **Settings** ➔ **Pages**。
3. 在 **Branch** 下选择 `main`，目录选 `/ (root)`，点击 **Save** 即可获得专属全球免费网址。

---

## 🔒 隐私与安全模型

> **学术调查中，学生手稿与评阅记录的机密性高于一切。**

1. **没有后端**：无任何 Node.js、Python、PHP 或 API 服务端；
2. **纯客户端运算**：文件通过浏览器原生能力在内存中直接解构；
3. **无数据埋点与追踪**：无 Cookie、无用户行为打点、无任何第三方分析 SDK。

---

## 👤 作者与主页

- **开发者**：Lucas (Linkai Liu)
- **个人网站**：[linkai.codes](https://linkai.codes)
- **在线体验入口**：[linkai.codes/turnitin-check/](https://linkai.codes/turnitin-check/)

如果这个工具帮助你辨别了真伪或避免了代查陷阱，欢迎给仓库点一个 **Star ⭐** 支持！

---

## ⚖️ 免责声明

本工具仅提供基于 PDF 静态元数据与常见篡改特征的启发式技术排查参考，不代表 Turnitin, LLC 的官方认证。如遇学术诚信争议，最终裁决请始终以学校教务处或机构内部 LMS 官方系统的在线提交记录为准。

---

## 📄 开源许可

本项目基于 [MIT License](LICENSE) 协议开源。欢迎提交 Issue、PR 或补充更多异常特征规则！
