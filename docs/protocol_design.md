# 通信协议设计说明

## 一、协议设计目标

本系统由 AIoT 智能眼镜控制端、Linux IoT Gateway 和 STM32 灵动手执行端组成。

通信协议需要解决三个问题：

1. 智能眼镜如何把手势动作发送给 Linux 网关
2. Linux 网关如何解析手势并生成控制命令
3. Linux 网关如何通过串口控制 STM32 灵动手执行动作

整体通信链路：

```text
AIoT 智能眼镜
    ↓ MQTT JSON
Linux IoT Gateway
    ↓ UART 自定义帧
STM32 灵动手执行端
```

---

## 二、智能眼镜到 Linux 网关：MQTT JSON 协议

### 1. MQTT Topic 设计

智能眼镜发布主题：

```text
glasses/gesture
```

Linux 网关订阅主题：

```text
glasses/gesture
```

后续系统状态反馈可以预留：

```text
hand/status
gateway/log
```

### 2. JSON 数据格式

智能眼镜上传的数据格式：

```json
{
  "device": "smart_glasses_01",
  "gesture": "NOD",
  "mode": "CONTROL",
  "cmd": "GRAB",
  "timestamp": 1718600000
}
```

### 3. 字段说明

| 字段 | 类型 | 示例 | 说明 |
|---|---|---|---|
| device | string | smart_glasses_01 | 设备编号 |
| gesture | string | NOD | 识别到的手势 |
| mode | string | CONTROL | 当前工作模式 |
| cmd | string | GRAB | 最终控制命令 |
| timestamp | int | 1718600000 | 时间戳，后续可选 |

### 4. gesture 取值

| gesture | 含义 |
|---|---|
| LEFT | 左倾 |
| RIGHT | 右倾 |
| NOD | 点头 |
| SHAKE | 摇头 |
| BUTTON_LONG | 长按按键 |

### 5. mode 取值

| mode | 含义 |
|---|---|
| NORMAL | 普通显示模式 |
| CONTROL | 控制灵动手模式 |
| SETTING | 设置模式 |

### 6. cmd 取值

| cmd | 含义 |
|---|---|
| OPEN | 灵动手张开 |
| GRAB | 灵动手抓取 |
| RELEASE | 灵动手释放 |
| STOP | 停止动作，进入安全状态 |
| NONE | 无控制命令 |

---

## 三、手势到控制命令映射

| 智能眼镜动作 | gesture | cmd | 灵动手动作 |
|---|---|---|---|
| 左倾 | LEFT | OPEN | 张开 |
| 点头 | NOD | GRAB | 抓取 |
| 右倾 | RIGHT | RELEASE | 释放 |
| 长按按键 | BUTTON_LONG | STOP | 停止 / 安全状态 |
| 摇头 | SHAKE | NONE | 暂不执行 |

设计说明：

- 智能眼镜只负责识别动作和生成命令
- Linux 网关负责二次校验和协议转换
- STM32 只执行明确的动作命令

---

## 四、Linux 网关到 STM32：UART 自定义帧协议

### 1. UART 帧格式

```text
0xAA 0x55 CMD LEN DATA CHECKSUM 0x0D 0x0A
```

### 2. 字段说明

| 字段 | 长度 | 说明 |
|---|---:|---|
| 0xAA 0x55 | 2 字节 | 帧头 |
| CMD | 1 字节 | 命令字 |
| LEN | 1 字节 | DATA 数据长度 |
| DATA | N 字节 | 命令数据 |
| CHECKSUM | 1 字节 | 校验和 |
| 0x0D 0x0A | 2 字节 | 帧尾 |

### 3. CMD 命令字定义

| 命令 | CMD 十六进制 | 说明 |
|---|---:|---|
| CMD_OPEN | 0x01 | 灵动手张开 |
| CMD_GRAB | 0x02 | 灵动手抓取 |
| CMD_RELEASE | 0x03 | 灵动手释放 |
| CMD_STOP | 0x04 | 停止动作 |
| CMD_HEARTBEAT | 0x05 | 心跳检测 |
| CMD_ERROR | 0xFF | 异常命令 |

### 4. 示例帧

#### OPEN 命令

```text
AA 55 01 00 01 0D 0A
```

含义：

```text
AA 55：帧头
01：CMD_OPEN
00：DATA 长度为 0
01：CHECKSUM
0D 0A：帧尾
```

#### GRAB 命令

```text
AA 55 02 00 02 0D 0A
```

#### RELEASE 命令

```text
AA 55 03 00 03 0D 0A
```

#### STOP 命令

```text
AA 55 04 00 04 0D 0A
```

---

## 五、CHECKSUM 计算方式

当前 MVP 使用简单校验和：

```text
CHECKSUM = CMD + LEN + DATA所有字节
```

只保留低 8 位：

```text
CHECKSUM = (CMD + LEN + DATA_SUM) & 0xFF
```

示例：

```text
CMD_OPEN = 0x01
LEN = 0x00
DATA 为空

CHECKSUM = 0x01 + 0x00 = 0x01
```

所以 OPEN 帧为：

```text
AA 55 01 00 01 0D 0A
```

---

## 六、Linux 网关处理流程

```text
1. 订阅 MQTT Topic: glasses/gesture
2. 接收智能眼镜 JSON 数据
3. 解析 gesture、mode、cmd 字段
4. 判断 mode 是否为 CONTROL
5. 判断 cmd 是否有效
6. 将 cmd 映射为 UART CMD
7. 计算 CHECKSUM
8. 发送 UART 自定义帧给 STM32
9. 写入系统日志
```

---

## 七、STM32 灵动手处理流程

```text
1. UART 接收数据
2. 判断帧头是否为 0xAA 0x55
3. 判断帧尾是否为 0x0D 0x0A
4. 读取 CMD 和 LEN
5. 计算 CHECKSUM 并校验
6. 根据 CMD 切换动作状态机
7. 执行 OPEN / GRAB / RELEASE / STOP
8. 异常命令进入 HAND_ERROR 状态
```

---

## 八、错误处理设计

| 错误情况 | 处理方式 |
|---|---|
| JSON 缺少 cmd 字段 | 丢弃并记录日志 |
| mode 不是 CONTROL | 不下发控制命令 |
| cmd 不在命令表中 | 发送 CMD_ERROR 或丢弃 |
| UART 帧头错误 | 丢弃该帧 |
| CHECKSUM 错误 | 丢弃该帧 |
| 超时未收到命令 | STM32 回到安全状态 |
| 舵机角度越界 | 限幅到安全范围 |

---

## 九、系统日志格式

Linux 网关日志格式：

```text
[2026-06-xx 20:30:12] device=smart_glasses_01 gesture=NOD mode=CONTROL cmd=GRAB uart=AA550200020D0A status=OK
```

异常日志示例：

```text
[2026-06-xx 20:31:01] device=smart_glasses_01 gesture=UNKNOWN mode=CONTROL cmd=INVALID status=DROP
```

---

## 十、设计优势

- MQTT JSON 适合智能眼镜和 Linux 网关之间的无线通信
- UART 自定义帧适合 Linux 网关和 STM32 之间的稳定控制
- JSON 易读、方便调试
- UART 帧短小、STM32 解析简单
- Linux 网关承担协议转换，降低 STM32 复杂度
- 后续可以扩展 ACK、心跳、错误码和动作参数