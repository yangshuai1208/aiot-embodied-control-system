# 模块设计说明

## 一、aiot-smart-glasses 智能眼镜控制端

### 核心职责

- 采集 MPU6050 姿态数据
- 判断 LEFT / RIGHT / NOD / SHAKE
- OLED 显示当前模式和手势
- 按键切换 NORMAL / CONTROL / SETTING
- 生成 JSON 控制命令
- 通过 ESP32-S3 / MQTT 上传

### MVP 功能

- 姿态识别
- OLED 状态显示
- 按键模式切换
- 串口或 MQTT 输出手势命令

---

## 二、linux-iot-gateway Linux 网关

### 核心职责

- 订阅 MQTT Topic
- 接收智能眼镜 JSON 数据
- 解析 gesture / cmd 字段
- 将手势映射为灵动手命令
- 通过串口发送自定义控制帧
- 保存系统日志

### MVP 功能

- MQTT 接收
- JSON 解析
- UART 命令发送
- 日志记录

---

## 三、embodied-robotic-hand 灵动手执行端

### 核心职责

- 接收 UART 控制命令
- 解析自定义协议帧
- 执行 OPEN / GRAB / RELEASE
- 控制 PCA9685 输出 PWM
- 管理多舵机角度
- 加入安全限幅和异常保护

### MVP 功能

- 单舵机控制
- 多舵机动作库
- 动作状态机
- 安全限幅

---

## 四、esp32-mqtt-ota OTA 扩展模块

### 核心职责

- ESP32-S3 连接 WiFi
- ESP32-S3 连接 MQTT Broker
- 发布手势 JSON
- 学习 OTA 基础流程

### OTA 流程

```text
版本检查
    ↓
固件下载
    ↓
固件校验
    ↓
重启切换
    ↓
失败回滚