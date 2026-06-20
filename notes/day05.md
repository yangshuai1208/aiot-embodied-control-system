# Day05 学习记录

## 今日目标

- 新建 aiot-embodied-control-system 系统级仓库
- 编写旗舰项目 README 初版
- 设计 AIoT 智能眼镜 + 具身智能灵动手控制系统总架构
- 明确 aiot-smart-glasses、linux-iot-gateway、embodied-robotic-hand、esp32-mqtt-ota 四个模块职责
- 完成 GitHub 提交

## 今日完成

- 新建 aiot-embodied-control-system 项目仓库
- 编写 README.md，说明项目简介、系统目标、总体架构和数据流
- 新增 docs/system_architecture.md，绘制系统总架构图
- 新增 docs/module_design.md，明确各模块职责
- 新增 docs/roadmap.md，规划 Day5-Day30 项目路线
- 明确系统 MVP 闭环：智能眼镜识别动作 → MQTT 上传 → Linux 网关解析 → UART 转发 → STM32 灵动手执行

## 今日代码提交

- commit: feat: create aiot embodied control system project

## 实验现象

- 项目目录结构清晰
- README.md 中可以看到系统整体架构
- docs 中可以看到系统架构、模块设计和项目路线图
- 项目从想法阶段进入正式工程阶段

## 遇到的问题

1. 现象：旗舰项目涉及智能眼镜、Linux 网关、灵动手、OTA，模块较多
2. 原因：系统是多端嵌入式项目，如果不先设计架构，后续代码容易混乱
3. 解决：先建立系统级仓库，用 README 和 docs 明确系统边界、模块职责和数据流

## 面试可讲点

- 本项目是一个 AIoT 智能眼镜控制具身智能灵动手的系统级项目
- 智能眼镜作为可穿戴控制端，负责姿态识别和命令生成
- Linux IoT Gateway 作为中间层，负责 MQTT 接收、JSON 解析、协议转换和日志记录
- STM32 灵动手作为执行端，负责舵机控制、动作库、状态机和安全限幅
- ESP32 OTA 作为后续升级能力，用于体现智能硬件远程升级思路
- 项目采用 MQTT、UART、JSON、自定义协议完成多端通信

## 明日计划

- 设计智能眼镜到网关的数据格式
- 设计网关到灵动手的 UART 控制协议
- 设计 JSON 数据格式
- 设计自定义帧格式