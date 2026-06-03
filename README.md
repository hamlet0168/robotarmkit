# FlexArm · 灵臂

> Robotic arm automation with vision calibration & pixel-level precision

<p align="center">
  <img src="flx-icon-256.png" alt="FlexArm Logo" width="120">
</p>

> 给AI一双眼睛和双手，替你操控真实手机
>
> *Give AI eyes to see reality and hands to operate your phone*

[![GitHub Release](https://img.shields.io/badge/v2.0.1-Server-purple)](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.1)
[![GitHub Release](https://img.shields.io/badge/v1.0.0-Calibration-green)](https://github.com/hamlet0168/FlexArm/releases/tag/v1.0.0)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-lightgrey)]()
[![License](https://img.shields.io/badge/License-MIT-yellow)]()

---

## 📦 项目组件 / Project Components

本仓库包含两个独立的 FlexArm 子项目：

*This repository contains two independent FlexArm sub-projects:*

### 🎯 Calibration · 标定程序 ✅ 已发布 (v1.0.0)

**位置 / Location：** [`calibration/`](calibration/)

独立标定程序，开箱即用。通过摄像头识别手机四角点完成机械臂标定，标定时鼠标在电脑画面上点击操控手机。

*Standalone calibration program, ready to use. Identifies phone corner points via camera to complete robot arm calibration. After calibration, control your phone by clicking on the computer screen.*

- [📥 下载最新发布版 / Download (v1.0.0, 159 MB)](https://github.com/hamlet0168/FlexArm/releases/tag/v1.0.0)
- [📖 查看使用教程 / User Guide (中文)](https://hamlet0168.github.io/FlexArm/calibration/tutorial_cn.html)
- [📖 View User Guide (English)](https://hamlet0168.github.io/FlexArm/calibration/tutorial_en.html)
- [📂 查看 Calibration 源码 / Source](calibration/README.md)

### 🌐 FlexArm Server · API 服务 ✅ 已发布 (v2.0.1)

**位置 / Location：** [`RobotArmServer/`](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.1)

FlexArm Server HTTP API 服务，提供脚本化自动控制能力。支持 YAML 脚本编排、智能页面检测、图标/文字识别点击、条件分支等。

*FlexArm Server HTTP API service providing scripted automatic control. Supports YAML script orchestration, smart page detection, icon/text recognition & click, conditional branching, etc.*

- [📥 下载 / Download RobotArmServer v2.0.1 (236 MB)](https://github.com/hamlet0168/FlexArm/releases/latest)
- [📖 AI Agent Skill 文档 / Documentation](FlexArmServer/skill.md)
- [📖 桌面识别配置教程 / Desktop Setup Guide](https://hamlet0168.github.io/FlexArm/FlexArmServer/desktop_tutorial_cn.html)
- [📖 Desktop Setup Guide (English)](https://hamlet0168.github.io/FlexArm/FlexArmServer/desktop_tutorial_en.html)

---

## 🏗️ 仓库结构 / Repository Structure

```
flexarm/
├── README.md                  # 项目总览 / Project overview
├── index.html                 # GitHub Pages 品牌首页 / Brand homepage
├── FlexArmServer/skill.md     # AI Agent 完整 API 技能文档 / Full API skill doc
├── flx-icon-256.png           # 品牌 Logo / Brand Logo (256x256)
├── calibration/               # 标定程序 / Calibration (v1.0.0 released)
│   ├── README.md              # Calibration 详细文档 / Detailed docs
│   ├── tutorial_cn.html       # 中文使用教程 / Chinese tutorial
│   ├── tutorial_en.html       # English tutorial
│   └── tutorial_assets/       # 教程图片 / Tutorial images
├── FlexArmServer/             # FlexArm Server + 教程 / Server + tutorials
│   ├── README.md              # 项目介绍 + 教程索引 / Project intro + tutorial index
│   ├── skill.md               # AI Agent 完整 API 技能文档 / Full API skill doc
│   ├── desktop_tutorial_cn.html  # 桌面识别配置教程 / Desktop setup guide (CN)
│   ├── desktop_tutorial_en.html  # Desktop setup guide (EN)
│   ├── desktop_demo_pic/        # 教程示例图片 / Demo images
│   ├── dial_tutorial_cn.html     # 拨号/挂断识别教程 / Dial recognition guide (CN)
│   ├── dial_tutorial_en.html     # Dial recognition guide (EN)
│   └── dial_demo_pic/           # 拨号教程示例图片 / Dial demo images
```

---

## ❓ 我应该用哪个版本？ / Which version should I use?

| 需求场景 / Use Case | 推荐版本 / Recommended |
|----------|----------|
| 我想用鼠标直接操控手机 / Control phone with mouse | **Calibration 标定程序** ✅ |
| 我想通过 API 脚本控制机械臂 / Control via API scripts | **FlexArm Server** ✅ |
| 两个都想用 / Both | 先标定，再装 Server / Calibrate first, then install Server |

---

## 📄 许可证 / License

本项目采用 [MIT License](LICENSE)。

*This project is licensed under the [MIT License](LICENSE).*

---

## 🤝 联系 / Contact

- 📧 hamlet0168@163.com
- 🐛 [提交 Issue / Report Issues](https://github.com/hamlet0168/FlexArm/issues)
