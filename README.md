# DLSS5-6x-AMD-OptiScaler

夸克网盘整合包：
链接：https://pan.quark.cn/s/a066f57d7905?pwd=qW4m
提取码：qW4m

## 📖 使用教程

[使用教程](https://space.bilibili.com/173069391?spm_id_from=333.1007.0.0)
v5.1版本对齐政宗SAMA 项目v1.9.3-alpha https://github.com/TheAutomatic/dlss-5-amd-project/releases  上游项目，文件自行在其项目中下载。
使用方法，去到上面这个项目把项目文件v1.9.3-ZIP文件下载下来，然后在把V5.1版本下载下来，将v5.1项目拷贝覆盖到v1.9.3-ZIP文件中，然后用nm.bat启动，选择文件夹中的【无权限游戏可注入在此程序中然后把全部文件移动到游戏主目录】选择注入DLL注入方式，注入。
然后把全部文件拷贝到游戏根目录中即可，后面的步骤和正常的OptiScaler使用方式就差不多了。可以去我主页看使用教程。

## 📖 项目简介

**DLSS5-6x-AMD-OptiScaler** 面向 AMD GPU 用户，主要研究和实践在 AMD 硬件环境下通过 **OptiScaler** 使用 DLSS 相关功能、安装方法、测试结果以及兼容性信息。

项目重点关注：

- AMD GPU + OptiScaler 的实际运行效果
- DLSS 5/6x 相关实验性配置
- Frame Generation 测试
- 游戏兼容性测试
- 参数调试与配置整理
- 性能和画质对比
- 实际使用过程中出现的问题及排查方法

> **说明：** 本项目是基于OptiScaler，独立的社区项目，与 AMD、NVIDIA 或任何游戏发行商没有官方关联，也不代表任何相关厂商的立场或支持。

本项目基于https://github.com/optiscaler/OptiScaler  项目 

基于https://github.com/Vodkaman23/DLSS-NR-UE5-Opti-DLL  项目  
基于https://github.com/MatheusGViana/dlss-5-amd-project  项目  
基于https://github.com/danielblnc/DLSS-NR-on-AMD  项目  
基于https://github.com/TheAutomatic/dlss-5-amd-project 国内UP@政宗SAMA 项目,他的主页https://space.bilibili.com/291088/dynamic

特别鸣谢以上项目。
> 基于以上项目开发的多帧生成兼容项目，兼容了多种帧生成方式。
---

## ✨ 项目特点

- AMD GPU 环境下的 DLSS 相关技术实践
- 基于 OptiScaler 的配置和实验
- DLSS 5/6x 相关实验性配置整理
- Frame Generation 实际测试
- 不同游戏的兼容性测试
- 性能与画质对比
- 提供 Windows / Linux 相关安装脚本
- 持续整理测试结果、配置和问题反馈
- 欢迎社区提交 Issue、测试结果和使用反馈

---

## 🗂️ 项目结构

当前仓库主要包含：

```text
DLSS5-6x-AMD-OptiScaler/
├── .github/
├── OptiScaler/
│   └── OptiScaler/
├── external/
├── images/
├── OptiScaler.ini
├── OptiScaler.sln
├── Streamlined_fetcher_windows.bat
├── setup_windows.bat
├── setup_linux.sh
└── README.md
```

### 主要目录 / 文件

| 文件 / 目录 | 说明 |
|---|---|
| `OptiScaler/` | OptiScaler 相关项目内容 |
| `external/` | 第三方依赖及相关资源 |
| `images/` | 项目图片、测试截图及文档素材 |
| `OptiScaler.ini` | OptiScaler 配置文件 |
| `OptiScaler.sln` | Visual Studio 解决方案 |
| `setup_windows.bat` | Windows 安装 / 配置脚本 |
| `setup_linux.sh` | Linux 安装 / 配置脚本 |
| `Streamlined_fetcher_windows.bat` | Windows 相关资源获取 / 部署脚本 |

---

## 🖥️ 测试环境

项目以实际硬件和游戏环境测试为基础。

由于 DLSS、OptiScaler、Frame Generation 以及相关配置会受到 GPU、驱动、Windows、游戏版本和具体渲染路径影响，因此测试结果可能存在差异。

### 测试项目

后续将持续记录：

| GPU | 驱动版本 | Windows | 游戏 | 分辨率 | OptiScaler | DLSS / FG | 状态 |
|---|---|---|---|---|---|---|---|
| 持续补充 | 持续补充 | 持续补充 | 持续补充 | 持续补充 | 持续补充 | 持续补充 | 测试中 |

> 测试结果基于实际测试环境，不同 GPU、驱动版本、Windows 版本、OptiScaler 版本和游戏版本可能存在差异。

---

## 📦 安装

请优先使用 GitHub **Releases** 中发布的版本。

基本流程：

1. 下载对应版本的 Release 安装包。
2. 解压到合适的位置。
3. 根据项目说明完成安装或部署。
4. 根据游戏和硬件环境修改相关配置。
5. 启动游戏并观察实际效果。
6. 如果出现问题，请记录 GPU、驱动、游戏版本、OptiScaler 配置和日志后提交 Issue。

> 建议在修改配置前备份原始游戏文件和配置文件。

---

## ⚙️ 配置说明

项目中的配置主要用于 AMD GPU + OptiScaler 环境下的实验和测试。

### `OptiScaler.ini`

配置内容可能会随着 OptiScaler 版本以及项目测试结果持续调整。

建议：

- 不要直接复制其他 GPU / 游戏的配置并假设一定有效。
- 修改参数前保存原始配置。
- 每次只修改少量参数，方便定位问题。
- 出现闪烁、拖影、重影、黑屏、崩溃等问题时，优先恢复最近一次可用配置。
- 记录 GPU、驱动、游戏版本和 OptiScaler 版本，方便复现问题。

> **注意：** 不同游戏使用的渲染路径和输入数据可能不同，因此不存在适用于所有游戏的通用配置。

---

## 🎮 游戏兼容性测试

项目将持续收集 AMD GPU + OptiScaler + DLSS 相关功能在不同游戏中的实际运行情况。

推荐使用以下格式记录测试：

```text
GPU:
Driver:
Windows:
Game:
Game Version:
OptiScaler Version:
Resolution:
Upscaling:
Frame Generation:
Configuration:
Result:
Issues:
Logs / Screenshots:
```

### 兼容性状态

| 状态 | 含义 |
|---|---|
| ✅ Working | 基本正常运行 |
| 🟡 Testing | 正在测试 |
| ⚠️ Issues | 可以运行，但存在问题 |
| ❌ Not Working | 当前环境无法正常运行 |
| ❓ Unknown | 暂无足够测试数据 |

---


## 🎮 实际测试：生化危机 9

### AMD Radeon RX 7900 XTX

本项目基于 **OptiScaler** 开发，并在 AMD Radeon RX 7900 XTX 上进行了《生化危机 9》的实际测试。

目前记录的测试结果如下：

### 4K 测试

| 配置 | 帧率 |
|---|---:|
| 4K + 6 倍帧生成 | **476 FPS** |
| 4K + 6 倍帧生成 + DLSS5 | **78 FPS** |

### 2K 测试

| 配置 | 帧率 |
|---|---:|
| 2K + 6 倍帧生成 | **889 FPS** |
| 2K + 6 倍帧生成 + DLSS5 | **148 FPS** |

### 测试结果

| 分辨率 | 6 倍帧生成 | 开启 DLSS5 |
|---|---:|---:|
| **4K** | **476 FPS** | **78 FPS** |
| **2K** | **889 FPS** | **148 FPS** |

从当前测试结果来看，在 **RX 7900 XTX + OptiScaler** 环境下，《生化危机 9》可以实现较高的 6 倍帧生成帧率；开启 DLSS5 后，帧率会明显降低，但仍保持较高的实际帧率。

> ⚠️ 以上数据为当前测试环境下的实际测试结果，仅代表本项目当前配置和测试环境下的表现。不同 GPU、驱动版本、游戏版本、OptiScaler 版本以及配置可能产生不同结果。

> **测试条件说明：** 当前记录仅包含上述分辨率、6 倍帧生成及 DLSS5 开启后的帧率数据。未提供的驱动版本、游戏版本、具体场景、基础渲染帧率等信息暂不作推测。

## 📊 性能测试

项目不仅关注是否能够运行，也会记录实际性能表现。

后续测试将尽可能记录：

- 原生渲染 FPS
- Upscaling FPS
- Frame Generation FPS
- 1% Low
- GPU 占用率
- GPU 显存占用
- CPU 占用率
- 输入延迟
- 不同配置之间的性能差异

建议测试时保持：

- 相同游戏版本
- 相同分辨率
- 相同画质设置
- 相同场景
- 相同驱动
- 相同测试时间和方法

这样可以提高不同配置之间的可比性。

---

## 🖼️ 画质对比

项目会记录不同配置下的画质差异，包括：

- 原生画质
- DLSS / Upscaling
- Frame Generation
- DLSS 相关实验配置
- AMD GPU 下的实际表现

重点观察：

- 细节保留
- 远处物体
- 植被
- 反射
- 透明效果
- UI
- 闪烁
- 拖影
- 重影
- 锐度变化

截图和测试素材将持续整理到 `images/` 目录。

---

## ⚠️ 已知问题

由于项目属于实验性研究，目前可能存在：

- 不同游戏之间兼容性差异
- 不同 GPU / 驱动之间表现差异
- Frame Generation 稳定性差异
- 闪烁、拖影、重影等画面问题
- 黑屏、崩溃或无法正常初始化
- 某些游戏需要额外配置
- 不同 OptiScaler 版本可能产生不同结果

如果遇到问题，请优先确认：

1. GPU 型号
2. AMD 驱动版本
3. Windows 版本
4. 游戏版本
5. OptiScaler 版本
6. 当前配置
7. 日志和截图

---

## 🛠️ 维护工作

作为项目维护者，我持续进行以下工作：

- AMD GPU + OptiScaler 环境测试
- DLSS 相关配置研究和整理
- DLSS 5/6x 实验性配置测试
- Frame Generation 测试
- 不同游戏兼容性测试
- 性能与画质对比
- 《生化危机 9》RX 7900 XTX 实际性能测试
- 参数调整和问题排查
- 安装 / 部署脚本维护
- README 和项目文档维护
- 整理测试结果和社区反馈
- 发布项目版本并维护 Release

项目会根据实际测试结果持续更新。

---

## 🤖 Codex 使用计划

计划使用 Codex 辅助项目开发和维护，包括：

- 分析项目代码和配置
- 排查运行问题
- 代码开发与重构
- Bug 修复
- 配置文件分析
- 编写和优化辅助工具
- 自动化部分测试流程
- 分析 GitHub Issues 和社区反馈
- 维护 README 和项目文档
- 辅助 Release 和版本维护

Codex 的使用重点是减少重复性的维护工作，并提高测试、分析和文档整理效率。

---

## 💳 API Credits 使用计划

如果获得 API Credits，主要用于本项目的持续开发、测试和维护，包括：

- 自动分析测试日志
- 配置参数分析
- 测试数据整理
- 兼容性结果整理
- Issue 分析和分类
- 自动化测试辅助
- 文档生成和维护
- Release 信息整理
- 配置验证和维护工具
- 辅助开发项目工具

目标是让 AMD + OptiScaler 的兼容性测试和研究过程更加自动化、可重复和易于社区参与。

---

## 🐛 Issue / Feedback

欢迎提交：

- **Bug Report**
- **游戏兼容性测试结果**
- **GPU / 驱动测试结果**
- **配置问题**
- **性能测试结果**
- **画质对比**
- **改进建议**

提交问题时，建议尽可能提供：

```text
GPU:
Driver:
Windows:
Game:
Game Version:
OptiScaler Version:
Resolution:
Upscaling:
Frame Generation:
Configuration:
Problem:
Logs:
Screenshots:
```

> 请不要在 Issue、日志或截图中公开 API Key、密码、账号信息或其他敏感数据。

---

## 🔐 安全

项目涉及配置文件、动态库、安装脚本和第三方组件。

后续将持续关注：

- DLL 加载
- 文件处理
- 配置解析
- 第三方依赖
- 安装与更新流程
- 脚本执行安全性

如果发现潜在安全问题，请避免在公开 Issue 中发布敏感信息。

---

## 📌 后续计划

- [x] 建立 AMD + OptiScaler DLSS 5/6x 实验项目
- [x] 整理基础配置
- [x] 提供 Windows / Linux 相关脚本
- [x] 建立基础测试和文档体系
- [ ] 测试更多 AMD GPU
- [ ] 测试更多游戏
- [ ] 完善 GPU / 游戏兼容性列表
- [ ] 增加更多性能测试数据
- [ ] 增加更多画质对比
- [ ] 完善自动化测试
- [ ] 完善配置验证工具
- [ ] 持续修复问题并发布新版本

---

## 📜 License

本项目中的代码和文档请以仓库中的 `LICENSE` 文件为准。

项目中使用的 OptiScaler、第三方库、动态库、SDK、游戏文件及其他组件，其版权和许可证仍归各自权利人所有。

请在使用、修改、分发相关组件前确认其对应的许可证要求。

---

## ⚠️ Disclaimer

**DLSS5-6x-AMD-OptiScaler** 是独立的社区研究项目。

本项目与 AMD、NVIDIA、OptiScaler、任何游戏发行商或其他第三方厂商没有官方关联、授权或背书。

项目中的配置和测试结果仅代表特定测试环境下的实验结果，不保证在所有硬件、驱动、游戏和系统环境中正常工作。

用户应自行确认并遵守所使用的软件、游戏、驱动以及第三方组件的许可证和服务条款。

---

## ⭐ 支持项目

如果这个项目对你有帮助，可以通过 GitHub **Star** 支持项目。

欢迎提交 Issue、测试结果和使用反馈，帮助项目持续完善。

感谢所有参与测试、反馈问题和提供技术支持的社区成员。

