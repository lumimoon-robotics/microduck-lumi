# Microduck-Lumi · 飞特 1910 国产化复刻项目

> **🦆 一只用瑞芯微 RK3566 跑 ONNX 策略、用飞特 HD-1910-C001 驱动 15 关节的 25 cm 双足机器鸭复刻项目**
>
> **主线控制器**：瑞莎 **Radxa Zero 3W**（RK3566，4× Cortex-A55，0.8 TOPS NPU，与 Pollen 官方硬件同源）
> **执行器**：飞特 **HD-1910-C001** 总线舵机 × 15（凸舵盘，已适配官方 8 个连杆件）
> **两条电源路线**：
> ① **有线**——7.5 V/3 A 桌面电源适配器（开发调试期推荐）
> ② **无线**——2S 18650 锂电池组（整机集成期推荐）
> **最后更新**：2026-10-07（基于全网开源资料 + D-Robotics 论坛 + 飞特官方规格书 + Radxa 官方文档 多源校验）

---

[![Organization](https://img.shields.io/badge/org-lumimoon--robotics-blueviolet)](https://github.com/lumimoon-robotics)
[![Repository](https://img.shields.io/badge/repo-microduck--lumi-blue)](https://github.com/lumimoon-robotics/microduck-lumi)
[![License](https://img.shields.io/badge/license-Apache--2.0-green.svg)](LICENSE)
[![Servo](https://img.shields.io/badge/Servo-飞特%20HD--1910-orange)](https://www.feetech.cn/510257)
[![Mainboard](https://img.shields.io/badge/Mainboard-Radxa%20Zero%203W-red)](https://radxa.com/products/zero-3w/)
[![Stars](https://img.shields.io/github/stars/lumimoon-robotics/microduck-lumi?style=social)](https://github.com/lumimoon-robotics/microduck-lumi/stargazers)
[![Forks](https://img.shields.io/github/forks/lumimoon-robotics/microduck-lumi?style=social)](https://github.com/lumimoon-robotics/microduck-lumi/network/members)
[![Issues](https://img.shields.io/github/issues/lumimoon-robotics/microduck-lumi)](https://github.com/lumimoon-robotics/microduck-lumi/issues)
[![Last commit](https://img.shields.io/github/last-commit/lumimoon-robotics/microduck-lumi)](https://github.com/lumimoon-robotics/microduck-lumi/commits/main)

**所属组织**：[lumimoon-robotics](https://github.com/lumimoon-robotics)（启月探微 · Qiyue Robotics）· **同组织项目**：[qiyue-tanwei-website](https://github.com/lumimoon-robotics/qiyue-tanwei-website)

---

## 📚 目录

- [项目简介](#-项目简介)
- [快速开始](#-快速开始)
- [为什么选 Radxa Zero 3W + 飞特 HD-1910](#-为什么选-radxa-zero-3w--飞特-hd-1910)
- [硬件总览](#-硬件总览)
- [两条电源路线](#-两条电源路线有线桌面-/-无线整机)
- [物料清单 BOM](#-物料清单-bom含淘宝购买链接)
- [3D 打印与机械](#-3d-打印与机械)
- [舵机准备与配 ID](#-舵机准备与配-id)
- [上位机镜像烧录](#-上位机镜像烧录radxa-zero-3w)
- [舵机控制台与协议层](#-舵机控制台与协议层)
- [策略 ONNX 部署](#-策略-onnx-部署)
- [整机集成与上电流程](#-整机集成与上电流程)
- [安全与故障保护](#-安全与故障保护)
- [常见坑位与排查](#-常见坑位与排查)
- [参考资源（全网已校验）](#-参考资源全网已校验)
- [许可证与致谢](#-许可证与致谢)

---

## 🦆 项目简介

这是一份**面向全网复刻者**的完整教程，目标是让任何一个有 3D 打印机、有基础 Linux 操作经验、有耐心的人，都能从零搭起一只会走路的双足机器鸭。

**与 Pollen Robotics 官方 Microduck 的关系**：

| 维度 | 官方 Microduck | 本复刻 |
|---|---|---|
| 主控 | Radxa Zero 3W（RK3566） | **同款**（瑞芯微同系列，市售模块直跑） |
| 执行器 | Dynamixel XL330-M288-T × 15 | **飞特 HD-1910-C001 × 15**（国产、便宜约一半、力矩约为 2.5×） |
| 协议 | Dynamixel V2（1 Mbps 单线半双工） | 飞特 STS/SCS（1 Mbps 单线半双工）——**协议层完全改写** |
| 策略 | 官方 9 个 ONNX | **必须用 HD-1910 的 BAM 参数重训**（沿用 `microduck_rl`） |
| 头部 IMU | LSM6DSV16X 装在 HAT | **同款**（IMU 走 I²C） |
| 电池 | NP-F970（官方）/ NP-F550（复刻实测） | **2S 18650 锂电池**（更便宜、更普及） |
| 总线 | 1 Mbps 单线半双工 TTL | **同方案** |
| 控制环 | 50 Hz，61 维观测 → 14 维动作 | **同契约** |

**复刻核心理念**：**机械照抄 + 电控自建 + 训练沿用**。放弃 100% 复刻的执念，把不可得的部分用逆向和市售模块补齐。

---

## 🚀 快速开始

如果你只想跟着走，3 个文件就能跑起来：

1. **[BOM 清单（淘宝实链）](docs/采购/BOM-含淘宝链接.md)** → 直接拿去下单
2. **[打印清单](docs/装配/打印清单.md)** → 把 STL/STEP 丢进 Bambu Studio
3. **[整机集成上电流程](docs/装配/整机集成上电流程.md)** → 跟着步骤一步步来

详细章节见 `docs/` 目录。

---

## 🤔 为什么选 Radxa Zero 3W + 飞特 HD-1910

### 选 Radxa Zero 3W 的理由（已经联网校验）

1. **官方同款**：Pollen Robotics 的 Microduck 真机端就是 Radxa Zero 3W（RK3566，4× A55，Mali-G52，0.8 TOPS NPU），**我们直接用同一款模块**。
2. **官方文档明确电源要求**：Radxa 官方文档说 "ZERO 3W/3E 采用 USB-C 1 接口供电，**仅支持 5V 输入**，建议最低使用 5V/2A 电源适配器" —— 这就是为什么主控要独立一条 5 V 电源，**不能和舵机共用 7.5 V 适配器**（需要 DC-DC 降到 5 V，或者干脆主控和舵机各走一路）。
3. **生态成熟**：Armbian / Radxa OS 都有现成镜像，社区有 wlan0 DHCP、URT-2 上电顺序等已知坑位的解决方案（`fanhao375/microduck-replica` 仓库的 `踩坑记录.md` 写得很细）。

### 选飞特 HD-1910-C001 的理由（已经联网校验）

1. **力矩足够**：堵转扭矩官方规格书 **12 kg·cm**（淘宝页面 / 飞特产品页 / D-Robotics 论坛三方一致）；XL330 在 Microduck 里实际是**超压 37% 跑**（额定 3.7–6.0 V、实际 6.6–8.2 V），而 **HD-1910 是额定匹配**（4–8.4 V、实测 6.6–8.2 V 落在 5–8.4 V 区间内）。
2. **价格约为一半**：HD-1910 单只淘宝 118 元，XL330 美站 $412 / 欧站 €603，一只鸭子 15 颗就差好几千。
3. **协议开放**：飞特 STS/SCS 协议文档公开，社区有 Python / Rust / Arduino 全栈实现。
4. **凸舵盘问题已解决**：`fanhao375/microduck-replica` 仓库专门用 `-FT` 后缀重画了 8 个与凸舵盘配合的连杆件，直接拿 SolidWorks / STEP 改即可。
5. **BAM 动力学参数已就绪**：`LuwuDynamics/xgoduck_rl` 公开了 HD-1910 的 BAM M6 电机参数（`kt = 0.692 N·m/A`，`fanhao375` 仓库已实测 R²=0.998 验证），训练端直接用，**不用自辨识**。

> ⚠️ **型号警告**（来自 `fanhao375/microduck-replica` 踩坑记录）：`HL-2915-C002` 是 9–14 V、2S 电池带不动、且脚序相反，**勿参考**。**只认 HD-1910-C001**。

---

## 🔩 硬件总览

### 系统架构图

```
        7.5V/3A 桌面适配器
        或 2S 18650 电池组
              │
              ▼
        ┌─────────────┐
        │  电源管理   │  DC-DC 5V → 主控   DC 7.5V → 舵机
        └──────┬──────┘
               │
       ┌───────┴────────┐
       ▼                ▼
  ┌─────────┐      ┌──────────────┐
  │ Radxa   │ USB  │ 飞特 URT-2  │  飞特单线半双工总线（1 Mbps）
  │ Zero 3W ├─────►│ 舵机调试板  ├──────────────┐
  │ (RK3566)│      └──────────────┘              │
  │ + IMU   │                                  ▼
  │ + 摄像头 │                          15 颗 HD-1910
  └────┬────┘                          （左腿5+右腿5+头颈4+嘴1）
       │
       ▼
   ONNX Runtime @ 50 Hz
   （61维观测 → 14维动作）
```

### 核心器件

| 类别 | 选型 | 数量 | 备注 |
|---|---|---|---|
| **主控** | 瑞莎 Radxa Zero 3W（RK3566，2 GB RAM，**带排针版**） | 1 | 推荐 2G/16G，板载 WiFi 必选 |
| **执行器** | 飞特 HD-1910-C001（4–8.4 V，12 kg·cm 堵转，凸舵盘） | 15 + 3 备用 | 左腿 5 / 右腿 5 / 头颈 4 / 嘴 1 |
| **电源（有线路线）** | 7.5 V / 3 A 桌面电源适配器（DC 5.5×2.1 插头，3C 认证） | 1 | 调试期推荐；不接电池，方便安全 |
| **电源（无线路线）** | 2S 18650 锂电池组（7.4 V，3400 mAh，XH2.54-2P 公头，放电倍率 2C） | 1 | 整机集成时用；需自行加保险丝和动力开关 |
| **DC-DC 降压** | 5 V/3 A DC-DC 模块（从 7.5 V/2S 电池降到 5 V 给主控） | 1 | 主控和舵机**严禁共正极并接** |
| **舵机调试板** | 飞特 URT-2 串口总线舵机驱动板（Type-C 接口） | 1 | 配 ID、测单舵机用 |
| **IMU** | LSM6DSV16X 模块（I²C 接口，3.3 V） | 1 | 装头颈俯仰轴附近 |
| **摄像头** | IMX219 模组（MIPI FPC 排线，500 万像素） | 1 | 装胸前；本教程可选（基础步行不需要） |
| **轴承** | 6700K（10×15×3）+ ET2216（16×22×4） | 3 + 1 | 与原版同型号 |
| **SD 卡** | MicroSD / TF 64 GB A2 | 1 | 烧 Armbian / Radxa OS |
| **结构件** | PLA / PETG / TPU 95A 3D 打印 | 1 套 | 承力件务必 PETG，脚垫 TPU |
| **紧固件** | M2 自攻螺丝（约 325 件） | 1 包 | 实际装配以图纸为准 |

### 关节拓扑（与官方一致）

| 组 | 关节 | 数量 | 备注 |
|---|---|---|---|
| 左腿 | hip_yaw / hip_roll / hip_pitch / knee / ankle | 5 | 步态主驱 |
| 右腿 | hip_yaw / hip_roll / hip_pitch / knee / ankle | 5 | 步态主驱 |
| 头颈 | head_yaw / head_roll / head_pitch / neck_pitch | 4 | 表达 |
| 嘴部 | mouth | 1 | 独立控制，**不参与步态**（故策略输出 14 维） |

**总计 15 颗舵机 / 14 个受控关节**。

---

## ⚡ 两条电源路线：有线桌面 / 无线整机

### 路线 A：有线（7.5 V / 3 A 桌面电源）—— **开发期推荐**

**适用阶段**：舵机配 ID、协议层调试、策略上机调试、台架试走。

**接线**：

```
7.5V/3A 适配器 ──► DC 5.5×2.1 ──► 飞特 URT-2 调试板 ──► 15 颗 HD-1910（菊花链）
                                            │
                                            └─► 单独 USB 取电给 Radxa Zero 3W（5 V/3 A PD 适配器）
```

**优点**：
- 不接电池，没有爆炸 / 短路风险；
- 适配器限流 3 A，舵机堵转自动降压，不会烧舵机；
- 长时间调试不用顾虑电量。

**缺点**：
- 拖着电源线，不能整机走动；
- 走策略时电源线会绊脚。

### 路线 B：无线（2S 18650 锂电池）—— **整机集成期推荐**

**适用阶段**：机械结构全装好、策略已验过、准备整机展示 / 视频拍摄。

**接线**：

```
2S 18650 电池组（7.4 V）──► 保险丝（5 A）──► 动力开关 ──┬─► 飞特舵机总线（V+/GND/DATA）
                                                        │
                                                        └─► DC-DC 5V/3A 降压模块 ──► Radxa Zero 3W（Type-C）
```

**严格禁止**：
- ❌ **两路电源正极并接**（电池正极与适配器正极接一起，会短路烧板）；
- ❌ **舵机大电流走 URT-2 的 5 A 通路**（URT-2 不扛 15 颗堵转）；
- ❌ **软件卸力代替动力开关**（关 ARM 之后舵机仍带电，碰一下会甩飞）。

**最低工作电压**：`fanhao375/microduck-replica` 实测下限 6.0 V；空电关机阈值 6.6 V（`robotd` 触发，参考 `sim336/OptiDuck` v1.02 修正）。

**电池容量选型**：
- 7.4 V / 3400 mAh 2S 锂电池组：站立 + 慢走约 30 min；激烈动作约 15 min。
- 如需更长续航，可升 7.4 V / 5200 mAh 2S（淘宝 50–60 元）。

### 两种路线的切换

整个项目里**只换电源**，不换线。机械结构件完全相同；电源管理模块（DC-DC、保险丝、动力开关）单独做成可拔插的"电池盒"子组件。

---

## 🛒 物料清单 BOM（含淘宝购买链接）

详见 **[docs/采购/BOM-含淘宝链接.md](docs/采购/BOM-含淘宝链接.md)**。

要点：
- 凡是本教程作者淘宝店铺有售的元件，**直接挂本店 SKU 链接**（已剥掉无关 query 参数，保留能定位到具体 SKU 的最小链接）；
- 旗舰店直营 / 立创商城 / Radxa 官方店 的链接保留原样；
- 电池、保险丝、动力开关、3D 打印件耗材等**通用件**，用户自行到熟悉的渠道下单。

---

## 🖨️ 3D 打印与机械

详见 **[docs/装配/打印清单.md](docs/装配/打印清单.md)**。

### 关键提示

1. **STL 来源**：**不要从本仓库拿**，请去官方仓库 `pollen-robotics/microduck` 拉 `microduck_description/` 下的 47 个原始 STL（或用 `fanhao375/microduck-replica-cad` 仓库的 SolidWorks + STEP 二次开发）。
2. **-FT 后缀件**：把原版 STL 中**与凹舵盘配合的 8 个连杆件**换成 `-FT` 后缀的版本（适配 HD-1910 凸舵盘）。`fanhao375` 仓库的 `cad/` 已开源。
3. **承力件用 PETG**：脚踝、髋关节、肩部连杆务必 PETG 0.20 mm · 5 壁 · 5 层顶底 · 40% gyroid。
4. **脚垫用 TPU 95A**：单独打印，能显著提升站立稳定性。
5. **孔特征反推**：`fanhao375` 已从 47 个 STL 孔特征反推 M2 紧固件清单，本仓库**沿用**。

### 装配顺序

```
源装配 → 左腿五轴链 → 躯干背包 → 右腿（镜像）→ 头颈/嘴部 → 罩壳
```

每步装完**通电测单舵机**（URT-2 + 上位机 `servo-web` 网页调试台），确认零位和正方向再装下一步。

---

## 🎛 舵机准备与配 ID

详见 **[docs/调试/舵机配ID与方向校正.md](docs/调试/舵机配ID与方向校正.md)**。

### 关键提示

1. **出厂 ID 全部是 1**——**一次只能接 1 颗**到 URT-2 配 ID，否则会总线冲突。
2. **ID 分配约定**（与官方 `microduck_rl` 训练脚本一致）：
   - 左腿：21–25
   - 右腿：11–15
   - 头颈：31–34
   - 嘴：1
   - IMU 板（如果做 IMU 转发板）：200
3. **飞特脚序**：`1=Signal, 2=Vcc, 3=GND`（与 XL330 的 `1=GND, 2=Vcc, 3=Signal` **完全相反**）—— 配线时务必**逐根核对**，接反必烧。
4. **零位与方向**：每颗舵机装配完成后**先不通电用手转**到中位，再通电发指令写零位。**任何时候堵转测试都要把电压限制在 5 V 以下**（8.4 V 满电 + 堵转 = 舵机壳体烧穿风险）。

---

## 💾 上位机镜像烧录（Radxa Zero 3W）

详见 **[docs/调试/上位机镜像烧录.md](docs/调试/上位机镜像烧录.md)**。

### 快速流程

1. 下载 `Armbian_25.x_radxa-zero3_bookworm_xxx.img.xz`（或 Radxa 官方 Radxa OS）；
2. 用 Raspberry Pi Imager / balenaEtcher 烧到 64 GB SD 卡；
3. 第一次启动默认 `root/1234`，按提示改密；
4. `apt update && apt upgrade -y`；
5. 安装 ONNX Runtime 1.28+：`pip install onnxruntime`（CPU 版本足够）；
6. 克隆本仓库 `git clone https://github.com/lumimoon-robotics/microduck-lumi.git`。

> ⚠️ **已知坑**（来自 `fanhao375/microduck-replica`）：Radxa Zero 3W 默认 Armbian 镜像的 wlan0 DHCP 偶发抽风，建议**先插网线**，WiFi 留到整机阶段再开。

---

## 🐍 舵机控制台与协议层

详见 **[docs/调试/舵机控制台与协议层.md](docs/调试/舵机控制台与协议层.md)**。

### 飞特 STS 协议（HD-1910 起点，实测核对）

```
指令帧: 0xFF 0xFF | ID | 长度 | 指令 | 参数... | 校验和
校验和: CheckSum = ~(ID + Length + Instruction + Params) & 0xFF
ID: 0~253，254(0xFE) = 广播
指令: PING=0x01 / READ=0x02 / WRITE=0x03 / SYNC_WRITE=0x83 / SYNC_READ=0x82 ...
```

> ⚠️ HD-1910 的**专属内存控制表**（寄存器地址）、默认波特率、逻辑电平**需以飞特官方《内存控制表》《通讯协议》文档为准**，不要硬编码 XL330 的 HLS/STS。

### 推荐实现路径

- **Python 调试期**：`fanhao375` 仓库的 `tools/servo-web/feetech.py`（一百多行，不依赖飞特 SDK，浏览器拖滑块控制 + 3D 实时同步）；
- **Rust 板载期**：直接用 `rustypot` 仓库（`pollen-robotics` 上游）的飞特适配 + 官方 `FeetechIo`（`fanhao375` 已适配 v1，跟上官方 0.14.1）；
- **自检工具**：`sim336/OptiDuck` 仓库的 `tools/ft_regs.py`（寄存器直读 / 直写）、`bench_mirror.py`（台架只读实时镜像）。

---

## 🧠 策略 ONNX 部署

详见 **[docs/调试/策略部署.md](docs/调试/策略部署.md)**。

### 策略契约（与官方一致）

- **61 维观测 → 14 维动作**（嘴部独立，**不参与步态**）；
- 控制环 **50 Hz**（20 ms 一拍）；
- 板上推理实测：Radxa Zero 3W 跑 ONNX Runtime p50 = 0.367 ms（占 50 Hz 周期 ≈ 1.8%，p99 = 0.836 ms，**over_20ms = 0**，来自 `sim336/OptiDuck` v1.03 实测）。

### 训练（如果你想自己重训）

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
git clone https://github.com/pollen-robotics/microduck_rl.git
cd microduck_rl
uv sync
uv run wandb login
# 用 HD-1910 的 BAM 参数（来自 LuwuDynamics/xgoduck_rl）替换默认电机参数
# 跑 PPO 训练，导出 ONNX
```

**Sim-to-real 四层保护**（官方做法）：

1. BAM 电机模型（用 HD-1910 实际参数）；
2. Domain randomization（质量、阻尼、随机扰动）；
3. Backlash 变体（齿轮间隙）；
4. 真机前用 `infer_policy.py` 契约重演。

> ⚠️ **不能用官方原版 ONNX**：Dynamixel XL330 与 HD-1910 的力矩曲线、齿隙、PWM 斜率都不同，**直接插即用必摔**。

---

## 🔌 整机集成与上电流程

详见 **[docs/装配/整机集成上电流程.md](docs/装配/整机集成上电流程.md)**。

### 5 步逐级上电（**绝对不要跳级**）

1. **单舵机夹具辨识**（延迟 / 死区 / 回差 / 负载响应），用 URT-2 + 桌面电源；
2. **离线回放**（不接任何硬件，跑 `infer_policy.py` 看 ONNX 输出范围）；
3. **悬吊**（用绳子把鸭子吊起来，让策略在真机上跑，看舵机是否按预期动）；
4. **单腿**（拆掉一条腿，单独测另一条腿的步态策略）；
5. **受保护站立**（系绳 / 软垫 / 保险杠 + 人在旁边，准备接住）；
6. **系绳慢走**（腰带 / 头颈套绳，能接住）。

每步不通过**回退上一级**。

---

## 🛡 安全与故障保护

详见 **[docs/调试/安全与故障保护.md](docs/调试/安全与故障保护.md)**。

### 硬性安全规则

- 连续 100 ms 无新有效目标帧 → 锁存故障、丢弃积压、尝试关闭力矩；
- 坏包 / 重复包 / 心跳**不续命**；
- 上电、重连**不自动运动**，恢复需人工复位并重新 ARM；
- 软件卸力**不能代替**动力开关；
- 任何时候**堵转测试都要把电压限制在 5 V 以下**（8.4 V 满电 + 堵转 = 舵机壳体烧穿）。

### 桌面调试期额外建议

- 装一根**拉绳 / 急停开关**串在动力线上；
- 第一次悬吊测试**至少两人**（一人操作、一人接）；
- 周围不要有易碎品 / 笔记本 / 宠物；
- 准备好灭火毯 / 沙包（2S 锂电短路会喷火）。

---

## 🪤 常见坑位与排查

详见 **[docs/调试/常见坑位.md](docs/调试/常见坑位.md)**。摘录高频：

| 现象 | 根因 | 怎么办 |
|---|---|---|
| 总线扫描不到任何舵机 | 飞特脚序接反 / 数据线太长未加屏蔽 | 逐根核对线序，缩短到 < 30 cm |
| 总线 ID 冲突 | 出厂全部是 1，一次接多颗 | 一次只接 1 颗，配完 ID 再串联 |
| 舵机 8.4 V 满电堵转后壳体发热 | 超规格 | 改 5 V 限压测试；长时堵转不要超过 10 s |
| ONNX 推理直接摔 | 没换电机参数 | 必须用 HD-1910 的 BAM 参数重训 |
| 整机走两步就倒立翻 | 凸舵盘连杆件没换 | 8 个 `-FT` 后缀件务必全换 |
| `robotd` 报 "torque off" 但实际舵机有扭矩 | 误读 reg 40 | `sim336/OptiDuck` v1.03 修了，**用新固件** |
| wlan0 DHCP 抽风 | Radxa Zero 3W 已知 bug | 先用网线，WiFi 留到整机阶段再开 |
| 摄像头 `/dev/video0` 被 `mediad` 独占 | 视频服务与自建 MJPEG 冲突 | `sim336/OptiDuck` v1.01 用 `report_video.py` 走 GStreamer |

---

## 🌐 参考资源（全网已校验）

### 一手官方

- **Pollen Robotics 真机端**：[pollen-robotics/microduck](https://github.com/pollen-robotics/microduck) —— Apache-2.0，1,235 commits，2026-10-06 最后更新；15 颗舵机 / 50 Hz / 61→14 维契约
- **Pollen Robotics 训练端**：[pollen-robotics/microduck_rl](https://github.com/pollen-robotics/microduck_rl)
- **Pollen Robotics HAT**：[pollen-robotics/elec_RPI_Robot_HAT](https://github.com/pollen-robotics/elec_RPI_Robot_HAT) —— Apache-2.0，KiCad 9 + Gerber + BOM + STEP
- **官方产品页**：[pollen-robotics.com/microduck](https://pollen-robotics.com/microduck)

### 国产化复刻仓库（按"HD-1910 路线对口度"排序）

| 仓库 | 重点 | 与本项目关系 |
|---|---|---|
| [fanhao375/microduck-replica](https://github.com/fanhao375/microduck-replica) | 机械反推 + HAT 自建 + HD-1910 平替实测 | **主要参考**（凸舵盘 -FT 件、BAM 参数、踩坑记录） |
| [fanhao375/microduck-replica-cad](https://github.com/fanhao375/microduck-replica-cad) | SolidWorks + STEP + 装配 BOM | 机械源 |
| [JoyandAI/OpenMicroDuck](https://github.com/JoyandAI/OpenMicroDuck) | 桌面调试 + 整机集成两阶段 BOM | **BOM 参考**（本仓库 7.5V/3A 适配器、2S 电池选型沿用） |
| [JoyandAI/microduck_rl](https://github.com/JoyandAI/microduck_rl) | HD-1910 BAM 参数 fork | 训练端用 |
| [sim336/OptiDuck](https://github.com/sim336/OptiDuck) | 整机装配完成 + 策略首次上机 + 板端服务全套 | 部署参考（HD-1910 供电基准修正、IMU 固件、`robotd` 调试） |
| [AI-FanGe/Microduck-build-tutorial](https://github.com/AI-FanGe/Microduck-build-tutorial) | 保姆级教程 + 预构建镜像 + STL/STEP | 流程参考（**用 XL330，非飞特**） |
| [LuwuDynamics/xgoduck_rl](https://github.com/LuwuDynamics/xgoduck_rl) | 公开 HD-1910 BAM M6 动力学参数 | 训练端电机参数 |
| [SaberOnGo/open-microduck](https://github.com/SaberOnGo/open-microduck) | 官方规格表中文版 | 文档参考 |
| [unergybot/microduck-ubot](https://github.com/unergybot/microduck-ubot) | 关节配置详解（14 自由度） | 拓扑参考 |
| [Datawhale/every-embodied](https://github.com/datawhalechina/every-embodied) | 强化学习教程《Open Duck Mini 与 Microduck》 | 训练教程 |
| [YDxun/microduck-duck-play](https://github.com/YDxun/microduck-duck-play) | 云端仿真 + 追球 / 认人闭环 | 视觉玩法 |
| [emwstudio/DuckEMW](https://github.com/emwstudio/DuckEMW) | 跳舞训练权重 | 玩法参考 |
| [rustypot](https://github.com/pollen-robotics/rustypot) | 上游舵机协议库（已支持飞特 v1） | Rust 板端协议层 |
| [pollen-robotics/elec_RPI_Robot_HAT](https://github.com/pollen-robotics/elec_RPI_Robot_HAT) | HAT 板 KiCad 9 工程 | 板端硬件 |

### 视频教程

- [手搓 microduck 保姆级完整教程（AI-FanGe）](https://www.bilibili.com/video/BV1uUbG6FEfb/)
- [MicroDuck 复刻 -- OpenMicroDuck 第一话](https://www.bilibili.com/video/BV1TbtZ62Eq8/) — **飞特方案**
- [Microduck 硬件架构拆解](https://www.snm0516.aisee.tv/video/BV1R2tH6tEsK/)
- [国产方案调通啦 — MicroDuck 复刻·第三话](https://www.bilibili.com/video/BV1GdYC68Enz/) — **飞特 HD-1910 已通**
- [Microduck 平替复刻进度 — 起身强化学习](https://www.bilibili.com/video/BV1Pcbs6DEoG/)
- [刚开源的 Microduck，被我用一张 4090D 教会跳舞](https://www.bilibili.com/video/BV18ytV6ZEEc/)

### 论坛 / 文章

- **D-Robotics 论坛**：[Duck Together Tutorial — 一键启动 MicroDuck 云仿真器并接入 RDK X5](https://forum-en.d-robotics.cc/t/duck-together-tutorial-launch-a-microduck-cloud-simulator-in-one-click-and-connect-your-rdk-x5/518)
- **D-Robotics 论坛**：[实体机器鸭：X5 + 底板控制方案](https://forum.d-robotics.cc/t/topic/35686)
- **飞特官方规格书**：[HD-1910-C001 4.8V 9kg / 6V 12kg TTL 舵机](https://www.feetech.cn/510257)
- **飞特论坛 PDF**：[HD-1910-C001 规格书 (D-Robotics 论坛镜像)](https://forum.d-robotics.cc/uploads/short-url/8ZVU1JhKRT57YPwTNXTXn5oizl0.pdf)
- **Radxa 官方文档**：[ZERO 3W 电源准备](https://docs.radxa.com/zero/zero3/other-os/android/preparation)
- **CSDN / 掘金 教程**：[Microduck-HD1910 手把手开发教程](https://blog.csdn.net/qq_41584795/article/details/165632956) / [掘金版](https://juejin.cn/post/7686341837754744867)
- **什么值得买**：[舵机比整机还贵：MicroDuck 复刻热潮，这笔账到底该怎么算](https://post.smzdm.com/p/a70nxnqg/)
- **立创开源**：[Radxa Zero 3W 供电扩展板](https://oshwhub.com/nnnno/open-radxa-zero-3w-gong-dian-wang-ka-kuo-zhan-ban)
- **知乎**：[Microduck 产品与技术报告](https://zhuanlan.zhihu.com/p/2078210646819316651)
- **今日头条**：[Microduck 的硬件配置与运动控制架构](https://www.toutiao.com/article/7683593654031712814/)
- **CSDN**：[Microduck 全栈拆解](https://blog.csdn.net/alspd_zhangpan/article/details/164297887)

---

## 📄 许可证与致谢

### 许可证

- 本仓库代码 / 文档：[Apache-2.0](LICENSE)
- 机械文件、3D 打印件：沿用上游 Microduck 3D 模型的 **CC BY-NC-SA 4.0**（如要商用需先联系 Pollen Robotics）
- 训练策略 / ONNX：沿用上游 Apache-2.0

### 致谢

- **Pollen Robotics** —— 开创了这只小鸭子，让全球开发者看到双足机器人可以如此可爱而触手可及
- **fanhao375** —— 凸舵盘 -FT 适配件、踩坑记录、IMU 板逆向协议
- **JoyandAI / OpenMicroDuck** —— 桌面调试 + 整机集成两阶段 BOM 思路
- **sim336 / OptiDuck** —— 板端服务、IMU 固件、HD-1910 供电基准修正
- **AI-FanGe** —— 保姆级教程的流程范式
- **LuwuDynamics** —— 公开的 HD-1910 BAM 动力学参数
- **D-Robotics 论坛** —— RDK X5 路线的云仿真教程（虽然我们用的是 Radxa Zero 3W，但视觉 / BPU 玩法可参考）
- **飞特 Feetech** —— HD-1910-C001 舵机、URT-2 调试板、官方规格书
- **瑞莎 Radxa** —— Zero 3W 同款主控 + 文档支持

本项目**与 Pollen Robotics / Hugging Face 无隶属关系，未获其背书**。

---

_最后更新：2026-10-07 · 基于全网 7 个 GitHub 仓库 + 6 个 B 站视频 + 3 个论坛 + 飞特官方规格书 + Radxa 官方文档 联网校验_
