# Week01 项目总结

## 一、本周目标

第 1 周主要目标是完成已有项目整理，并确定 AIoT 智能眼镜 + 具身智能灵动手控制系统的总架构。

本周重点不是大量写新功能，而是把已有项目变成可以展示、可以讲解、可以写进简历的工程项目。

## 二、本周完成内容

### Day1：整理 env-monitor-v2 项目

完成内容：

- 整理 STM32 + FreeRTOS 环境监测项目仓库
- 补充 README.md 项目介绍
- 写清楚硬件组成、项目功能和 FreeRTOS 任务划分
- 新增 notes/day01.md
- 完成 GitHub 提交

项目核心功能：

- DHT11 温湿度采集
- OLED 实时显示
- EC11 页面切换
- LED / 蜂鸣器状态提示
- W25Q64 历史数据存储
- FreeRTOS 多任务调度

### Day2：补充 env-monitor-v2 系统架构

完成内容：

- 新增 docs/architecture.md
- 新增 docs/freertos_tasks.md
- 绘制 STM32 + FreeRTOS 系统架构图
- 梳理 sensor_task、oled_task、uart_task、alarm_task、storage_task
- 说明 FreeRTOS 队列如何传递温湿度数据

核心技术点：

- FreeRTOS 任务划分
- 队列通信
- 模块化驱动
- 任务解耦
- 数据流设计

### Day3：整理 linux-iot-gateway 项目

完成内容：

- 在 Ubuntu / WSL 中找到原 Linux 学习项目目录
- 定位到 linux-learning-submit 中的网关相关代码
- 整理 linux-iot-gateway 正式仓库
- 补充 README.md
- 写清楚串口、TCP、MQTT、日志模块

核心技术点：

- Linux 串口 termios
- TCP Server
- MQTT 发布订阅
- 日志模块
- Makefile 多文件编译

### Day4：补充 Linux 网关运行说明和测试记录

完成内容：

- 新增 docs/build_run.md
- 新增 docs/test_record.md
- 补充 make、make clean、./iot_gateway 运行说明
- 整理串口测试、TCP 测试、MQTT 测试现象
- 明确 Linux 网关在后续系统中的中间层作用

核心技术点：

- Linux C 编译运行
- 串口权限处理
- mosquitto_pub / mosquitto_sub
- nc TCP 客户端测试
- 日志记录和问题定位

### Day5：设计旗舰项目总架构

完成内容：

- 新建 aiot-embodied-control-system 系统级仓库
- 编写 README.md 初版
- 新增 docs/system_architecture.md
- 新增 docs/module_design.md
- 新增 docs/roadmap.md
- 明确四个核心模块：
  - aiot-smart-glasses
  - linux-iot-gateway
  - embodied-robotic-hand
  - esp32-mqtt-ota

系统总架构：

```text
AIoT 智能眼镜控制端
    ↓ MQTT JSON
Linux IoT Gateway
    ↓ UART 自定义协议
STM32 灵动手执行端
    ↓ PCA9685
MG90S 舵机 / 灵动手结构件