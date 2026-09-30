# DLSS5-30x-6x-AMD-OptiScaler

Quark Cloud Drive package:
Link: https://pan.quark.cn/s/a066f57d7905?pwd=qW4m
Extraction code:qW4m

## 📖 Installation & Usage Guide

### 🔗 Tutorial

- Video tutorial: [柠檬有多萌萌胧未可知 · 哔哩哔哩个人空间](https://space.bilibili.com/173069391?spm_id_from=333.1007.0.0)
Version v7.1 is aligned with the MasamuneSAMA project's v1.9.6.3 
https://github.com/TheAutomatic/dlss-5-amd-project/releases upstream release. Download the required files directly from the upstream project. Aligned with 0.5.0.
Usage: download the v1.9.6.3-ZIP project archive from the upstream project above, then download the V6.1 version. Copy the V6.1 project files into the extracted v1.9.6.3-ZIP directory and overwrite the matching files. Then run `nm.bat`, click "Select Folder", choose the corresponding DLL injection method by number, press Enter, enter `3`, press Enter, enter `2`, press Enter, and finally enter `1` and press Enter.
Copy all resulting files to the game's root directory. The remaining steps are essentially the same as the normal OptiScaler workflow.
---

### 🚀 Multi-Frame Generation Architecture

This project organizes the AMD + OptiScaler experimental approach into a unified **FG Input → XeFG Output** architecture:

```text
Game Render Frame
    │
    ├── FG Input (Frame Generation input source)
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
Multi-Frame Generation / Multi-Frame Generation
```

### 🧩 Current 4 Core Paths

## 🚀 XeFG 30X Core Architecture Rework

This project has restructured the original frame-generation pipeline, with a focus on the **reconstruction and implementation of the XeFG 30X multi-frame generation pipeline**.

### 30X Frame-Generation Pipeline

The reworked XeFG 30X pipeline is designed around **inserting thirty generated frames at 1/30 intervals**, taking a single base frame as input and expanding it into continuous multi-frame output.

```text
Base Rendered Frame
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
Continuous Multi-Frame Output
```

Unlike traditional 2X / 3X / 4X frame generation, **the core of 30X is not simply increasing a fixed multiplier; it is a restructured multi-frame insertion pipeline in which a single base rendered frame can correspond to up to 30 generated-frame positions.**

### Theoretical 30X Output Capability

> **1 base rendered frame + 30 generated-frame positions = a theoretical maximum 30X multi-frame generation multiplier**

Under theoretical conditions, if GPU compute performance, VRAM capacity, memory bandwidth, and overall pipeline throughput are sufficient, output approaching the theoretical 30X capability may be achievable.

The actual multiplier is affected by GPU performance, VRAM, frame time, output resolution, game workload, and the FG input pipeline.

### 30X Technical Positioning

The focus of XeFG 30X is not simply increasing a multiplier value, but restructuring the following pipeline:

```text
FG Input
   ↓
Multi-Frame Generation
   ↓
FG Output
```

The current implementation includes:

```text
OptiFG
   ↓
XeFG 30X
```

and:

```text
FSR 3.0 FG ─┐
FSR 3.1 FG ─┤
DLSSG ──────┤
             ↓
         XeFG 30X
             ↓
        Multi-Frame Generation Output
```

XeFG 30X can therefore serve as a unified multi-frame generation output path after different FG input sources.

> **Note: 30X represents the theoretical maximum generation multiplier. The achievable multiplier and final FPS are limited by GPU performance, game workload, FG input type, resolution, and the current runtime environment.**

## 🚀 5 Currently Implemented FG / MFG Paths

> **The following 5 FG paths are currently implemented in this project.**  
> Actual availability depends on the game, OptiScaler version, FG input type, and current configuration.

| # | FG Input | FG Output | Implementation Status | Requirements |
|---|---|---|---|---|
| **1** | **OptiFG** | **XeFG 30X** | ✅ **Implemented** | Does not require native game FG |
| **2** | **FSR 3.1 FG** | **XeFG 30X** | ✅ **Implemented** | Requires native FSR 3.1 FG support |
| **3** | **FSR 3.0 FG** | **XeFG 30X** | ✅ **Implemented** | Requires native FSR 3.0 FG support |
| **4** | **DLSSG** | **XeFG 30X** | ✅ **Implemented** | Requires native DLSS FG support |
| **5** | **DLSSG** | **DLSSG 6X** | ✅ **Implemented** | Requires native DLSS FG support |

### Implemented FG Paths

The project currently implements the following 5 Frame Generation (FG) / Multi-Frame Generation (MFG) combinations:

```text
① OptiFG → XeFG 30X
② FSR 3.1 FG → XeFG 30X
③ FSR 3.0 FG → XeFG 30X
④ DLSSG → XeFG 30X
⑤ DLSSG → DLSSG 6X
```

****OptiFG Input Path:****

```text
Game Rendering
   ↓
OptiFG
   ↓
XeFG 30X
   ↓
Multi-Frame Generation Output
```

Key point: **No native game FG is required**.

****Native Game FG Input Paths:****

```text
FSR 3.1 FG ─┐
FSR 3.0 FG ─┤
DLSSG ──────┤ → XeFG 30X
             │
DLSSG ──────┴ → DLSSG 6X
```

These paths require the game to provide the corresponding **native Frame Generation** feature and for it to be enabled in the game.

### ⭐ Path 1: OptiFG + XeFG 30X

**Recommended use cases:**

- The game does not provide native Frame Generation;
- The game's native FG does not work correctly;
- You want to use OptiScaler's experimental OptiFG to establish an FG input and then pass it to XeFG for multi-frame generation.

```text
game
 ↓
OptiScaler / OptiFG
 ↓
XeFG
 ↓
Multi-Frame Generation
```

**Notes:**

- OptiFG is currently primarily targeted at DX12.
- OptiFG is an experimental solution and is not equivalent to native game FG.
- Some games may exhibit HUD ghosting, duplicated UI, flickering, or other frame-generation artifacts.
- If the game already has usable native FG, native FG input should generally be preferred.

---

### ⭐ Path 2: FSR 3.1 FG + XeFG 30X

For games with **native FSR 3.1 Frame Generation** support.

```text
native game FSR 3.1 FG
          ↓
      OptiScaler
          ↓
        XeFG
          ↓
      Multi-Frame Generation
```

**Requirements:**

1. The game supports FSR 3.1 Frame Generation.
2. Open the game settings and enable **FSR 3.1 FG / Frame Generation**.
3. Launch OptiScaler.
4. Set FG Input to the corresponding FSR FG input.
5. Set FG Output to **XeFG**.
6. Set the frame-generation multiplier according to the XeFG multi-frame configuration.
7. Restart the game and test.

> When the game already provides native FSR FG input, prefer the native input instead of forcing an input through OptiFG.

---

### ⭐ Path 3: FSR 3.0 FG + XeFG 30X

For games that still use **FSR 3.0 Frame Generation**.

```text
native game FSR 3.0 FG
          ↓
      OptiScaler
          ↓
        XeFG
          ↓
      Multi-Frame Generation
```

**Requirements:**

- The game must provide FSR 3.0 FG;
- FSR FG must be enabled in the game;
- OptiScaler must receive the FG input and be configured for XeFG output;
- Increase the multi-frame multiplier gradually based on actual game stability.

> FSR 3.0 and FSR 3.1 may differ in resources, timing, and compatibility depending on the game. Do not assume that the same parameters can be directly copied to every game.

---

### ⭐ Path 4: DLSSG + XeFG 30X

For games with **native DLSS Frame Generation** support.

```text
Native Game DLSSG
      ↓
Streamline / OptiScaler FG Input
      ↓
     XeFG
      ↓
Multi-Frame Generation
```

**Requirements:**

1. The game must support DLSS Frame Generation.
2. Enable DLSS FG in the game settings.
3. Select the appropriate FG Input based on the game's Streamline / DLSSG support.
4. Set FG Output to **XeFG**.
5. Set the multi-frame multiplier according to the XeFG configuration.
6. Restart the game and check `OptiScaler.log` and the actual visual output.

> For games that already support native DLSS FG, native FG input paths such as DLSSG via Streamline should generally be considered first; OptiFG is better suited as an experimental option when native FG is unavailable.

---

### ⭐ Path 5: DLSSG + DLSSG 6X

This is another experimental multi-frame generation path for games with **native DLSS Frame Generation / DLSSG** support.

```text
Native Game DLSSG
      ↓
Streamline / OptiScaler
      ↓
    DLSSG
      ↓
   DLSSG 6X
      ↓
Multi-Frame Generation
```

**Requirements:**

1. The game must support DLSS Frame Generation.
2. Enable DLSS FG in the game settings.
3. Confirm that the DLSSG / Streamline FG input initializes correctly.
4. Select the **DLSSG 6X** output / multi-frame generation configuration supported by the current version.
5. After launching the game, verify that FG is activated correctly.
6. Evaluate the actual result using FPS, frame time, visual stability, and UI / HUD behavior.

> **Positioning:** `DLSSG + DLSSG 6X` and `DLSSG + XeFG 30X` are two separate experimental paths. The former keeps DLSSG as part of the FG / multi-frame generation pipeline, while the latter uses DLSSG as the input and passes it to XeFG for output. Actual multiplier availability and compatibility depend on the current project version, game, and configuration.

---

### 🛠️ Recommended Selection Order

To reduce compatibility issues, select FG Input in the following order:

```text
① Native Game DLSSG
        ↓
② native game FSR 3.1 FG
        ↓
③ native game FSR 3.0 FG
        ↓
④ OptiFG
```

They then enter the common pipeline:

```text
FG Input
    ↓
OptiScaler
    ↓
XeFG Output
    ↓
Multi-Frame Generation
```

**Core principle:**

> When native game FG input is available, prefer it. Consider OptiFG only when the game has no native FG or native FG is unavailable.

---

### ⚙️ Core OptiScaler Configuration Concept

The multi-frame generation setup revolves around two main concepts:

```ini
[FrameGen]
FGInput=...
FGOutput=xefg
```

Where:

- `FGInput`：determines **where the frame-generation input comes from**；
- `FGOutput`：determines **which FG output method is ultimately used**。

The core experimental path of this project:

```text
OptiFG      ─┐
FSR 3.1 FG  ─┤
FSR 3.0 FG  ─┼──→ OptiScaler ──→ XeFG ──→ Multi-Frame Generation
DLSSG       ─┘
```

Use the configuration files provided with the current Release as the reference. Do not directly copy a complete `OptiScaler.ini` from another game.

---

### 🔄 Standard Workflow After Changing Configuration

```text
Edit OptiScaler.ini
        ↓
Save the configuration
        ↓
Completely exit the game
        ↓
Restart the game
        ↓
Verify FG Input
        ↓
Verify FG Output = XeFG
        ↓
Verify FG Active
        ↓
Monitor FPS / Frame Time / visual artifacts
```

If you encounter flickering, trails, ghosting, UI jitter, duplicated HUD elements, a black screen, or crashes, restore the most recent working configuration first, then modify parameters one at a time.

## 📖 Project Overview

**DLSS5-30x-6x-AMD-OptiScaler** is an experimental technical research and compatibility project for AMD GPU users.

Rather than focusing on a single Frame Generation implementation, the project provides a unified experimental framework around **OptiScaler + multiple FG Inputs + XeFG Output + AMD GPUs** for researching upscaling, frame generation, multi-frame generation, and related neural-rendering technologies across different games on AMD hardware.
Current research focuses on

| **1** | **OptiFG** | **XeFG 30X** | ✅ **Implemented** | Does not require native game FG |

| **2** | **FSR 3.1 FG** | **XeFG 30X** | ✅ **Implemented** | Requires native FSR 3.1 FG support |

| **3** | **FSR 3.0 FG** | **XeFG 30X** | ✅ **Implemented** | Requires native FSR 3.0 FG support |

| **4** | **DLSSG** | **XeFG 30X** | ✅ **Implemented** | Requires native DLSS FG support |

| **5** | **DLSSG** | **DLSSG 6X** | ✅ **Implemented** | Requires native DLSS FG support |


### 🎯 Core Research Areas

- AMD GPU + OptiScaler runtime and compatibility testing
- DLSS / DLSSG input-path research
- FSR 3.0 / 3.1 Frame Generation input-path research
- Experimental OptiFG Frame Generation input
- XeFG multi-frame generation output
- DLSS 5 / Neural Rendering experiments
- Game compatibility, stability, and image-quality testing
- Performance data collection including frame time, FPS, 1% lows, GPU usage, and VRAM usage
- Compatibility analysis of different FG Input / Output combinations

### 🧠 Technical Architecture

The project follows a **decoupled input-source and output-end** architecture:

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

Where:

- **FG Input**：provides OptiScaler with the input required for frame generation；
- **OptiScaler**：handles the compatibility layer, resource conversion, and FG path management；
- **XeFG**：serves as the Frame Generation / Multi-Frame Generation output end in the experimental architecture；
- **MFG**：enables higher-multiplier frame-generation experiments through XeFG-related configuration。

This architecture allows the same research framework to cover native FG paths across different games while retaining OptiFG as an experimental option for games without native FG.

### 🔗 Project Foundations & Upstream Sources

This project is based on and references the following open-source projects and community work:

- [OptiScaler](https://github.com/optiscaler/OptiScaler) —— Core compatibility layer and Upscaling / Frame Generation framework
- [DLSS-NR-UE5-Opti-DLL](https://github.com/Vodkaman23/DLSS-NR-UE5-Opti-DLL) —— DLSS Neural Rendering / AMD experimental foundation
- [dlss-5-amd-project](https://github.com/MatheusGViana/dlss-5-amd-project) —— AMD DLSS / Neural Rendering community project
- [DLSS-NR-on-AMD](https://github.com/danielblnc/DLSS-NR-on-AMD) —— AMD Neural Rendering implementation and research
- [TheAutomatic/dlss-5-amd-project](https://github.com/TheAutomatic/dlss-5-amd-project) —— Community-maintained version and AMD DLSS5-related work
- MasamuneSAMA community project

Special thanks to the authors and community contributors of the projects above.

> **Note:** This project is an independent experimental and documentation effort based on the open-source projects, public information, and community research listed above. Code, DLLs, models, plugins, and licenses from different projects remain the property of their respective owners. This repository does not claim ownership of third-party copyrights or licenses.

> **Important:** Multiplier descriptions such as “XeFG 30X / 6X” in this project reflect current experimental configurations, plugin capabilities, or recorded test results. They should not be interpreted as fixed default multipliers officially supported by Intel XeFG or OptiScaler.

## ✨ Project Features

- DLSS-related technical experimentation on AMD GPUs
- OptiScaler-based configuration and experimentation
- Experimental DLSS 5/6x configuration collection
- Real-world Frame Generation testing
- Cross-game compatibility testing
- Performance and image-quality comparisons
- Windows / Linux installation and setup scripts
- Continuous collection of test results, configurations, and issue feedback
- Community Issues, test results, and usage feedback are welcome

---

## 🗂️ Project Structure

The repository currently contains:

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

### Main Directories / Files

| File / Directory | Description |
|---|---|
| `OptiScaler/` | OptiScaler project content |
| `external/` | Third-party dependencies and related resources |
| `images/` | Project images, test screenshots, and documentation assets |
| `OptiScaler.ini` | OptiScaler configuration file |
| `OptiScaler.sln` | Visual Studio solution |
| `setup_windows.bat` | Windows installation / configuration script |
| `setup_linux.sh` | Linux installation / configuration script |
| `Streamlined_fetcher_windows.bat` | Windows resource acquisition / deployment script |

---

## 🧩 FG Path Quick Reference

| FG Input | XeFG Output | Requires Native Game FG | Recommended Use Case |
|---|---|---|---|
| **OptiFG** | **XeFG 30X** | ❌ No | Experimental scenarios without native FG / when native FG is unavailable |
| **FSR 3.1 FG** | **XeFG 30X** | ✅ Yes | Games with native FSR 3.1 FG |
| **FSR 3.0 FG** | **XeFG 30X** | ✅ Yes | Games with native FSR 3.0 FG |
| **DLSSG** | **XeFG 30X** | ✅ Yes | Games with native DLSS Frame Generation |
| **DLSSG** | **DLSSG 6X** | ✅ Yes | Games with native DLSS Frame Generation using the DLSSG 6X experimental path |

> **Selection principle:** Native FG input is generally preferred over OptiFG; OptiFG is primarily intended for games without native FG or when native FG is unavailable.

---

## 🖥️ Test Environment

The project is tested using real hardware and game environments.

Because DLSS, OptiScaler, Frame Generation, and related configurations are affected by the GPU, driver, Windows, game version, and rendering path, test results may vary.

### Test Matrix

The following data will be recorded continuously:

| GPU | Driver Version | Windows | game | Resolution | OptiScaler | DLSS / FG | Status |
|---|---|---|---|---|---|---|---|
| To be added | To be added | To be added | To be added | To be added | To be added | To be added | Testing |

> Test results are based on the actual test environment. Results may vary across different GPUs, driver versions, Windows versions, OptiScaler versions, and game versions.

---

## 📦 Installation & Deployment

### 1. Obtain Project Files

It is recommended to use the version published in this repository's **GitHub Releases**.

```text
Release
  ↓
Download the corresponding version
  ↓
Extract
  ↓
Read the current release notes
  ↓
Select the FG Input for the target game
```

### 2. Install to the Game Directory

Basic deployment procedure:

1. Close the game.
2. Back up the original game files.
3. Copy the required files from the current Release to the game's root directory.
4. Confirm that `OptiScaler.ini` and the corresponding DLL / plugin directories are in the correct locations.
5. Select the FG Input according to the game.
6. Set FG Output to XeFG (if supported by the current version/configuration).
7. Launch the game.
8. Check the Frame Generation status in the OptiScaler Overlay.
9. After changing parameters, completely exit and restart the game.

### 3. First-Run Recommendations

Do not immediately aim for the highest multiplier on the first run:

```text
Confirm that the game starts normally
        ↓
Confirm that OptiScaler loads correctly
        ↓
Confirm that FG Input works correctly
        ↓
Confirm that XeFG Output works correctly
        ↓
Test at a low multiplier
        ↓
Confirm image quality / Frame Time / stability
        ↓
Gradually increase the multi-frame multiplier
```

### 4. Safety & Compatibility Recommendations

- Back up the original file before changing configuration.
- Change only a small number of parameters at a time.
- Do not directly copy a complete configuration from another game.
- If a crash occurs, restore the most recent working configuration first.
- Record the GPU, driver, Windows version, game version, and OptiScaler version.
- Before using this with online games, verify whether the relevant mods / DLLs / injection methods violate game rules or trigger anti-cheat systems.

## ⚙️ Configuration

The configurations in this project are primarily intended for experiments and testing in AMD GPU + OptiScaler environments.

### `OptiScaler.ini`

Configuration may change as OptiScaler versions and project test results evolve.

Recommendations:

- Do not copy configurations from other GPUs / games and assume they will work.
- Save the original configuration before changing parameters.
- Change only a small number of parameters at a time to make troubleshooting easier.
- If flickering, trails, ghosting, black screens, crashes, or similar issues occur, restore the most recent working configuration first.
- Record the GPU, driver, game version, and OptiScaler version to make issues reproducible.

> **Note:** Different games may use different rendering paths and input data, so there is no universal configuration that works for every game.

---

## 🎮 Game Compatibility Testing

The project will continue collecting real-world results for AMD GPU + OptiScaler + DLSS-related features across different games.

The following format is recommended for recording tests:

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

### Compatibility Status

| Status | Meaning |
|---|---|
| ✅ Working | Generally works |
| 🟡 Testing | Testing |
| ⚠️ Issues | Runs with issues |
| ❌ Not Working | Does not work in the current environment |
| ❓ Unknown | Insufficient test data |

---


## 🎮 Real-World Test: Resident Evil 9

### AMD Radeon RX 7900 XTX

This project is based on **OptiScaler** and has been tested with *Resident Evil 9* on an AMD Radeon RX 7900 XTX.

The currently recorded results are as follows:

### 4K Test

| Configuration | FPS |
|---|---:|
| 4K + 6X Frame Generation | **476 FPS** |
| 4K + 6X Frame Generation + DLSS5 | **78 FPS** |

### 2K Test

| Configuration | FPS |
|---|---:|
| 2K + 6X Frame Generation | **889 FPS** |
| 2K + 6X Frame Generation + DLSS5 | **148 FPS** |

### Test Results

| Resolution | 6X Frame Generation | DLSS5 Enabled |
|---|---:|---:|
| **4K** | **476 FPS** | **78 FPS** |
| **2K** | **889 FPS** | **148 FPS** |

Based on the current test results, *Resident Evil 9* can achieve a high 6X frame-generation FPS on **RX 7900 XTX + OptiScaler**. With DLSS5 enabled, FPS decreases significantly but remains at a relatively high level.

> ⚠️ The above figures are actual results from the current test environment and represent only the performance of the current project configuration and test setup. Different GPUs, driver versions, game versions, OptiScaler versions, and configurations may produce different results.

> **Test condition note:** The current record contains only the FPS data listed above for the specified resolutions, 6X frame generation, and DLSS5-enabled configurations. Driver version, game version, specific scene, base rendering FPS, and other details that were not provided are not inferred.

## 📊 Performance Testing

The project focuses not only on whether a configuration works, but also on recording actual performance.

Future tests will record, where possible:

- Native Rendering FPS
- Upscaling FPS
- Frame Generation FPS
- 1% Low
- GPU Utilization
- GPU VRAM Usage
- CPU Utilization
- Input Latency
- Performance differences between configurations

For comparable tests, keep the following consistent:

- Same game version
- Same resolution
- Same graphics settings
- Same scene
- Same driver
- Same test time and methodology

This improves comparability between configurations.

---

## 🖼️ Image Quality Comparison

The project records image-quality differences between configurations, including:

- Native image quality
- DLSS / Upscaling
- Frame Generation
- DLSS-related experimental configurations
- Real-world behavior on AMD GPUs

Key areas of observation:

- Detail retention
- Distant objects
- Vegetation
- Reflections
- Transparency effects
- UI
- Flickering
- Trailing artifacts
- Ghosting
- Changes in sharpness

Screenshots and test assets will be continuously organized in the `images/` directory.

---

## ⚠️ Known Issues

As this is an experimental research project, the following issues may occur:

- Compatibility differences between games
- Performance differences between GPUs / drivers
- Frame Generation stability differences
- Flickering, trails, ghosting, and other visual artifacts
- Black screens, crashes, or initialization failures
- Some games may require additional configuration
- Different OptiScaler versions may produce different results

If you encounter an issue, first confirm:

1. GPU model
2. AMD driver version
3. Windows version
4. game version
5. OptiScaler version
6. Current Configuration
7. Logs and screenshots

---

## 🛠️ Maintenance

As the project maintainer, I continuously work on:

- AMD GPU + OptiScaler environment testing
- DLSS-related configuration research and documentation
- Experimental DLSS 5/6x configuration testing
- Frame Generation testing
- Cross-game compatibility testing
- Performance and image-quality comparisons
- *Resident Evil 9* RX 7900 XTX real-world performance testing
- Parameter tuning and troubleshooting
- Installation / deployment script maintenance
- README and project documentation maintenance
- Test-result and community-feedback organization
- Project releases and Release maintenance

The project will continue to be updated based on actual test results.

---

## 🤖 Codex Usage Plan

Codex is planned to assist with project development and maintenance, including:

- Analyzing project code and configuration
- Troubleshooting runtime issues
- Code development and refactoring
- Bug fixes
- Configuration analysis
- Writing and optimizing helper tools
- Automating parts of the testing workflow
- Analyzing GitHub Issues and community feedback
- Maintaining the README and project documentation
- Assisting with Releases and version maintenance

The goal of using Codex is to reduce repetitive maintenance work and improve the efficiency of testing, analysis, and documentation.

---

## 💳 API Credits Usage Plan

If API Credits are available, they will primarily support ongoing project development, testing, and maintenance, including:

- Automated test-log analysis
- Configuration parameter analysis
- Test-data organization
- Compatibility-result organization
- Issue analysis and classification
- Automated testing assistance
- Documentation generation and maintenance
- Release information organization
- Configuration validation and maintenance tools
- Project-tool development assistance

The goal is to make AMD + OptiScaler compatibility testing and research more automated, reproducible, and accessible to the community.

---

## 🐛 Issues / Feedback

Contributions are welcome in the form of:

- **Bug Report**
- **Game compatibility test results**
- **GPU / driver test results**
- **Configuration issues**
- **Performance test results**
- **Image-quality comparisons**
- **Improvement suggestions**

When submitting an issue, please provide as much of the following information as possible:

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

> Please do not publish API keys, passwords, account information, or other sensitive data in Issues, logs, or screenshots.

---

## 🔐 Security

The project involves configuration files, dynamic libraries, installation scripts, and third-party components.

The project will continue to monitor:

- DLL loading
- File handling
- Configuration parsing
- Third-party dependencies
- Installation and update procedures
- Script execution safety

If you discover a potential security issue, avoid posting sensitive information in a public Issue.

---

## 📌 Roadmap

- [x] Establish the AMD + OptiScaler DLSS 5/6x experimental project
- [x] Organize baseline configurations
- [x] Provide Windows / Linux scripts
- [x] Establish a basic testing and documentation system
- [ ] Test more AMD GPUs
- [ ] Test more games
- [ ] Expand the GPU / game compatibility list
- [ ] Add more performance test data
- [ ] Add more image-quality comparisons
- [ ] Improve automated testing
- [ ] Improve configuration validation tools
- [ ] Continue fixing issues and publishing new releases

---

## 📜 License

For the licensing of the code and documentation in this project, refer to the `LICENSE` file in the repository.

OptiScaler, third-party libraries, dynamic libraries, SDKs, game files, and other components used by this project remain subject to the copyrights and licenses of their respective owners.

Please review the applicable license requirements before using, modifying, or redistributing any related components.

---

## ⚠️ Disclaimer

**DLSS5-6x-AMD-OptiScaler** is an independent community research project.

This project is not officially affiliated with, authorized, endorsed, or sponsored by AMD, NVIDIA, OptiScaler, any game publisher, or any other third-party company.

The configurations and test results in this project represent experiments conducted in specific test environments. No guarantee is made that they will work correctly across all hardware, drivers, games, or system environments.

Users are responsible for verifying and complying with the licenses and terms of service of the software, games, drivers, and third-party components they use.

---

## ⭐ Support the Project

If this project is useful to you, you can support it by giving it a **Star** on GitHub.

Issues, test results, and usage feedback are welcome and help improve the project.

Thanks to everyone in the community who has contributed testing, issue reports, and technical support.

## 📄 License

Original code in this repository is released under the [MIT License](LICENSE).

### Third-Party Projects and Components

This project is based on or references multiple open-source projects, including but not limited to:

- OptiScaler
- DLSS-NR-related projects
- XeFG / XeSS-related components
- Other third-party open-source components

Third-party projects and their associated code, models, DLLs, and other components remain subject to their original licenses.

When using, modifying, or redistributing this project, please also comply with the licenses and copyright notices of the relevant third-party projects.
