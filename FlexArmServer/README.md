# 🌐 FlexArm Server · API 服务

> 给AI一双眼睛和双手，替你操控真实手机
> *Give AI eyes to see reality and hands to operate your phone*

[![GitHub Release](https://img.shields.io/badge/v2.0.0-Server-purple)](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.0)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue)]()
[![Platform](https://img.shields.io/badge/Platform-Windows%2010%2F11-lightgrey)]()
[![License](https://img.shields.io/badge/License-MIT-yellow)]()

---

## 📖 简介 / Introduction

FlexArm Server 是 FlexArm 项目的 HTTP API 服务组件，基于 Flask 构建。提供完整的机械臂脚本化自动控制能力，支持 YAML 脚本编排、智能页面检测、图标/文字识别点击、条件分支等高级功能。

*FlexArm Server is the HTTP API service component of the FlexArm project, built on Flask. It provides full scripted automatic control of the robot arm, supporting YAML script orchestration, smart page detection, icon/text recognition & click, conditional branching, and more.*

适用于需要通过 API 或脚本实现自动化批量操作的场景，也可作为 AI Agent 控制机械臂的标准接口。

*Ideal for automated batch operations via API or scripts, and serves as the standard interface for AI Agents to control the robot arm.*

---

## 📚 教程索引 / Tutorial Index

> 以下教程按序号排列，后续将持续更新。
> *Tutorials are numbered below. More will be added over time.*

| # | 教程 / Tutorial | 中文 | English |
|---|----------------|------|---------|
| (1) | 手机桌面识别配置 / Phone Desktop Recognition | [中文教程](https://hamlet0168.github.io/FlexArm/FlexArmServer/desktop_tutorial_cn.html) | [English Guide](https://hamlet0168.github.io/FlexArm/FlexArmServer/desktop_tutorial_en.html) |
| (2) | 拨号/挂断识别 / Dial Number & Hang Up Detection | [中文教程](https://hamlet0168.github.io/FlexArm/FlexArmServer/dial_tutorial_cn.html) | [English Guide](https://hamlet0168.github.io/FlexArm/FlexArmServer/dial_tutorial_en.html) |
| (3) | 🚧 即将推出 / Coming Soon | — | — |
| (4) | 🚧 即将推出 / Coming Soon | — | — |
| (5) | 🚧 即将推出 / Coming Soon | — | — |

---

## 🤖 AI Agent Skill 文档

完整 API 技能参考文档，供 AI Agent 直接调用：

- [📖 AI Agent Skill 文档 (skill.md)](skill.md)

*Full API skill reference document for AI Agent direct invocation.*

---

## 📦 目录结构 / Directory Structure

```
FlexArmServer/
├── README.md                  # 本文件 / This file
├── skill.md                   # AI Agent 完整 API 技能文档 / Full API skill doc
├── desktop_tutorial_cn.html   # 桌面识别配置教程 / Desktop setup guide (CN)
├── desktop_tutorial_en.html   # Desktop setup guide (EN)
├── desktop_demo_pic/          # 教程示例图片 / Demo images
├── dial_tutorial_cn.html      # 拨号/挂断识别教程 / Dial recognition guide (CN)
├── dial_tutorial_en.html      # Dial recognition guide (EN)
└── dial_demo_pic/             # 拨号教程示例图片 / Dial demo images
```

---

## 🚀 快速开始 / Quick Start

1. 从 [Releases](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.0) 下载 `RobotArmServer.zip`
2. 解压到任意目录（如 `D:\FlexArm`）
3. 运行 `RobotArmServer.exe`
4. 浏览器访问 `http://127.0.0.1:7826/api/health` 验证服务

*1. Download `RobotArmServer.zip` from [Releases](https://github.com/hamlet0168/FlexArm/releases/tag/v2.0.0)*
*2. Extract to any directory (e.g. `D:\FlexArm`)*
*3. Run `RobotArmServer.exe`*
*4. Visit `http://127.0.0.1:7826/api/health` to verify*

---

## 📄 许可证 / License

本项目采用 MIT 许可证 — 详见 [LICENSE](LICENSE) 文件。
*This project is licensed under the MIT License — see the [LICENSE](LICENSE) file.*

---

## 🤝 联系 / Contact

- 📧 hamlet0168@163.com
- 🐛 [提交 Issue / Report Issues](https://github.com/hamlet0168/FlexArm/issues)
