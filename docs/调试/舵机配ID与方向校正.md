# 舵机配 ID 与方向校正

> **来源**：`fanhao375/microduck-replica` 仓库的 `docs/飞特适配架构.md` / `踩坑记录.md` 双源校验。
> **关键参考**：`sim336/OptiDuck` 仓库的 `tools/ft_regs.py`（寄存器直读直写）。

---

## 一、HD-1910-C001 关键参数（已联网校验）

| 项 | 参数 | 来源 |
|---|---|---|
| 型号 | **HD-1910-C001**（只认这个） | 飞特官方 + `fanhao375` 警告 |
| 电压 | 4–8.4 V | 飞特官方 |
| 堵转扭矩 | 4.8V → 9 kg·cm / **6V → 12 kg·cm** | 飞特产品页 + 规格书 PDF |
| 额定负载 | 3.0 kg·cm（= 0.294 N·m） | `fanhao375` 实测 |
| 齿比 | 320:1 | `JoyandAI/OpenMicroDuck` 仓库 |
| 通信 | TTL 单线半双工（1 Mbps 默认） | 飞特协议 |
| 接口 | AMP2.0-3P（**注意是 2.0 mm 线距**） | 飞特产品页 |
| 出厂 ID | **全部是 1** | `fanhao375` 实测 |
| 协议 | 飞特 STS / SCS（与 XL330 不兼容） | 飞特规格书 |
| 脚序 | **1=Signal 2=Vcc 3=GND**（与 XL330 完全相反） | `fanhao375` 实测 |

> ⚠️ **型号警告**：`HL-2915-C002` 是 9–14 V，2S 电池带不动，且脚序相反，**勿参考**。`STS3215` 是不同型号，力矩不够，**也勿参考**。

---

## 二、配 ID 工具

### 2.1 飞特官方 URT-2 调试板（推荐）

- **接口**：Type-C
- **配套软件**：Feetech Debug Center（Windows / macOS / Linux）
- **配 ID 流程**（按手册）：
  1. URT-2 插入 Type-C 到电脑；
  2. 7.5V/3A 适配器插到 URT-2 电源口；
  3. 打开 Feetech Debug Center → 选 STS 协议 → 选 URT-2 串口；
  4. Scan → 应该能看到 1 颗舵机（出厂 ID 1）；
  5. 选 ID 1 → 写入新 ID（如 11）→ 确认；
  6. 断电 → 重 Scan → 应该看到 ID 11 出现。

### 2.2 自己用 URT-1（更便宜）

- 用 **USB 转 TTL 模块**（CH340 / CP2102）+ 飞特协议代码（Python 库 `feetech-servo`）；
- 优点：便宜（CH340 5 元）；
- 缺点：只能 ping / 写 ID，不能图形化调舵机；
- **推荐**：用官方 URT-2（80 元）省事。

### 2.3 `sim336/OptiDuck` 仓库的 `tools/ft_regs.py`

- 高级玩家用：寄存器直读直写，可批量配 ID、读温度、读位置；
- 代码片段（来自 sim336/OptiDuck 仓库）：
  ```python
  import serial
  from ft_regs import FeetechServo

  ser = serial.Serial('/dev/ttyUSB0', 1000000, timeout=0.1)
  servo = FeetechServo(ser, id=1)

  # 改 ID
  servo.write_byte(5, new_id)  # 寄存器 5 = ID

  # 读位置
  pos = servo.read_word(56)  # 寄存器 56 = Present Position
  print(f"Position: {pos}")

  # 读温度
  temp = servo.read_byte(63)  # 寄存器 63 = Present Temperature
  print(f"Temperature: {temp}°C")
  ```

---

## 三、ID 分配约定（与 `microduck_rl` 训练脚本一致）

| 组 | ID 范围 | 数量 | 备注 |
|---|---|---|---|
| **左腿** | **21–25** | 5 | hip_yaw=21, hip_roll=22, hip_pitch=23, knee=24, ankle=25 |
| **右腿** | **11–15** | 5 | hip_yaw=11, hip_roll=12, hip_pitch=13, knee=14, ankle=15 |
| **头颈** | **31–34** | 4 | head_yaw=31, head_roll=32, head_pitch=33, neck_pitch=34 |
| **嘴部** | **1** | 1 | mouth |
| IMU 板（如果做 IMU 转发板） | 200 | 1 | 与舵机总线统一 |

> 💡 **为什么 IMU 板要 200**：`fanhao375` 仓库把 IMU 做成"伪装成 Dynamixel 从机"，**所有从机（包括 IMU）走同一条总线**，主机跑 `sync_read` 一次性读完姿态 + 15 颗舵机状态，不用第二条总线。

---

## 四、配 ID 步骤（15 颗）

### 4.1 准备工作

- 15 颗 HD-1910（**全部从包装里拆出来**）；
- 1 个 URT-2 调试板；
- 1 个 7.5V/3A 适配器；
- 1 根 AMP2.0-3P 转 3 根杜邦线（**或 PH2.0-3P 转接线**）；
- 标签贴纸（**强烈建议**，配完一颗标一颗）。

### 4.2 逐颗配 ID（每次只接 1 颗）

> ⚠️ **关键警告**：出厂所有 ID 都是 1，**一次只能接 1 颗**，否则总线冲突，会出现"scan 到 1 颗" / "scan 不到任何一颗"交替出现的诡异现象。

```
步骤：
1. URT-2 上电（7.5V 适配器）
2. URT-2 插入电脑（USB-C）
3. 打开 Feetech Debug Center
4. 取 1 颗新舵机，插到 URT-2
5. Scan → 看到 ID 1
6. 写入新 ID（如 11，**先存到贴纸上**）
7. 断电 → 拔下舵机
8. 贴标签 "ID 11, 顺序 #1"
9. 重复步骤 4-8
```

> 💡 **小技巧**：把 15 颗舵机配 ID 后**按 ID 顺序摆成一排**（11, 12, 13, 14, 15, 21, 22, 23, 24, 25, 31, 32, 33, 34, 1），后面装配时直接拿对应 ID。

### 4.3 验证 15 颗 ID 全部唯一

- 15 颗全配完后，**15 颗同时接上 URT-2 菊花链**（用 5264-3P 转接线串接）；
- Scan → 应该看到 15 个 ID（11-15, 21-25, 31-34, 1）；
- 如果有重复，**回到 4.2 重新配那 2 颗**。

---

## 五、零位与方向校正

> **核心原则**：每颗舵机装配完成后**先不通电用手转**到中位，再通电发指令写零位。

### 5.1 准备

- 用 URT-2 + `servo-web` 网页调试台（`fanhao375` 仓库的 `tools/servo-web/`）；
- `servo-web` 提供：
  - 浏览器拖滑块控制单舵机；
  - 3D 模型实时同步；
  - 重心投影显示；
  - **不依赖飞特 SDK**（自实现飞特协议 `feetech.py`）。

### 5.2 单舵机零位校正

```
步骤（对每颗舵机都做）：
1. 舵机未通电时，用手转到中位（2048 对应的角度）；
2. 装到机械结构上（**先不拧紧螺丝**）；
3. 通电（URT-2 上电）；
4. 用 servo-web 发指令：目标位置 2048；
5. 看舵盘是否在中位（**如果偏了，用手轻拨到中位**）；
6. 拧紧螺丝；
7. 用 servo-web 测 ±30°：看转向是否正确
   - 左腿正方向：从身体中线看，向前摆动 = +X
   - 右腿正方向：从身体中线看，向前摆动 = +X（**镜像**）
   - 头颈正方向：抬头 = +pitch
   - 嘴部正方向：张嘴 = +opening
8. 填入 calibration.json
```

### 5.3 calibration.json 结构

参考 `microduck-lumi` 项目的 `config/calibration.json`（在 `docs/调试/舵机配ID与方向校正.md` 中给出）：

```json
{
  "servos": [
    {
      "id": 11,
      "name": "right_ankle",
      "group": "right_leg",
      "direction": 1,
      "home": 2048,
      "min": 700,
      "max": 3300,
      "deadband": 2,
      "backlash": 0.002
    }
  ],
  "imu": {
    "mount_quat": [0, 0, 0, 1],
    "axis": "xyz"
  }
}
```

> ⚠️ **不能直接套用 XL330 参数作为 HD-1910 默认值**：两者量程、分辨率、死区、PWM 斜率都不同。

---

## 六、批量方向测试

### 6.1 `sim336/OptiDuck` 的 `bench_mirror.py`

- **功能**：台架只读实时镜像（不发送指令，只读 15 颗舵机当前状态）；
- **用途**：装机后做"健康检查"——15 颗舵机状态全绿 = 通过。

### 6.2 `sim336/OptiDuck` 的 `servo_config_gui.py`

- **功能**：舵机逐个配置（图形界面，Python tkinter）；
- **用途**：**比 URT-2 自带软件更直观**——可以看到 15 颗舵机的当前位置、温度、电压、负载；
- **仓库**：`sim336/OptiDuck/tools/servo_config_gui.py`

---

## 七、飞特协议层自检

### 7.1 通信帧格式

```
指令帧: 0xFF 0xFF | ID | 长度 | 指令 | 参数... | 校验和
校验和: CheckSum = ~(ID + Length + Instruction + Params) & 0xFF
ID: 0~253，254(0xFE) = 广播
指令: PING=0x01 / READ=0x02 / WRITE=0x03 / SYNC_WRITE=0x83 / SYNC_READ=0x82 ...
```

### 7.2 HD-1910 关键寄存器（实测核对）

| 地址 | 名称 | 字节 | 备注 |
|---|---|---|---|
| 5 | ID | 1 | 0–253，254 = 广播 |
| 6 | Baud Rate | 1 | 7 = 1 Mbps（默认） |
| 8 | Return Delay Time | 1 | 默认 0（比 XL330 少一次读写） |
| 40 | Torque Enable | 1 | 0 = 卸力，1 = 上力 |
| 42 | Goal Position | 2 | 0–4095（中位 2048） |
| 56 | Present Position | 2 | 同上 |
| 57 | Present Speed | 2 | |
| 58 | Present Load | 2 | |
| 60 | Present Voltage | 1 | 0.1 V 单位 |
| 63 | Present Temperature | 1 | °C |

> ⚠️ HD-1910 的**专属内存控制表**（寄存器地址）、默认波特率、逻辑电平**需以飞特官方《内存控制表》《通讯协议》文档为准**，不要硬编码 XL330 的 HLS/STS 寄存器。

### 7.3 SYNC_READ 批量读

- 飞特 STS 协议支持 **SYNC_READ (0x82)** 一次性读多颗舵机的同一寄存器；
- 用法与 Dynamixel 一致；
- `fanhao375` 仓库的 `feetech.py` 已实现：
  ```python
  positions = feetech.sync_read_position([11, 12, 13, 14, 15], start_addr=56, length=2)
  ```

---

## 八、易错清单

| ❌ 错 | ✅ 对 | 原因 |
|---|---|---|
| 一次配 ID 接多颗 | **一次只接 1 颗** | 出厂 ID 全部是 1，总线冲突 |
| 用 XL330 的脚序 | **用飞特脚序 1=Signal 2=Vcc 3=GND** | 完全相反 |
| 8.4V 满电直接堵转 | **5V 限压测试** | 8.4V 满电 + 堵转 = 壳体烧穿 |
| 装好后再校零位 | **先手转中位再装** | 装好后再校要拆 |
| 直接套用 XL330 的 calibration | **每颗舵机单独校** | 两者参数差异大 |
| 装紧螺丝后再通电测 | **先通电测再装紧** | 装紧后舵盘转不动会烧 |

---

_参考来源：
1. 飞特官方产品页 https://www.feetech.cn/510257
2. 飞特论坛规格书 PDF https://forum.d-robotics.cc/uploads/short-url/8ZVU1JhKRT57YPwTNXTXn5oizl0.pdf
3. fanhao375/microduck-replica 仓库 https://github.com/fanhao375/microduck-replica
4. sim336/OptiDuck 仓库 https://github.com/sim336/OptiDuck
5. JoyandAI/OpenMicroDuck 仓库 https://github.com/JoyandAI/OpenMicroDuck
6. microduck-lumi 项目（自身）https://github.com/lumimoon-robotics/microduck-lumi_
