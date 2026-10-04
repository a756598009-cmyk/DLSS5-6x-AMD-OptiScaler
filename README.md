[**中文**](README.md) | [English](README_EN.md)
# DLSS5-30x-6x-AMD-OptiScaler

夸克网盘整合包：
链接：https://pan.quark.cn/s/a066f57d7905?pwd=qW4m
提取码：qW4m

## 📖 使用教程

### 🔗 教程入口

- 视频教程：[柠檬有多萌萌胧未可知 · 哔哩哔哩个人空间](https://space.bilibili.com/173069391?spm_id_from=333.1007.0.0)
v9.6版本对齐政宗SAMA 项目v1.10.0 https://github.com/TheAutomatic/dlss-5-amd-project/releases 上游项目，文件自行在其项目中下载。使用方法，去到上面这个项目把项目文件v1.10.0-ZIP文件下载下来，然后在把v9.6版本下载下来，将v9.6项目拷贝覆盖到v1.10.0文件中覆盖，然后用nm.bat启动，点击选择文件夹，选择你要注入的Dll方式对应数字，回车键，输入1，回车，输入2，回车，输入2，回车,输入1，回车。 然后把全部文件拷贝到游戏根目录中，在运行开关30倍帧生成，按提示输入对应数字选择你要开启的多帧生成方式，后面的步骤和正常的OptiScaler使用方式就差不多了。【此版本加入了4种 xess 30倍帧生成】可以去我主页看使用教程。
RX 6000（RDNA2）：
增加了对RX 6000系列（RDNA2）显卡的支持
RDNA2显卡需要安装AMD HIP 7.2运行时才能正常工作，可以从AMD官网下载（https://www.amd.com/en/developer/resources/rocm-hub/hip-sdk.html)
---

### 🚀 多帧生成核心架构

本项目将 AMD + OptiScaler 的实验方案整理为统一的 **FG Input → XeFG Output** 架构：

```text
游戏渲染帧
    │
    ├── FG Input（帧生成输入源）
    │      ├─ OptiFG
    │      ├─ FSR 3.1 FG
    │      ├─ FSR 3.0 FG
    │      └─ DLSSG
    │
    ▼
OptiScaler
    │
    ▼
XeFG
    │
    ▼
多帧生成 / Multi-Frame Generation
```

### 🧩 当前整理的 4 种方案

## 🚀 XeFG 30X 核心技术重构

本项目对原有帧生成链路进行了重新构建，重点完成了 **XeFG 30X 多帧生成链路的重构与实现**。

### 30X 帧生成链路

重新构建后的 XeFG 30X 链路，以 **1/30 插入三十帧** 为核心设计目标，将单次基础帧作为输入，并扩展为连续的多帧生成输出。

```text
基础渲染帧
    ↓
┌──────────────────────────────┐
│          XeFG 30X            │
│                              │
│  1/30 → Frame 01             │
│  2/30 → Frame 02             │
│  3/30 → Frame 03             │
│  ...                         │
│  30/30 → Frame 30            │
└──────────────────────────────┘
    ↓
连续多帧输出
```

与传统的 2X / 3X / 4X 帧生成不同，**30X 的核心并不是简单提高一个固定倍数，而是重新构建多帧插入链路，使单个基础渲染帧可以对应最多 30 个生成帧位置。**

### 理论 30X 输出能力

> **1 个基础渲染帧 + 30 个生成帧位置 = 理论最高 30X 多帧生成倍率**

在理论条件下，如果 GPU 的计算性能、显存容量、显存带宽以及整个生成链路的吞吐能力足够，便可以实现接近理论 30X 的输出能力。

实际运行倍率会受到 GPU 性能、显存、Frame Time、输出分辨率、游戏负载以及 FG 输入链路等因素影响。

### 30X 技术定位

XeFG 30X 的重点并非单纯提高一个倍率数字，而是重新构建：

```text
FG Input
   ↓
Multi-Frame Generation
   ↓
FG Output
```

目前已经形成：

```text
OptiFG
   ↓
XeFG 30X
```

以及：

```text
FSR 3.0 FG ─┐
FSR 3.1 FG ─┤
DLSSG ──────┤
             ↓
         XeFG 30X
             ↓
        多帧生成输出
```

因此，XeFG 30X 可以作为不同 FG 输入源之后的统一多帧生成输出链路。

> **注意：30X 为理论最大生成倍率，实际可达到的倍率与最终 FPS 会受到 GPU 性能、游戏负载、输入 FG 类型、分辨率以及当前运行环境等因素限制。**

## 🚀 目前已经实现的 5 种 FG / MFG 路径

> **以下 5 种 FG 链路目前已经在本项目中实现。**  
> 具体可用性取决于游戏、OptiScaler 版本、FG 输入类型以及当前配置。

| # | FG Input | FG Output | 当前实现状态 | 条件 |
|---|---|---|---|---|
| **1** | **OptiFG** | **XeFG 30X** | ✅ **已实现** | 不要求游戏原生 FG |
| **2** | **FSR 3.1 FG** | **XeFG 30X** | ✅ **已实现** | 需要游戏自带 FSR 3.1 FG |
| **3** | **FSR 3.0 FG** | **XeFG 30X** | ✅ **已实现** | 需要游戏自带 FSR 3.0 FG |
| **4** | **DLSSG** | **XeFG 30X** | ✅ **已实现** | 需要游戏自带 DLSS FG |
| **5** | **DLSSG** | **DLSSG 6X** | ✅ **已实现** | 需要游戏自带 DLSS FG |

### 已实现的 FG 链路

本项目当前已经实现以下 5 种帧生成（Frame Generation, FG）/ 多帧生成（Multi-Frame Generation, MFG）组合路径：

```text
① OptiFG → XeFG 30X
② FSR 3.1 FG → XeFG 30X
③ FSR 3.0 FG → XeFG 30X
④ DLSSG → XeFG 30X
⑤ DLSSG → DLSSG 6X
```

**OptiFG 输入链路：**

```text
游戏渲染
   ↓
OptiFG
   ↓
XeFG 30X
   ↓
多帧生成输出
```

特点：**不要求游戏原生提供 FG**。

**游戏原生 FG 输入链路：**

```text
FSR 3.1 FG ─┐
FSR 3.0 FG ─┤
DLSSG ──────┤ → XeFG 30X
             │
DLSSG ──────┴ → DLSSG 6X
```

这类方案要求游戏本身提供对应的 **原生 Frame Generation**，并在游戏中启用相应 FG 功能。

### ⭐ 方案 1：OptiFG + XeFG 30X

**推荐使用场景：**

- 游戏本身没有原生 Frame Generation；
- 游戏的原生 FG 无法正常工作；
- 希望通过 OptiScaler 的实验性 OptiFG 建立 FG 输入，再交给 XeFG 进行多帧生成。

```text
游戏
 ↓
OptiScaler / OptiFG
 ↓
XeFG
 ↓
多帧生成
```

**注意：**

- OptiFG 当前主要面向 DX12。
- OptiFG 属于实验性方案，不等同于游戏原生 FG。
- 部分游戏可能出现 HUD 重影、UI 重复、闪烁或其他插帧伪影。
- 如果游戏已经拥有可用的原生 FG，原则上优先使用原生 FG 输入。

---

### ⭐ 方案 2：FSR 3.1 FG + XeFG 30X

针对**游戏原生支持 FSR 3.1 Frame Generation** 的方案。

```text
游戏原生 FSR 3.1 FG
          ↓
      OptiScaler
          ↓
        XeFG
          ↓
      多帧生成
```

**使用条件：**

1. 游戏本身支持 FSR 3.1 Frame Generation。
2. 进入游戏设置，开启 **FSR 3.1 FG / Frame Generation**。
3. 启动 OptiScaler。
4. 将 FG Input 设置为对应的 FSR FG 输入。
5. 将 FG Output 设置为 **XeFG**。
6. 根据 XeFG 多帧配置设置插帧倍率。
7. 重启游戏后测试。

> 游戏已经提供原生 FSR FG 输入时，优先使用原生输入，不需要再用 OptiFG 强行创建输入。

---

### ⭐ 方案 3：FSR 3.0 FG + XeFG 30X

针对仍然使用 **FSR 3.0 Frame Generation** 的游戏。

```text
游戏原生 FSR 3.0 FG
          ↓
      OptiScaler
          ↓
        XeFG
          ↓
      多帧生成
```

**使用条件：**

- 游戏需要自带 FSR 3.0 FG；
- 游戏内必须开启 FSR FG；
- OptiScaler 接入 FG 输入并配置 XeFG 输出；
- 根据实际游戏稳定性逐步提高多帧倍率。

> FSR 3.0 与 FSR 3.1 在具体游戏中的资源、时序和兼容表现可能不同，不建议假设同一套参数可以直接复制到所有游戏。

---

### ⭐ 方案 4：DLSSG + XeFG 30X

针对**游戏原生支持 DLSS Frame Generation** 的游戏。

```text
游戏原生 DLSSG
      ↓
Streamline / OptiScaler FG Input
      ↓
     XeFG
      ↓
多帧生成
```

**使用条件：**

1. 游戏需要支持 DLSS Frame Generation。
2. 在游戏设置中开启 DLSS FG。
3. 根据游戏的 Streamline / DLSSG 支持情况选择对应的 FG Input。
4. FG Output 设置为 **XeFG**。
5. 根据 XeFG 配置设置多帧倍率。
6. 重启游戏后检查 `OptiScaler.log` 以及实际画面表现。

> 对于已经支持原生 DLSS FG 的游戏，优先考虑 DLSSG via Streamline 等原生 FG 输入路径；OptiFG 更适合作为没有原生 FG 时的实验性方案。

---

### ⭐ 方案 5：DLSSG + DLSSG 6X

这是针对**游戏原生支持 DLSS Frame Generation / DLSSG** 的另一条多帧生成实验路线。

```text
游戏原生 DLSSG
      ↓
Streamline / OptiScaler
      ↓
    DLSSG
      ↓
   DLSSG 6X
      ↓
多帧生成
```

**使用条件：**

1. 游戏需要支持 DLSS Frame Generation。
2. 在游戏设置中开启 DLSS FG。
3. 确认 DLSSG / Streamline FG 输入能够正常初始化。
4. 根据当前版本配置选择 **DLSSG 6X** 输出/多帧生成方案。
5. 启动游戏后检查 FG 是否正常激活。
6. 通过 FPS、Frame Time、画面稳定性以及 UI / HUD 表现确认实际效果。

> **定位说明：** `DLSSG + DLSSG 6X` 与 `DLSSG + XeFG 30X` 是两条不同的实验路线。前者保持 DLSSG 作为 FG / 多帧生成链路的一部分，后者则将 DLSSG 作为输入并交由 XeFG 输出。实际可用倍率和兼容性以当前项目版本、游戏和配置为准。

---

### 🛠️ 推荐选择顺序

为了减少兼容性问题，可以按照下面的思路选择 FG Input：

```text
① 游戏原生 DLSSG
        ↓
② 游戏原生 FSR 3.1 FG
        ↓
③ 游戏原生 FSR 3.0 FG
        ↓
④ OptiFG
```

然后统一进入：

```text
FG Input
    ↓
OptiScaler
    ↓
XeFG Output
    ↓
Multi-Frame Generation
```

**核心原则：**

> 能使用游戏原生 FG 输入，就优先使用原生 FG 输入；只有在游戏没有原生 FG，或者原生 FG 不可用时，再考虑 OptiFG。

---

### ⚙️ OptiScaler 中的核心配置思路

多帧生成方案主要围绕两个概念：

```ini
[FrameGen]
FGInput=...
FGOutput=xefg
```

其中：

- `FGInput`：决定**帧生成输入来自哪里**；
- `FGOutput`：决定**最终使用哪一种 FG 输出方式**。

本项目的核心实验路线：

```text
OptiFG      ─┐
FSR 3.1 FG  ─┤
FSR 3.0 FG  ─┼──→ OptiScaler ──→ XeFG ──→ Multi-Frame Generation
DLSSG       ─┘
```

具体参数以当前 Release 提供的配置文件为准，不建议直接把其他游戏的完整 `OptiScaler.ini` 原样复制。

---

### 🔄 修改配置后的标准流程

```text
修改 OptiScaler.ini
        ↓
保存配置
        ↓
完全退出游戏
        ↓
重新启动游戏
        ↓
确认 FG Input
        ↓
确认 FG Output = XeFG
        ↓
确认 FG Active
        ↓
观察 FPS / Frame Time / 画面伪影
```

如果出现闪烁、拖影、重影、UI 抖动、HUD 重复、黑屏或崩溃，建议先恢复最近一次可用配置，再逐项修改参数。

## 📖 项目简介

**DLSS5-30x-6x-AMD-OptiScaler** 是一个面向 AMD GPU 用户的实验性技术研究与兼容性整理项目。

项目核心不是单独实现某一种 Frame Generation，而是围绕 **OptiScaler + 多种 FG Input + XeFG Output + AMD GPU** 建立统一实验框架，用于研究不同游戏在 AMD 硬件环境下的超分辨率、帧生成、多帧生成以及相关神经渲染技术。
目前主要研究

| **1** | **OptiFG** | **XeFG 30X** | ✅ **已实现** | 不要求游戏原生 FG |

| **2** | **FSR 3.1 FG** | **XeFG 30X** | ✅ **已实现** | 需要游戏自带 FSR 3.1 FG |

| **3** | **FSR 3.0 FG** | **XeFG 30X** | ✅ **已实现** | 需要游戏自带 FSR 3.0 FG |

| **4** | **DLSSG** | **XeFG 30X** | ✅ **已实现** | 需要游戏自带 DLSS FG |

| **5** | **DLSSG** | **DLSSG 6X** | ✅ **已实现** | 需要游戏自带 DLSS FG |


### 🎯 项目核心方向

- AMD GPU + OptiScaler 实际运行与兼容性测试
- DLSS / DLSSG 输入路径研究
- FSR 3.0 / 3.1 Frame Generation 输入路径研究
- OptiFG 实验性 Frame Generation 输入
- XeFG 多帧生成输出方案
- DLSS 5 / Neural Rendering 相关实验
- 游戏兼容性、稳定性与画质测试
- Frame Time、FPS、1% Low、GPU / VRAM 等性能数据记录
- 不同 FG Input / Output 组合的兼容性分析

### 🧠 技术架构

本项目采用“**输入源与输出端解耦**”的思路：

```text
                ┌─ OptiFG
                ├─ FSR 3.1 FG
                ├─ FSR 3.0 FG
Game ───────────┤
                └─ DLSSG
                      │
                      ▼
                  OptiScaler
                      │
                      ▼
                    XeFG
                      │
                      ▼
             Multi-Frame Generation
```

其中：

- **FG Input**：负责向 OptiScaler 提供帧生成所需输入；
- **OptiScaler**：负责兼容层、资源转换以及 FG 路径管理；
- **XeFG**：作为实验方案中的 Frame Generation / Multi-Frame Generation 输出端；
- **MFG**：通过 XeFG 相关配置进行更高倍率的插帧实验。

这种架构允许同一套研究框架覆盖不同游戏的原生 FG 路径，同时保留 OptiFG 作为无原生 FG 游戏的实验性补充。

### 🔗 项目基础与上游来源

本项目基于并参考以下开源项目及社区工作：

- [OptiScaler](https://github.com/optiscaler/OptiScaler) —— 核心兼容层与 Upscaling / Frame Generation 框架
- [DLSS-NR-UE5-Opti-DLL](https://github.com/Vodkaman23/DLSS-NR-UE5-Opti-DLL) —— DLSS Neural Rendering / AMD 相关实验基础
- [dlss-5-amd-project](https://github.com/MatheusGViana/dlss-5-amd-project) —— AMD DLSS / Neural Rendering 社区项目
- [DLSS-NR-on-AMD](https://github.com/danielblnc/DLSS-NR-on-AMD) —— AMD Neural Rendering 相关实现与研究
- [TheAutomatic/dlss-5-amd-project](https://github.com/TheAutomatic/dlss-5-amd-project) —— 社区持续维护版本及 AMD DLSS5 相关方案
- 政宗SAMA 社区项目

特别感谢上述项目作者及社区贡献者。

> **说明：** 本项目是在上述开源项目、公开资料和社区研究基础上的独立实验与整理项目。不同项目之间的代码、DLL、模型、插件和许可证保持其各自归属。本仓库不会将第三方项目的版权或许可证视为本项目所有。

> **重要：** 本项目中的“XeFG 30X / 6X”等倍率描述属于当前实验配置、插件能力或测试结果的整理方式，不应理解为 Intel XeFG 或 OptiScaler 官方默认支持的固定倍率。

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

## 🧩 FG 方案速查

| FG Input | XeFG Output | 是否需要游戏原生 FG | 推荐使用场景 |
|---|---|---|---|
| **OptiFG** | **XeFG 30X** | ❌ 不需要 | 无原生 FG / 原生 FG 不可用的实验场景 |
| **FSR 3.1 FG** | **XeFG 30X** | ✅ 需要 | 游戏自带 FSR 3.1 FG |
| **FSR 3.0 FG** | **XeFG 30X** | ✅ 需要 | 游戏自带 FSR 3.0 FG |
| **DLSSG** | **XeFG 30X** | ✅ 需要 | 游戏自带 DLSS Frame Generation |
| **DLSSG** | **DLSSG 6X** | ✅ 需要 | 游戏自带 DLSS Frame Generation，使用 DLSSG 6X 实验路线 |

> **选择原则：** 原生 FG 输入通常优先于 OptiFG；OptiFG 主要用于没有原生 FG 或原生 FG 无法工作的情况。

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

## 📦 安装与部署

### 1. 获取项目文件

建议优先使用本仓库 **GitHub Releases** 发布的版本。

```text
Release
  ↓
下载对应版本
  ↓
解压
  ↓
阅读当前版本说明
  ↓
根据目标游戏选择 FG Input
```

### 2. 安装到游戏目录

基础部署流程：

1. 关闭游戏。
2. 备份原始游戏文件。
3. 将当前 Release 所需文件复制到游戏根目录。
4. 确认 `OptiScaler.ini` 与对应 DLL / 插件目录位置正确。
5. 根据游戏类型选择 FG Input。
6. 将 FG Output 设置为 XeFG（如果当前版本/配置支持该路径）。
7. 启动游戏。
8. 在 OptiScaler Overlay 中检查 Frame Generation 状态。
9. 修改参数后完全退出并重新启动游戏。

### 3. 第一次运行建议

不要第一次就直接追求最高倍率：

```text
确认游戏正常启动
        ↓
确认 OptiScaler 正常加载
        ↓
确认 FG Input 正常工作
        ↓
确认 XeFG Output 正常工作
        ↓
低倍率测试
        ↓
确认画面 / Frame Time / 稳定性
        ↓
再逐步提高多帧倍率
```

### 4. 安全与兼容性建议

- 修改配置前备份原文件。
- 每次只修改少量参数。
- 不要直接复制其他游戏的完整配置。
- 遇到崩溃时先恢复最近一次可用配置。
- 记录 GPU、驱动、Windows、游戏版本、OptiScaler 版本。
- 联机游戏使用前，应确认相关 Mod / DLL / 注入方式是否违反游戏规则或触发反作弊。

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

## 📄 许可证

本项目自有代码采用 [MIT License](LICENSE) 开源。

### 第三方项目与组件

本项目基于或参考了多个开源项目，包括但不限于：

- OptiScaler
- DLSS-NR 相关项目
- XeFG / XeSS 相关组件
- 其他第三方开源组件

第三方项目及其相关代码、模型、DLL 或其他组件仍受其原始许可证约束。

使用、修改或重新分发本项目时，请同时遵守相关第三方项目的许可证及版权声明。
