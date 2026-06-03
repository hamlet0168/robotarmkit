# FlexArm Robot — AI Agent Skill Reference

> 机械臂手机屏幕自动化操作系统。通过摄像头视觉 + 机械臂实现手机屏幕自动点击/滑动。

## 环境依赖与初始化

本技能的所有 API 调用均依赖于 `RobotArmServer.exe` 服务的运行。  
**使用本技能前，必须确保以下条件满足：**

- **独立机械臂标定程序**: 'RobotArmCalibration.exe’
  下载地址：[官方仓库 Release 页面](https://github.com/hamlet0168/flexarm/releases/tag/v1.0.0)   
  最新版本：v2.0.0  
  文件大小：约 160MB（压缩包）  
- **服务端程序**：`RobotArmServer.exe`  
  下载地址：[FlexArm v2.0.0 Release](https://github.com/hamlet0168/flexarm/releases/tag/v2.0.0)  
  最新版本：v2.0.0
  文件大小：约 231 MB（压缩包）


- **安装步骤**：
  1. 从上述地址下载 `RobotArmServer-v2.0.0.zip`
  2. 解压到任意目录（例如 `D:\FlexArm`），该目录即为 **项目根目录**
  3. 以管理员身份运行 `RobotArmServer.exe`（首次需管理员权限安装驱动）
  4. 验证：浏览器访问 `http://127.0.0.1:7826/api/health`，返回 `{"ok":true}`

- **目录结构约定**：  
  所有相对路径（如 `scripts/`、`icons/`）均基于上述项目根目录。  
  请勿修改 `_internal/` 目录下的文件。

> ⚠️ 如果服务未启动，本技能无法执行任何操作。  
> 在开始任务前，务必检查 `/api/health` 状态。

## 重要：固定端口号

**所有 API 请求必须使用 `7826` 端口，不是 5000。**

```
http://127.0.0.1:7826/api/*
```

端口固定为 7826，不可配置，不可修改。Flask 默认端口 5000 不适用本项目。

## 重要：curl 中文编码

**任何时候需要通过 HTTP API 传递中文字符，都严禁使用 curl 发送中文 JSON。**
curl 会破坏 UTF-8 编码，服务端无法正确识别中文关键词，导致查找失败。

```bash
# ❌ 错误：curl JSON 中的中文会被编码破坏
curl -X POST http://127.0.0.1:7826/api/find_text -d '{"text_keyword":"夸克"}'

# ✅ 正确：用 Python requests 发送中文参数
python -c "import requests; r = requests.post('http://127.0.0.1:7826/api/find_text', json={'text_keyword': '夸克'}); print(r.text)"
```

纯英文参数的 API（如 `detect_desktop`、`click_icon`、`run_script`、`click_at`）可用 curl，涉及中文关键词的（`find_text`、`click_text`、`detect_page 的页面名称`）必须用 Python。

---

## 完整 API 索引（59 个接口）

> 以下所有接口均可直接通过 HTTP POST/GET 调用，地址 `http://127.0.0.1:7826`。
> 点击链接跳转到文档中的详细说明。

### 系统 & 状态（8 个）

| # | 方法 | 接口 | 说明 |
|---|------|------|------|
| 1 | GET | `/` | 服务根路径 |
| 2 | GET | `/api/health` | 健康检查（服务状态、机械臂/摄像头连接） |
| 3 | GET | `/api/arm_status` | 机械臂状态（COM 口、服务运行、标定就绪） |
| 4 | GET | `/api/get_frame_info` | 获取画面宽高 |
| 5 | GET | `/api/get_overlay` | 获取画面覆盖层（视觉匹配结果） |
| 6 | GET | `/api/get_phone_corners` | 获取手机屏幕 4 角坐标 |
| 7 | GET | `/api/is_phone_present` | 检测手机是否在画面中 |
| 8 | GET | `/api/is_screen_on` | 检测屏幕是否点亮 |

### 画面显示 & 控制（10 个）

| # | 方法 | 接口 | 说明 |
|---|------|------|------|
| 9 | GET/POST | `/api/show_window` | 打开摄像头画面窗口 |
| 10 | GET/POST | `/api/hide_window` | 关闭画面窗口 |
| 11 | POST | `/api/toggle_phone_corners` | 切换手机外框叠加显示 |
| 12 | POST | `/api/change_focus` | 调整摄像头对焦（增量值） |
| 13 | GET | `/api/screenshots` | 列出历史截图文件 |
| 14 | POST | `/api/rotate_cw` | 画面顺时针旋转 90 度 |
| 15 | POST | `/api/rotate_ccw` | 画面逆时针旋转 90 度 |
| 16 | GET | `/api/get_rotation` | 获取当前旋转角度 |
| 17 | POST | `/api/set_rotation` | 直接设置旋转角度（0/90/180/270） |
| 18 | POST | `/api/reset_rotation` | 一键重置到 0 度（基准竖屏） |

### 动作执行（17 个）

| # | 方法 | 接口 | 说明 |
|---|------|------|------|
| 19 | POST | `/api/go_home` | Home 回到桌面 |
| 20 | POST | `/api/go_back` | Back 返回 |
| 21 | POST | `/api/go_forward` | Forward 前进 |
| 22 | POST | `/api/reset` | 机械臂复位 |
| 23 | POST | `/api/clear_overlay` | 清除画面覆盖框 |
| 24 | GET/POST | `/api/run_app` | 启动指定 APP |
| 25 | POST | `/api/swipe_up` | 上滑（大幅/小幅） |
| 26 | POST | `/api/swipe_down` | 下滑（大幅/小幅） |
| 27 | POST | `/api/swipe_up_normal` | 标准上划（成功率 ~80%） |
| 28 | POST | `/api/swipe_down_normal` | 标准下划 |
| 29 | POST | `/api/swipe` | 自定义滑动（指定起点/终点百分比） |
| 30 | POST | `/api/close_all_apps` | 关闭所有后台 APP |
| 31 | POST | `/api/click_icon` | 模板匹配点击图标 |
| 32 | POST | `/api/click_icons` | 依次点击多个图标 |
| 33 | POST | `/api/click_icon_many_times` | 同位置连点多次 |
| 34 | POST | `/api/click_text` | OCR 找文字并点击 |
| 35 | POST | `/api/click_at` | 画面像素坐标点击 |
| 36 | POST | `/api/click` | 手机百分比坐标点击 |
| 37 | POST | `/api/click_roi` | ROI 区域中心点击 |
| 38 | POST | `/api/screenshot` | 截图（保存文件 / 返回 base64） |
| 39 | POST | `/api/reload_gestures` | 重新加载手势配置 |

### 视觉检测（12 个）

| # | 方法 | 接口 | 说明 |
|---|------|------|------|
| 40 | POST | `/api/find_template` | 全屏模板匹配找图标 |
| 41 | POST | `/api/find_template_roi` | ROI 区域内模板匹配 |
| 42 | POST | `/api/find_text` | OCR 找文字 |
| 43 | POST | `/api/find_text_roi` | ROI 区域内 OCR 找文字 |
| 44 | POST | `/api/find_all_text` | 识别画面所有文字 |
| 45 | POST | `/api/find_all_templates` | 所有模板都必须匹配 |
| 46 | POST | `/api/find_any_template` | 任一模板匹配即可 |
| 47 | POST | `/api/count_template` | 统计模板出现次数 |
| 48 | POST | `/api/detect_desktop` | 检测当前桌面页面 |
| 49 | POST | `/api/detect_page` | 检测当前页面 |
| 50 | POST | `/api/wait_for_template` | 轮询等待模板出现 |
| 51 | POST | `/api/wait_for_page` | 轮询等待指定页面 |

### 脚本控制（4 个）

| # | 方法 | 接口 | 说明 |
|---|------|------|------|
| 52 | POST | `/api/run_script` | 执行 YAML 脚本（异步启动） |
| 53 | GET | `/api/script_status` | 查询脚本是否运行中 |
| 54 | GET | `/api/script_progress` | 查询脚本执行进度 |
| 55 | POST | `/api/stop_script` | 强制中断脚本 |

### 配置管理（6 个）

| # | 方法 | 接口 | 说明 |
|---|------|------|------|
| 56 | GET/PUT | `/api/config/daily` | 读取/更新每日自动化配置 |
| 57 | GET/PUT | `/api/config/app/<name>` | 读取/更新 APP 页面配置 |
| 58 | GET/PUT | `/api/config/gesture` | 读取/更新手势配置 |

### 系统管理（1 个）

| # | 方法 | 接口 | 说明 |
|---|------|------|------|
| 59 | POST | `/api/shutdown` | 优雅关闭服务 |

---

## 系统架构

```
用户/AI Agent
    │
    ├── HTTP API (POST/GET http://127.0.0.1:7826/api/*)
    │       控制机械臂、摄像头、脚本执行
    │
    └── YAML 脚本 (scripts/*.yaml)
            定义自动化操作流程（点击图标、找文字、循环、条件分支）
```

**致 AI Agent**：你无法直接看到摄像头画面，必须通过 API 来理解当前手机屏幕状态：
- `GET /api/get_frame_info` — 获取画面元数据
- `GET /api/is_phone_present` — 检测手机是否在画面中
- `GET /api/is_screen_on` — 检测屏幕是否点亮
- `POST /api/screenshot {"return_base64": true}` — 获取 base64 画面数据
- `POST /api/detect_page` — 检测当前页面名称

**核心原则**：
- 机械臂/摄像头/脚本引擎是**独占资源**，同一时间只能一个任务使用
- 所有视觉操作基于**模板匹配（图标）** 和 **OCR（文字）**
- **同步阻塞执行**：除 `/api/run_script` 外，所有 API 命令都是**同步阻塞**的。HTTP 请求发出后，必须等待服务器返回完整的 JSON 响应，才可发送下一个命令。响应中的 `"ok": true` 表示动作已完成（不是异步任务启动）。`run_script` 启动独立线程后台执行，立即返回启动状态；脚本通常耗时较长，启动后可通过 `script_status`（是否运行中）、`script_progress`（完整步骤日志）、`stop_script`（中断脚本）来监控和控制
- **资源保护**：脚本运行期间，动作命令会被自动拒绝并返回"脚本正在运行中"

| ✅ 脚本运行时可安全调用的 API | ❌ 脚本运行时会被拒绝的 API |
|---|---|
| `health`, `arm_status`, `script_status`, `script_progress` | `run_script`（同时只能一个脚本） |
| `get_frame_info`, `get_overlay`, `get_phone_corners` | `click`, `click_icon`, `click_text`, `click_at`, `click_roi` |
| `is_phone_present`, `is_screen_on` | `swipe`, `swipe_up`, `swipe_down`, `swipe_up_normal`, `swipe_down_normal` |
| `find_template`, `find_all_templates`, `find_any_template`, `count_template`（cv2 模板匹配，线程安全） | `go_home`, `go_back`, `go_forward` |
| `screenshot`（返回 base64 或保存文件） | `find_text`, `find_all_text`, `find_text_roi`（PaddleOCR 单例不保证线程安全） |
| `wait_for_template`（内部用 cv2 模板匹配） | `detect_desktop`, `detect_page`, `wait_for_page`（页面检测内部可能调用 OCR） |
| | `reset`, `clear_overlay`, `close_all_apps`, `run_app`, `reload_gestures` |

- 脚本是**解释执行**，不需要编译，改完即用
- 脚本执行完毕后机械臂自动复位

---

## 快速入门：Hello FlexArm

### 第 1 步：确认服务已启动

```bash
# 启动主服务
RobotArmServer.exe
# 默认监听端口: 7826
```

程序启动后会**自动检测并初始化**运行环境：
1. 检查机械臂 Windows 服务是否运行
2. 如未运行，尝试自动安装并启动服务
3. 自动检测机械臂 COM 口并连接
4. 自动加载最新标定文件（如无则启动自动标定）
5. 检查授权（未激活时弹出激活对话框）

> **注意**：如果是首次使用（从未安装过 Windows 服务），程序需要管理员权限才能自动安装服务。请以**管理员身份运行** `RobotArmServer.exe`。日常使用中如果服务已正常运行，则无需管理员权限。

> 服务安装失败时程序仍会启动，但机械臂不可用。可手动以管理员身份执行 `robot-arm-service\安装.bat` 安装服务。

### 第 2 步：测试连接

```bash
curl http://127.0.0.1:7826/api/health
# 返回: {"ok": true, "data": {"status": "running", ...}}
```

### 第 3 步：打开摄像头窗口，确认手机在画面中

```bash
curl -X POST http://127.0.0.1:7826/api/show_window -H "Content-Type: application/json" -d '{}'
```

窗口打开后，你应该能看到手机屏幕画面。按 ESC 关闭窗口。

> **AI Agent 专用：获取画面状态（无需看到窗口）**
>
> AI Agent 无法看到窗口画面，应使用以下 API 获取画面状态：
>
> ```bash
> # 获取画面宽高（元数据）
> curl http://127.0.0.1:7826/api/get_frame_info
> # 返回: {"ok":true,"data":{"width":960,"height":540}}
>
> # 检测手机是否在画面中（亮度检测）
> curl http://127.0.0.1:7826/api/is_phone_present
> # 返回: {"ok":true,"data":{"present":true}}
>
> # 检测屏幕是否点亮
> curl http://127.0.0.1:7826/api/is_screen_on
> # 返回: {"ok":true,"data":{"screen_on":true}}
>
> # 获取当前画面（base64 编码，可被 AI 解析）
> curl -X POST http://127.0.0.1:7826/api/screenshot -H "Content-Type: application/json" -d '{"return_base64":true,"phone_only":true}'
> # 返回: {"ok":true,"data":{"base64":"iVBORw0KGgoAAAANSUhEUg..."}}
>
> # 保存截图到文件
> curl -X POST http://127.0.0.1:7826/api/screenshot -H "Content-Type: application/json" -d '{"filename":"screenshots/test.png"}'
> # 返回: {"ok":true,"data":{"path":"E:\\robot_arm\\screenshots\\test.png"}}
>
> # 列出历史截图
> curl http://127.0.0.1:7826/api/screenshots?limit=5
> # 返回: [{"filename":"...","size":123456,"time":"2026-05-24 18:00:12"},...]
> ```

### 第 4 步：执行第一个动作 — 点击屏幕中央

```bash
curl -X POST http://127.0.0.1:7826/api/click \
  -H "Content-Type: application/json" \
  -d '{"x": 0.5, "y": 0.5}'
```

机械臂会自动移动并点击手机屏幕中央位置（`x: 0.5, y: 0.5` 为屏幕百分比坐标，0-1 范围）。

无需图标模板或配置，标定成功后即可直接使用。

### 第 5 步：写你的第一个脚本

> ⚠️ **重要**：所有 YAML 脚本文件必须使用 **UTF-8 无 BOM** 编码保存。带 BOM 的 UTF-8 会导致解析失败或中文参数乱码。

创建 `scripts/hello_flexarm.yaml`：

```yaml
name: hello_flexarm
description: "第一个 FlexArm 脚本 — 体验点击、滑动、等待等基本动作"

steps:
  # 1. 点击屏幕中央
  - action: click
    x: 0.5
    y: 0.5

  # 2. 等待 1 秒
  - action: wait
    seconds: 1

  # 3. 点击屏幕底部（触发返回键或手势导航）
  - action: click
    x: 0.5
    y: 0.95

  # 4. 等待 2 秒
  - action: wait
    seconds: 2

  # 5. 大幅上滑（翻页）
  - action: swipe_up
    large: true

  # 6. 等待 1 秒
  - action: wait
    seconds: 1

  # 7. 小幅下滑
  - action: swipe_down
    large: false

  # 8. 点击屏幕右上角（约返回/菜单区域）
  - action: click
    x: 0.85
    y: 0.1
```

运行：

```bash
curl -X POST http://127.0.0.1:7826/api/run_script \
  -H "Content-Type: application/json" \
  -d '{"path": "scripts/hello_flexarm.yaml"}'
```

> 以上示例仅需标定完成即可运行，不需要图标模板或页面配置。
>
> 要学习更高级、更智能的动作（图标点击、OCR 文字点击、页面切换、条件分支等），请继续阅读以下章节，了解如何配置图标模板和页面定义。

---

## 配置系统：让程序"认识"你的手机

### 手机桌面配置 — `scripts/configs/app_desktop.yaml`

**必须存在**，文件名固定为 `app_desktop.yaml`。程序通过它识别手机桌面的各个页面。

```yaml
app_name: 手机桌面

pages:
  - name: desktop_page0           # 页面名称，任意字符串
    min_match: 2                  # 至少匹配 2 个 features 才算通过
    must_features:                # 必选特征（全部通过才继续）
      - name: 电话
        type: image
        path: icons/app_phone.png
        mask: false               # false=4角背景采样，宽松匹配
      - name: 照相机
        type: image
        path: icons/app_camera.png
        mask: false
    features:                     # 可选特征（达到 min_match 即可通过）
      - name: 短信
        type: image
        path: icons/app_message.png
        mask: false
      - name: 设置
        type: image
        path: icons/app_settings.png
        mask: false

  - name: desktop_page1
    min_match: 3
    must_features:
      - name: 电话
        type: image
        path: icons/app_phone.png
        mask: false
      - name: 照相机
        type: image
        path: icons/app_camera.png
        mask: false
    features:
      - name: 汽水音乐
        type: image
        path: icons/app_qishui.png
        mask: false
      - name: 微信
        type: image
        path: icons/app_wechat.png
        mask: false

  - name: 任务卡                 # 多任务切换页面
    min_match: 0
    must_features:
      - name: 小窗显示
        type: image
        path: icons/task_show.png
        mask: false
      - name: 垃圾箱
        type: image
        path: icons/task_delete.png
        mask: false
    features: []
```

**关键字段说明**：
- `must_features`：全部必须匹配，否则跳过该页面
- `features`：可选特征，匹配数 >= `min_match` 即通过
- `mask: false`：使用 4 角背景采样，容忍图标大小/位置微变
- `mask: true`（或不写）：严格模板匹配，适合固定 UI 元素

### 页面配置 — `scripts/configs/app_xxx.yaml`

每个 APP 一个配置文件，定义该 APP 的所有可识别页面。

```yaml
app_name: 汽水音乐

pages:
  - name: 音乐
    min_match: 1
    must_features: []
    features:
      - name: 底部播放栏
        type: image
        path: icons/qishui/music_playing.png
        mask: false

  - name: 福利
    min_match: 1
    must_features:
      - name: 福利标题
        type: text
        text: "福利"
    features: []
```

> 脚本中的 `switch_page` 匹配不到任何页面时，会自动执行 `default` 分支，无需在配置文件中单独配置。

### 如何制作图标模板

1.凭授权码给开发者发送email至hamlet0168@163.com，可以获取辅助工具，包括屏幕截图，测试脚本等功能。

或者手动：用手机截图 → 裁剪出图标 → 放入 `icons/` 目录。

**图标要求**：
- PNG 格式
- 图标主体完整，边缘留 2-3px 空白，此边缘用于mask: true模式，会过滤到背景。 mask: false则严格匹配整图
- 不要包含动态内容（倒计时、动画）
- 文件名用小写英文 + 下划线，如 `app_qishui.png`

---

## YAML 脚本语言

### 基础结构

```yaml
name: 脚本名称
description: "脚本描述"

steps:
  - action: 动作类型
    参数1: 值1
    参数2: 值2
```

### 支持的动作

| action | 参数 | 说明 |
|--------|------|------|
| `click_icon` | `path`, `threshold`, `roi`, `mask` | 模板匹配点击图标 |
| `click_icons` | `paths`, `interval` | 依次点击多个图标（如拨号），每次点击后机械臂退出画面避免遮挡 |
| `click_icon_many_times` | `path`, `count`, `interval` | 找到图标后同位置连点多次，中间不复位 |
| `dial_number` | `number`, `interval` | 智能拨号（号码自动映射到数字图标，支持 `#` 和 `*`） |
| `click_text` | `text`, `roi`, `min_score` | OCR 找文字并点击 |
| `click` | `x`, `y` | 点击手机屏幕百分比坐标(0-1)，±30px 随机偏移 |
| `click_at` | `cam_x`, `cam_y` | 点击画面像素坐标（精准，无偏移） |
| `click_roi` | `roi`, `label` | 点击 ROI 区域中心（相对手机屏幕百分比） |
| `find_all_text` | `roi`, `min_score` | 识别画面所有文字，返回文字列表+位置+置信度 |
| `swipe` | `sx`, `sy`, `ex`, `ey`, `steps`, `step_wait_ms` | 自定义滑动（指定起点/终点百分比） |
| `swipe_up` | `large: true/false` | 上滑（大幅/小幅） |
| `swipe_down` | `large: true/false` | 下滑（大幅/小幅） |
| `swipe_up_normal` | 无 | 标准上划（成功率 ~80%） |
| `swipe_down_normal` | 无 | 标准下划 |
| `go_home` | `max_retries` | 返回桌面（检测+循环上滑直到回到桌面） |
| `go_back` | 无 | Back 返回键 |
| `go_forward` | 无 | 前进键 |
| `reset` | 无 | 机械臂复位 |
| `clear_overlay` | 无 | 清除画面覆盖框 |
| `run_app` | `app_name` | 运行指定 APP（go_home→检测页面→翻页→点击图标） |
| `close_all_apps` | `max_swipes` | 关闭所有后台 APP |
| `screenshot` | `filename`, `phone_only`, `show_board`, `return_base64` | 截图 |
| `reload_gesture` | 无 | 重新加载手势配置（gesture_config.json 热更新） |
| `set_video_to_coin` | `value` | 设置看视频赚金币模式 |
| `rotate_cw` | 无 | 画面顺时针旋转 90 度 |
| `rotate_ccw` | 无 | 画面逆时针旋转 90 度 |
| `set_rotation` | `angle` | 直接设置旋转角度（0/90/180/270） |
| `reset_rotation` | 无 | 一键重置到 0 度（基准竖屏） |
| `wait` | `seconds` | 等待（支持区间 `2-5`） |
| `loop` | `count`, `steps` | 循环执行子步骤（支持随机区间 `count: 3-5`） |
| `if_found` | `type`, `path`/`text`, `then`, `else` | 条件：找到目标则执行 then，否则 else |
| `if_found_roi` | `type`, `path`/`text`, `roi`, `then`, `else` | 同上，但限定 ROI 区域内查找 |
| `if_progress_stop` | `template`, `roi`, `then`, `else` | 进度条停顿检测（连续 N 次变化 < 阈值判定停顿） |
| `if_video_to_coin` | `then`, `else` | 根据赚金币模式状态执行分支 |
| `if_random` | `chance`, `then`, `else` | 随机概率分支 |
| `detect_desktop` | `config` | 检测当前是否在桌面（不断言，仅返回结果） |
| `detect_page` | `config` | 检测当前页面名称（不断言） |
| `is_screen_on` | 无 | 判断屏幕是否亮起 |
| `assert_desktop` | `config` | 必须在桌面，否则报错停止 |
| `switch_page` | `config`, `cases` | 检测页面 → 匹配 cases 分支 → 无匹配执行 default |
| `run_script` | `path` | 执行子脚本（同步，完成后回到父级） |
| `stop_loop` | 无 | 中断当前 loop，继续执行循环后步骤 |
| `stop_script` | 无 | 停止当前脚本层级（子脚本中只停子脚本） |
| `log` | `message` | 打印日志 |

### 参数详解

#### click_icon

```yaml
- action: click_icon
  path: icons/app_qishui.png     # 图标路径（相对项目根目录）
  threshold: 0.75                # 匹配阈值（默认 0.75）
  roi: [0.1, 0.2, 0.5, 0.6]     # 搜索区域 [sx, sy, ex, ey]（相对手机屏幕，0-1）
  mask: false                    # false=宽松，true=严格（默认 true）
```

#### click_text

```yaml
- action: click_text
  text: "领取"                   # 要查找的文字
  roi: [0.3, 0.5, 0.7, 0.8]     # 搜索区域（可选）
  min_score: 0.5                 # OCR 最低置信度（默认 0.3）
```

#### click（百分比坐标）

```yaml
- action: click
  x: 0.5                         # X 轴百分比（0=左，1=右）
  y: 0.96                        # Y 轴百分比（0=上，1=下）
```

#### loop（循环）

```yaml
- action: loop
  count: 10                      # 固定次数
  # count: 3-5                   # 也支持随机区间
  steps:
    - action: click_text
      text: "领取"
    - action: wait
      seconds: 2
```

#### if_found（条件分支）

```yaml
- action: if_found
  type: image/text                   # image=模板匹配, text=OCR
  path: icons/qishui/cross.png   # type=image 时用
  text: "继续观看"               # type=text 时用
  roi: [0.7, 0.0, 1.0, 0.15]    # 搜索区域（可选）
  then:
    - action: click_icon
      path: icons/qishui/cross.png
      roi: [0.7, 0.0, 1.0, 0.15]    # 搜索区域（可选）如果if_found指定了区域，那么点击也一定指定，防止画面有多个相同元素，误点其他的
  else:
    - action: wait
      seconds: 2
```

#### if_random（随机分支）

```yaml
- action: if_random
  chance: 0.4                    # 40% 概率走 then
  then:
    - action: log
      message: "走 then 分支"
  else:
    - action: log
      message: "走 else 分支"
```

#### switch_page（页面切换）

```yaml
- action: switch_page
  config: scripts/configs/app_qishui.yaml   # 页面配置文件
  cases:
    音乐:                                      # 匹配到"音乐"页面时
      - action: click
        x: 0.5
        y: 0.96
    福利:
      - action: click_text
        text: "福利"
    default:                                 # 未匹配任何页面时
      - action: swipe_up
        large: true
```

### 脚本嵌套

```yaml
steps:
  - action: run_script
    path: qishui/run_ad_card.yaml     # 执行子脚本，进入run_ad_card.yaml脚本内部依次执行动作
  - action: wait		# 脚本内部嵌套执行脚本，是串行机制，上一个脚本执行完毕，才轮到此动作执行
    seconds: 5
```

子脚本执行完毕后，自动回到父脚本继续。

### 随机延时

```yaml
- action: wait
  seconds: 2-5          # 随机等待 2~5 秒
```

### 执行模型

- 脚本**顺序执行**，每个动作完成后才执行下一个
- `loop` 内的动作循环指定次数
- `switch_page` 会遍历所有页面定义直到匹配
- `run_script` 是子调用，完成后回到父级
- `stop_loop` 中断当前 loop
- `stop_script` 停止当前层级（在子脚本中调用只停子脚本）
- 脚本结束或出错后，机械臂自动复位

---

## HTTP API 参考

**基础信息**：
- 地址：`http://127.0.0.1:7826`
- 所有 POST 接口 Body 为 JSON
- 成功返回：`{"ok": true, "data": {...}}`
- 失败返回：`{"ok": false, "error": "错误信息"}`

### 1. 健康检查

```
GET /api/health
```

返回服务状态、端口、运行时间等。

### 2. 机械臂状态

```
GET /api/arm_status
```

返回机械臂 COM 口、连接状态、移动范围等。

### 3. 摄像头画面

#### 获取画面信息

```
GET /api/get_frame_info
```

返回：
```json
{"ok": true, "data": {"width": 540, "height": 960, "fps": 29.5}}
```

#### 显示/隐藏窗口

```
POST /api/show_window
POST /api/hide_window
```

> **提示**：打开窗口后，支持完整键盘/鼠标交互功能！
> 详细操作请参考项目根目录下的 `PM/画面窗口交互功能说明.md` 文档。

#### 检测手机是否在画面中

```
GET /api/is_phone_present
GET /api/is_phone_present?bright_threshold=60&bright_ratio=0.08
```

返回：
```json
{"ok": true, "data": {"present": true}}
```

#### 检测屏幕是否点亮

```
GET /api/is_screen_on
GET /api/is_screen_on?dark_threshold=30&dark_ratio=0.7
```

返回：
```json
{"ok": true, "data": {"screen_on": true}}
```

#### 切换手机外框

```
POST /api/toggle_phone_corners
```

在画面窗口上叠加绿色手机屏幕边框。

#### 画面旋转

```
POST /api/rotate_cw                    # 顺时针旋转 90 度
POST /api/rotate_ccw                   # 逆时针旋转 90 度
GET  /api/get_rotation                 # 获取当前角度
POST /api/set_rotation {"angle": 180}  # 直接设置角度（0/90/180/270）
POST /api/reset_rotation               # 一键重置到 0 度
```

旋转只改变视觉显示，不影响基准坐标系（始终是 0°竖屏）。点击画面时会自动转换坐标。

#### 截图

```
POST /api/screenshot {"path": "screenshots/test.png"}    # 保存到文件
POST /api/screenshot {"return_base64": true}             # 返回 base64（AI Agent 推荐）
POST /api/screenshot {"phone_only": true}                # 只截取手机区域
POST /api/screenshot {"show_board": true}                # 带标尺全景
```

#### 列出历史截图

```
GET /api/screenshots
GET /api/screenshots?limit=10
```

返回：
```json
{"ok":true,"data":[{"filename":"20260524_1800_phone.png","path":"E:\\robot_arm\\screenshots\\...","size":123456,"time":"2026-05-24 18:00:12"},...]}
```

### 4. 页面检测

#### 检测桌面

```
POST /api/detect_desktop {"desktop_config": "scripts/configs/app_desktop.yaml"}
```

返回：
```json
{"ok": true, "data": {"matched": true, "page_name": "desktop_page1", "score": 0.84}}
```

#### 检测指定页面

```
POST /api/detect_page {"config_path": "scripts/configs/app_qishui.yaml", "threshold": 0.75}
```

返回：
```json
{"ok": true, "data": {"matched": true, "page_name": "福利", "score": 0.82}}
```

### 5. 视觉查找

#### 查找图标

```
POST /api/find_template
{"path": "icons/app_qishui.png", "threshold": 0.75, "roi": [0.1, 0.2, 0.5, 0.6], "auto_mask": false}
```

返回：
```json
{"ok": true, "data": {"x": 257, "y": 453, "w": 52, "h": 53, "score": 0.9446}}
```

#### 查找文字

```
POST /api/find_text {"text_keyword": "领取", "roi": [0.3, 0.5, 0.7, 0.8], "min_score": 0.5}
```

返回：
```json
{"ok": true, "data": {"x": 300, "y": 600, "w": 40, "h": 20, "text": "领取奖励", "score": 0.91}}
```

#### ROI 区域内查找文字

```
POST /api/find_text_roi {"roi": [0.0, 0.6, 1.0, 1.0], "text_keyword": "夸克", "min_score": 0.3}
```

与 `find_text` 类似，但必须指定 `roi`（数组格式 `[sx, sy, ex, ey]`，0-1）。

#### ROI 区域内模板匹配

```
POST /api/find_template_roi {"path": "icons/app_qishui.png", "roi": [0.1, 0.2, 0.5, 0.6], "threshold": 0.75}
```

与 `find_template` 类似，但必须指定 `roi` 区域。

#### 调整摄像头对焦

```
POST /api/change_focus {"value": 2}     # 拉近 +2
POST /api/change_focus {"value": -2}    # 拉远 -2
```

增量调整对焦值（边界 0~500），返回 `{"ok": true, "data": {"focus": 310.0}}`。

#### 查找所有文字

```
POST /api/find_all_text {"min_score": 0.5}
```

返回画面中所有识别到的文字列表。

**性能警告**：`find_all_text` 默认全屏 OCR 扫描，使用 CPU 推理，耗时取决于画面文字密度：
- 文字少（<20 条）：~15 秒
- 文字密集（小说 APP 等）：90~120 秒以上

**最佳实践**：尽量使用 `find_text` 指定 `roi` 区域搜索，速度快数个数量级。

#### 查找多个图标

```
POST /api/find_all_templates {"template_paths": ["icons/a.png", "icons/b.png"], "threshold": 0.75}
```
所有图标都检测到才返回 true。

#### 查找任一图标

```
POST /api/find_any_template {"template_paths": ["icons/a.png", "icons/b.png"], "threshold": 0.75}
```

返回第一个匹配到的图标。

### 6. 动作执行

#### 返回桌面

```
POST /api/go_home {"max_retries": 5}
```

`max_retries`：最大重试次数，默认 5 次（检测桌面 → 不在则上滑 → 再检测）。

#### 返回/前进

```
POST /api/go_back
POST /api/go_forward
```

#### 复位

```
POST /api/reset
```

#### 上滑/下滑

```
POST /api/swipe_up {"large": true}     # 大幅上滑
POST /api/swipe_down {"large": false}  # 小幅下滑
```

#### 点击图标

```
POST /api/click_icon
{"path": "icons/app_qishui.png", "threshold": 0.75, "roi": [0.1, 0.2, 0.5, 0.6], "mask": false, "reset": true}
```

#### 依次点击多个图标

```
POST /api/click_icons
{"paths": ["icons/phone/num1.png", "icons/phone/num3.png", "icons/phone/num2.png"], "interval": 1}
```

依次点击路径列表中的图标，每次点击后机械臂自动退出画面避免遮挡下一个图标，全部点完后复位。返回 `{"ok": true, "data": {"clicked": true, "success_count": N, "failed": []}}`。

#### 同位置连点多次

```
POST /api/click_icon_many_times
{"path": "icons/qishui/like.png", "count": 3, "interval": 0.5}
```

只搜索一次图标，在同位置连点多次，中间不复位不移动，全部点完后复位。返回 `{"ok": true, "data": {"clicked": true, "clicks": 3}}`。

#### 点击文字

```
POST /api/click_text
{"text": "领取", "roi": [0.3, 0.5, 0.7, 0.8], "min_score": 0.5}
```

#### 点击坐标

```
POST /api/click {"x": 0.5, "y": 0.96}
```

#### 点击区域中心

```
POST /api/click_roi {"roi": [0.3, 0.5, 0.7, 0.8]}
```

#### 自定义滑动

```
POST /api/swipe
{"sx": 0.5, "sy": 0.8, "ex": 0.5, "ey": 0.1, "steps": 5, "duration": 0.3}
```

`sx/sy/ex/ey` 是相对手机屏幕的百分比坐标，`steps` 是步数，`duration` 是滑动总时长（秒）。

#### 关闭所有 APP

```
POST /api/close_all_apps {"max_swipes": 15}
```

#### 运行指定 APP

```
GET /api/run_app?app_name=汽水音乐
# 或
POST /api/run_app {"app_name": "汽水音乐"}
```

通过 `app_desktop.yaml` 查找 APP 图标并点击。

### 7. 脚本控制

#### 运行脚本

```
POST /api/run_script {"path": "scripts/qishui_daily.yaml"}
```

返回：
```json
{"ok": true, "data": {"script": "D:\\...\\scripts/qishui_daily.yaml", "status": "started"}}
```

#### 查询脚本状态

```
GET /api/script_status
```

返回：
```json
{"ok": true, "data": {"running": false, "current_script": null}}
```

#### 查询脚本进度

```
GET /api/script_progress
```

返回完整执行日志 + 统计信息：

```json
{
  "ok": true,
  "data": {
    "running": true,
    "script": "scripts/xxx.yaml",
    "current_step": {"step_index": 5, "action": "switch_page", "target": "拨号页", "status": "ok", "detail": "匹配分支: 拨号页", "timestamp": 1780329450.91},
    "steps_log": [
      {"step_index": 0, "action": "script_start", "target": "test_66", "status": "ok", "detail": "5 个顶层步骤", "timestamp": ...},
      ...
    ],
    "stats": {
      "total_steps": 24,
      "completed_steps": 23,
      "failed_steps": 0,
      "elapsed": 61.6,
      "status": "running"
    }
  }
}
```

- `steps_log`：完整步骤历史，每一步都有时间戳
- `stats`：统计信息，包括耗时、完成数、失败数
- 脚本空闲时：`{"running": false, "script": null, "current_step": null, "steps_log": [], "stats": {}}`

#### 停止脚本

```
POST /api/stop_script
```

#### 轮询等待模板出现

```
POST /api/wait_for_template {"path": "icons/qishui/reward_popup.png", "timeout": 10, "interval": 0.5}
```

轮询等待直到模板出现或超时，`timeout` 秒内每 `interval` 秒检查一次。

#### 轮询等待页面

```
POST /api/wait_for_page {"config_path": "scripts/configs/app_xxx.yaml", "target_name": "福利页面", "timeout": 15}
```

轮询等待直到指定页面出现或超时。

#### 清除覆盖层

```
POST /api/clear_overlay
```

### 8. 配置管理

#### 读取/更新每日任务配置

```
GET  /api/config/daily
PUT  /api/config/daily  {"windows": [...]}
```

#### 读取/更新 APP 页面配置

```
GET  /api/config/app/qishui           # 返回 app_qishui.yaml 内容
PUT  /api/config/app/qishui           # 更新配置（Body 为 YAML 文本）
```

#### 读取/更新手势配置

```
GET  /api/config/gesture
PUT  /api/config/gesture  {...}
```

### 9. 关机

```
POST /api/shutdown
```

优雅关闭服务：检测脚本运行 → 安全终止 → 机械臂复位 → 释放资源 → 进程退出。

> **注意**：不要直接杀进程，否则 COM 口未释放，下次启动需重新安装驱动。

### 10. 系统托盘

RobotArmServer.exe 启动后会自动最小化到系统托盘：
- **左键点击/双击**：显示/隐藏控制台窗口
- **右键菜单**："显示/隐藏控制台"、"退出服务"
- 托盘退出 = API `/api/shutdown`（同样执行优雅关闭流程）

---

## 目录结构

```
RobotArmServer/
├── RobotArmServer.exe          ← 主程序
├── _internal/                  ← 程序库（不要动）
├── scripts/                    ← YAML 脚本目录
│   ├── hello_flexarm.yaml      ← 你的第一个脚本
│   ├── daily_config.yaml       ← 每日定时任务配置	# 暂不开放，任何AI agent都能具备自主处理定时任务的功能。
│   ├── configs/
│   │   ├── app_desktop.yaml    ← 手机桌面配置（必须）
│   │   ├── app_qishui.yaml     ← 汽水音乐页面配置
│   │   └── app_kuaishou.yaml   ← 快手极速版页面配置
│   └── qishui/                 ← 子脚本目录
│       ├── run_*.yaml
│       └── music_actions.yaml
├── icons/                      ← 图标模板目录
│   ├── app_phone.png
│   ├── app_camera.png
│   ├── app_qishui.png
│   └── qishui/
│       ├── cross.png
│       └── ...
├── calibrations/               ← 标定结果（标定系统自动生成）
├── screenshots/                ← 截图保存目录
├── camera_config.json          ← 摄像头对焦配置
├── device_config.json          ← 设备配置
├── gesture_config.json         ← 手势滑动配置
└── robot-arm-service/          ← Windows 服务驱动
```

---

## 常见问题

### Q: 机械臂不动？

1. 确认 `robot-arm-service/安装.bat` 已以管理员身份运行
2. 确认 `GET /api/arm_status` 返回 `"connected": true`
3. 确认标定已完成（`calibrations/` 目录下有 JSON 文件）

### Q: 找不到图标？

1. 确认图标文件在 `icons/` 目录下
2. 降低 `threshold`（如 0.65）
3. 设置 `mask: false`（4 角背景采样，更宽松）
4. 指定 `roi` 缩小搜索区域
5. 检查图标模板是否与画面中的图标一致（大小、颜色、背景）

### Q: 找不到文字？

1. 提高画面清晰度（调整摄像头对焦：`POST /api/change_focus {"value": 5}`）
2. 提高 `min_score` 到 0.5 以上减少误匹配
3. 指定 `roi` 缩小搜索区域
4. 使用 `find_all_text` 查看画面中实际识别到的文字

### Q: 脚本执行中断？

1. 查看 `GET /api/script_progress` 确认卡在哪一步
2. 检查日志输出（服务控制台）
3. 确认手机没有弹出系统弹窗（权限、通知等）

### Q: 如何添加新 APP 的自动化？

1. 截取 APP 图标 → 放入 `icons/` 目录
2. 更新 `scripts/configs/app_desktop.yaml`（添加新图标到 features）
3. 创建 `scripts/configs/app_xxx.yaml`（定义 APP 内各页面）
4. 编写 `scripts/run_xxx.yaml`（定义操作流程）
5. 运行：`POST /api/run_script {"path": "scripts/run_xxx.yaml"}`

---

## 错误处理指南

所有 API 返回统一格式：`{"ok": true/false, "data": {...}, "error": "..."}`

### 常见错误及处理策略

| 错误 | 原因 | 处理方式 |
|------|------|----------|
| `"error": "脚本正在运行中"` | 有脚本在后台执行 | 查询 `script_status` 确认，等待完成或调用 `stop_script` |
| `"error": "RobotActions 未初始化"` | 机械臂未连接/服务未启动 | 提示用户检查 `robot-arm-service/安装.bat` |
| `"error": "缺少参数: path"` | 请求参数不完整 | 检查 API 调用参数 |
| `"ok": false, "data": null` (find_template) | 图标未找到 | 降低 `threshold` 或检查图标文件，**不要无限重试** |
| `"ok": false, "data": null` (find_text) | OCR 未找到文字 | 扩大 `roi` 或降低 `min_score`，最多尝试 2-3 次后报错 |
| `"error": "未授权"` | 授权验证未通过 | 提示用户进行授权激活 |

### Agent 重试建议

- **图标查找**：失败 → 降低 threshold → 再试一次 → 仍失败 → 报告用户
- **文字查找**：失败 → 扩大 ROI → 再试 1 次 → 仍失败 → 报告用户
- **页面检测**：不匹配 → 尝试 `go_back` 或 `go_home` → 重新检测 → 仍不匹配 → 报告用户
- **脚本冲突**：收到"脚本运行中" → 查询 `script_status` → 如正在运行则等待或报告用户

---

## Agent 对话示例：找到并打开汽水音乐

以下展示一个 AI Agent 如何组合 API 完成"在桌面上找到并打开汽水音乐 APP"的完整流程：

**Step 1**: 确认服务状态
```bash
curl http://127.0.0.1:7826/api/health
# 返回: {"ok":true,"data":{"status":"running","arm_connected":true,...}}
```

**Step 2**: 检测当前是否在桌面
```bash
curl -X POST http://127.0.0.1:7826/api/detect_desktop -H "Content-Type: application/json" -d '{}'
# 返回: {"ok":true,"data":{"page_name":"desktop_page1","score":0.94,"matched":true}}
```

**Step 3**: 查找汽水音乐图标
```bash
curl -X POST http://127.0.0.1:7826/api/find_template -H "Content-Type: application/json" -d '{"path":"icons/app_qishui.png","threshold":0.75}'
# 返回: {"ok":true,"data":{"x":242,"y":516,"w":55,"h":55,"score":0.92}}
```

**Step 4**: 点击图标
```bash
curl -X POST http://127.0.0.1:7826/api/click_icon -H "Content-Type: application/json" -d '{"path":"icons/app_qishui.png"}'
# 返回: {"ok":true,"data":{"clicked":true,"score":0.92,...}}
```

**Step 5**: 等待 APP 启动，检测页面
```bash
python -c "import requests,time; time.sleep(2)"
python -c "import requests; r=requests.post('http://127.0.0.1:7826/api/detect_page',json={'config_path':'scripts/configs/app_qishui.yaml'}); print(r.text)"
# 返回: {"ok":true,"data":{"page_name":"音乐","score":0.85,"matched":true}}
```

**Step 6**: 确认手机在画面中
```bash
curl http://127.0.0.1:7826/api/is_phone_present
# 返回: {"ok":true,"data":{"present":true}}
```

✅ 任务完成：汽水音乐已打开，当前在音乐页面。

或者更直接的，确认app_desktop.yaml已配置正确，汽水音乐图标已存在，可以直接使用API接口 run_app。该接口会更加智能，自动翻页，找到APP所在的桌面页，并直接点击。
