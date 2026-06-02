# FlexArm · 灵臂

<p align="center">
  <img src="flx-icon-256.png" alt="FlexArm Logo" width="120">
</p>

> 让机械臂拥有"眼睛"�?双手"——视觉标�?+ 像素级精度，在电脑上操控真实手机

[![GitHub Release](https://img.shields.io/badge/v2.0.0-Server-purple)](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.0)
[![GitHub Release](https://img.shields.io/badge/v1.0.0-Calibration-green)](https://github.com/hamlet0168/FlexArm/releases/tag/v1.0.0)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-lightgrey)]()
[![License](https://img.shields.io/badge/License-MIT-yellow)]()

---

## 📦 项目组件

本仓库包含两个独立的 FlexArm 子项目：

### 🎯 Calibration · 标定程序 �?已发�?(v1.0.0)

**位置�?* [`calibration/`](calibration/)

独立标定程序，开箱即用。通过摄像头识别手机四角点完成机械臂标定，标定时鼠标在电脑画面上点击操控手机�?
- [📥 下载最新发布版 (v1.0.0, 159 MB)](https://github.com/hamlet0168/FlexArm/releases/tag/v1.0.0)
- [📖 查看使用教程](https://hamlet0168.github.io/FlexArm/calibration/tutorial_cn.html)
- [📂 查看 Calibration 源码](calibration/README.md)

### 🌐 FlexArm Server · API 服务 �?已发�?(v2.0.0)

**位置�?* [`RobotArmServer/`](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.0)

Flask HTTP API 服务，提供脚本化自动控制能力。支�?YAML 脚本编排、智能页面检测、图�?文字识别点击、条件分支等�?
- [📥 下载 RobotArmServer v2.0.0 (231 MB)](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.0)
- [📖 AI Agent Skill 文档](skill.md)

---

## 🏗�?仓库结构

```
flexarm/
├── README.md                  # 项目总览（本文件�?├── index.html                 # GitHub Pages 品牌首页
├── skill.md                   # AI Agent 完整 API 技能文�?├── flx-icon-256.png           # 品牌 Logo (256x256)
├── calibration/               # 标定程序（v1.0.0 已发布）
�?  ├── README.md              # Calibration 详细文档
�?  ├── tutorial_cn.html       # 中文使用教程
�?  └── tutorial_assets/       # 教程图片
└── RobotArmServer/            # Flask API 服务（v2.0.0 已发布）
    └── ...
```

---

## �?我应该用哪个版本�?
| 需求场�?| 推荐版本 |
|----------|----------|
| 我想用鼠标直接操控手�?| **Calibration 标定程序** �?|
| 我想通过 API 脚本控制机械�?| **FlexArm Server** �?|
| 两个都想�?| 先标定，再装 Server |

---

## 📄 许可�?
本项目采�?[MIT License](LICENSE)�?
---

## 🤝 联系

- 📧 hamlet0168@163.com
- 🐛 [提交 Issue](https://github.com/hamlet0168/FlexArm/issues)
