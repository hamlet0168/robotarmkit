# 🤖 FlexArm · Calibration — 标定程序

> 让机械臂像人手一样精确操作手机屏幕
> *Make the robot arm operate phone screens as precisely as a human hand*

[🚀 **立即下载最新版本 / Download Latest (v1.0.0)**](https://github.com/hamlet0168/FlexArm/releases/tag/v1.0.0) ｜ [📖 使用教程（中文）](https://hamlet0168.github.io/FlexArm/calibration/tutorial_cn.html) ｜ [📖 User Guide (EN)](https://hamlet0168.github.io/FlexArm/calibration/tutorial_en.html)

[![GitHub Release](https://img.shields.io/badge/v1.0.0-Calibration-green)](https://github.com/hamlet0168/FlexArm/releases/tag/v1.0.0)
[![Python](https://img.shields.io/badge/Python-3.10%2B-green)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-lightgrey)]()
[![License](https://img.shields.io/badge/License-MIT-yellow)]()

---

## 📖 简介 / Introduction

RobotArmKit 是一套基于视觉标定的机械臂自动化工具，通过机械臂顶部摄像头 + 手机屏幕角点识别，实现**像素级精度的手机屏幕自动操作**。

*RobotArmKit is a vision-calibration-based robot arm automation tool. Using a top-mounted camera + phone screen corner detection, it achieves **pixel-precision automatic phone screen operations**.*

标定完成后，你可以在电脑上用鼠标点击手机屏幕任意位置，机械臂会自动移动触摸笔到对应坐标完成点击、滑动等操作。

*After calibration, you can click anywhere on the phone screen via the computer camera view, and the robot arm will automatically move the stylus to the corresponding coordinate to tap or swipe.*

---

## ✨ 功能特性 / Features

- 🎯 **像素级精度标定** — 通过手机四角点识别 + 机械臂触控验证，标定误差仅数像素
  *Pixel-precision calibration — corner point detection + robot arm touch verification, error within a few pixels*
- 📷 **实时视觉反馈** — 摄像头画面实时显示，自动识别角点，鼠标确认锁定
  *Real-time visual feedback — live camera feed, auto corner detection, mouse confirmation*
- 🖱️ **鼠标操作手机** — 标定后可直接用鼠标点击操控手机，像操作触屏一样自然
  *Mouse-controlled phone — after calibration, control your phone with mouse clicks as naturally as a touchscreen*
- 💾 **标定结果持久化** — 支持保存/加载多套标定结果，切换手机无需重复标定
  *Persistent calibration — save/load multiple calibration profiles, switch phones without re-calibrating*
- 🔧 **独立标定程序** — 开箱即用的 Windows 可执行文件，无需安装 Python 环境
  *Standalone program — ready-to-use Windows executable, no Python environment needed*
- 🌐 **FlexArm API 服务**（已推出） — HTTP 接口支持脚本化自动控制
  *Flask FlexArm Server API (released) — HTTP interface for scripted automatic control*

---

## 📦 仓库结构 / Repository Structure

```
FlexArm/
├── calibration/          # 独立标定程序 / Standalone calibration ✅
│   ├── RobotArmCalibration.exe  ← 主程序 / Main program
│   ├── phone_calibration_app.py ← 手机端 Python 脚本 / Phone Python script
│   ├── pydroid-3-8-3-arm64.apk  ← 手机端 Python 环境 / Phone Python env
│   ├── calibrations/            ← 标定结果存储 / Calibration storage
│   ├── tutorial_cn.html         ← 中文教程 / Chinese tutorial
│   └── tutorial_en.html         ← English tutorial
├── FlexArmServer/         # FlexArm API 服务 / API service
├── docs/                 # 文档 / Documentation
└── README.md             # 本文件 / This file
```

---

## 🚀 快速开始 / Quick Start

### 系统要求 / System Requirements

| 项目 / Item | 要求 / Requirement |
|------|------|
| 操作系统 / OS | Windows 10 / Windows 11（64位） |
| 硬件 / Hardware | 支持 USB 摄像头的机械臂设备 |
| 手机 / Phone | Android 手机（用于安装 Pydroid 3） |
| 网络 / Network | 电脑与手机需在同一局域网（WiFi） / Same LAN (WiFi) |

### 使用步骤 / Steps

1. **下载压缩包** — 从 [Releases](https://github.com/hamlet0168/FlexArm/releases) 下载最新版本
   *Download the latest release from [Releases](https://github.com/hamlet0168/FlexArm/releases)*
2. **解压到指定目录** — 建议 `D:\RobotArmKit`
   *Extract to a directory, e.g. `D:\RobotArmKit`*
3. **运行主程序** — 双击 `RobotArmCalibration.exe`（首次运行自动安装驱动）
   *Double-click `RobotArmCalibration.exe` (first run auto-installs drivers)*
4. **手机端准备** — 安装 Pydroid 3 APK，打开校准脚本
   *Install Pydroid 3 APK on phone, open calibration script*
5. **网络配置** — 在手机脚本中填入电脑的内网 IP
   *Enter your computer's LAN IP in the phone script*
6. **角点识别** — 程序自动识别手机四角，用鼠标确认锁定
   *Program auto-detects phone corners, confirm with mouse clicks*
7. **机械臂标定** — 通过 WASD 控制机械臂移动到各角点，完成标定
   *Control robot arm with WASD to move to each corner, complete calibration*
8. **验证结果** — 程序自动生成随机点，验证标定精度
   *Program auto-generates random points to verify calibration accuracy*

### 标定后 / After Calibration

- 在电脑摄像头画面中直接用鼠标点击操控手机
  *Click anywhere in the computer camera view to control the phone*
- 选择已保存的标定结果，无需重复标定
  *Select saved calibration profiles, no need to re-calibrate*
- 为不同手机保存多套标定文件
  *Save multiple calibration profiles for different phones*

---

## 📚 详细文档 / Documentation

| 文档 / Document | 说明 / Description |
|------|------|
| [📖 使用教程（中文）/ Chinese Tutorial](https://hamlet0168.github.io/FlexArm/calibration/tutorial_cn.html) | 图文教程，含常见问题 |
| [📖 User Guide (English)](https://hamlet0168.github.io/FlexArm/calibration/tutorial_en.html) | Illustrated tutorial with FAQ |

---

## 🛠️ 技术栈 / Tech Stack

| 层级 / Layer | 技术 / Technology |
|------|------|
| 视觉识别 / Vision | OpenCV + 角点检测 / Corner detection |
| 机械臂控制 / Arm Control | Windows 服务 + USB 驱动 |
| 前端界面 / UI | 摄像头实时画面 + 信息面板 / Live camera + info panel |
| 通信协议 / Protocol | 局域网 TCP（PC ↔ 手机）/ LAN TCP |
| API 服务 / API | Flask (v2.0.0 已发布 / released) |

---

## ❓ 常见问题 / FAQ

**Q: 首次运行提示安装驱动失败？ / Driver install fails on first run?**
A: 请确认系统为 Windows 10/11，检查杀毒软件是否阻止了硬件驱动安装。
*Confirm Windows 10/11, check if antivirus is blocking driver installation.*

**Q: 摄像头画面不显示或黑屏？ / Camera feed not showing or black?**
A: 检查：① 机械臂电源（蓝灯常亮） ② 机械臂蓝色 USB 线接入电脑 ③ 摄像头黑色 USB 线接入电脑。
*Check: ① Robot arm power (blue LED steady) ② Blue USB cable to PC ③ Camera black USB cable to PC.*

**Q: 标定结果保存在哪里？ / Where are calibration results saved?**
A: `calibrations/` 目录下，以时间戳命名（如 `calib_20260529_143311.json`），支持多个标定结果共存。
*In `calibrations/` directory, timestamp-named (e.g. `calib_20260529_143311.json`), multiple profiles can coexist.*

**Q: 重新调整手机固定装置后需要重新标定吗？ / Need to re-calibrate after adjusting phone holder?**
A: 是的，即使微小调整也应重新标定，原有标定文件不再适用。
*Yes, even slight adjustments require re-calibration; previous calibration files become invalid.*

---

## 🤝 贡献 / Contribute

欢迎提交 Issue 和 Pull Request！
*Feel free to submit Issues and Pull Requests!*

- 📧 hamlet0168@163.com
- 🐛 [提交问题 / Report Issues](https://github.com/hamlet0168/FlexArm/issues)

---

## 📄 许可证 / License

本项目采用 MIT 许可证 — 详见 [LICENSE](LICENSE) 文件。
*This project is licensed under the MIT License — see the [LICENSE](LICENSE) file.*

---

> 🤖 **FlexArm · 灵臂** — 给AI一双眼睛和双手，替你操控真实手机
> *Give robot arms "eyes" and "hands"*
