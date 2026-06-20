# Day07 学习记录

## 今日目标

- 完成第 1 周项目总结
- 总结 env-monitor-v2 技术点
- 总结 linux-iot-gateway 技术点
- 总结 aiot-embodied-control-system 系统架构
- 准备项目规划截图或简短演示说明
- 完成 GitHub 提交

## 今日完成

- 新增 weekly/week01.md
- 总结 Day1-Day6 每天完成内容
- 梳理 STM32 + FreeRTOS 环境监测项目技术点
- 梳理 Linux IoT Gateway 项目技术点
- 梳理 AIoT 智能眼镜 + 具身智能灵动手系统架构
- 总结 MQTT JSON + UART 自定义帧通信协议
- 明确第 2 周智能眼镜控制端 MVP 任务

## 今日代码提交

- commit: docs: add week1 project architecture summary

## 实验现象

- weekly/week01.md 中可以看到第 1 周完整总结
- README.md 中可以补充第 1 周成果入口
- 项目结构更加适合 GitHub 展示
- 后续可以直接进入第 2 周智能眼镜控制端开发

## 遇到的问题

1. 现象：第 1 周涉及多个仓库和多个项目，内容较分散  
2. 原因：env-monitor-v2、linux-iot-gateway 和 aiot-embodied-control-system 分别属于不同层级项目  
3. 解决：通过 weekly/week01.md 按 Day1-Day6 顺序总结，并统一归纳为“已有项目整理 + 旗舰系统架构设计”

## 面试可讲点

- 第 1 周主要完成已有项目工程化整理和旗舰项目系统设计
- env-monitor-v2 展示 STM32 + FreeRTOS 多任务项目能力
- linux-iot-gateway 展示 Linux C、串口、TCP、MQTT 和日志能力
- aiot-embodied-control-system 展示 AIoT 系统级架构设计能力
- 系统采用 MQTT JSON + UART 自定义帧实现智能眼镜、Linux 网关和 STM32 灵动手之间的通信
- Linux 网关承担协议转换和日志记录职责，STM32 执行端专注实时控制

## 明日计划

- 新建 aiot-smart-glasses 仓库
- 编写智能眼镜控制端 README 初版
- 确定硬件组成：STM32 / ESP32、MPU6050、OLED、按键、蜂鸣器
- 开始第 2 周智能眼镜控制端 MVP