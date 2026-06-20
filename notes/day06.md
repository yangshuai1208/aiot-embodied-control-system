# Day06 学习记录

## 今日目标

- 设计智能眼镜到 Linux 网关的数据格式
- 设计 Linux 网关到 STM32 灵动手的控制协议
- 设计 MQTT JSON 数据格式
- 设计 UART 自定义帧格式
- 明确 gesture 到 cmd 的映射关系
- 完成 GitHub 提交

## 今日完成

- 新增 docs/protocol_design.md
- 设计智能眼镜到网关的 MQTT Topic：glasses/gesture
- 设计智能眼镜上传 JSON 格式
- 设计 Linux 网关到 STM32 的 UART 自定义帧格式
- 定义 OPEN / GRAB / RELEASE / STOP 命令字
- 设计 CHECKSUM 简单校验方式
- 设计 Linux 网关解析流程和 STM32 接收流程
- 更新 README.md 中的通信协议设计章节

## 今日代码提交

- commit: feat: design communication protocol for embodied control system

## 实验现象

- README.md 中可以看到通信协议设计
- docs/protocol_design.md 中可以看到完整 JSON 和 UART 协议
- 系统数据流从“动作识别”变成了“可传输、可解析、可执行的命令协议”
- 后续智能眼镜、Linux 网关和 STM32 灵动手可以按统一协议开发

## 遇到的问题

1. 现象：智能眼镜、Linux 网关、STM32 三端之间协议不统一，容易导致后续联调混乱  
2. 原因：MQTT 适合传 JSON，但 STM32 不适合直接解析复杂 JSON  
3. 解决：采用两段式协议设计。智能眼镜到 Linux 网关使用 MQTT JSON，Linux 网关到 STM32 使用短小的 UART 自定义帧

## 面试可讲点

- 系统采用 MQTT JSON + UART 自定义帧的两段式协议
- MQTT JSON 用于智能眼镜到 Linux 网关，优点是可读性强、方便调试、适合物联网通信
- UART 自定义帧用于 Linux 网关到 STM32，优点是短小稳定、STM32 解析简单
- Linux 网关承担协议转换职责，降低 STM32 执行端复杂度
- 帧格式包含帧头、命令字、长度、数据、校验和帧尾
- CHECKSUM 用于基础数据校验，异常帧会被丢弃
- gesture 到 cmd 的映射让系统动作逻辑更加清晰

## 明日计划

- 写 week01.md
- 总结 env-monitor-v2 和 linux-iot-gateway 技术点
- 总结旗舰项目架构和通信协议
- 准备项目规划截图或简短演示说明