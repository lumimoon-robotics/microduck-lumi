# NOTICE

本项目（Microduck 国芯复刻教程 · Radxa Zero 3W + 飞特 HD-1910）整合了以下开源项目的成果，并保留了各自的许可证与归属说明。

---

## 一手官方

- **Pollen Robotics microduck**（真机端）—— <https://github.com/pollen-robotics/microduck>
  - License: Apache-2.0
  - Copyright: Pollen Robotics
  - 引用：61→14 维策略契约、50 Hz 控制环、RK3566 主控、15 舵机拓扑
- **Pollen Robotics microduck_rl**（训练端）—— <https://github.com/pollen-robotics/microduck_rl>
  - License: Apache-2.0
  - 引用：MuJoCo Warp + PPO + BAM 训练管线
- **Pollen Robotics HAT** —— <https://github.com/pollen-robotics/elec_RPI_Robot_HAT>
  - License: Apache-2.0

## 国产化复刻仓库

- **fanhao375 / microduck-replica** —— <https://github.com/fanhao375/microduck-replica>
  - License: Apache-2.0
  - Copyright: fanhao375
  - 引用：HD-1910 凸舵盘 -FT 适配、踩坑记录、IMU 板逆向、自实现 feetech.py 协议栈
- **fanhao375 / microduck-replica-cad** —— <https://github.com/fanhao375/microduck-replica-cad>
  - License: Apache-2.0
  - 引用：SolidWorks + STEP CAD 工程
- **JoyandAI / OpenMicroDuck** —— <https://github.com/JoyandAI/OpenMicroDuck>
  - License: Apache-2.0（软件）/ CC BY-NC-SA 4.0（硬件）
  - 引用：桌面调试 + 整机集成两阶段 BOM 思路、URT-2 调试板选型
- **JoyandAI / microduck_rl** —— <https://github.com/JoyandAI/microduck_rl>
  - License: Apache-2.0
  - 引用：HD-1910 BAM 参数 fork
- **sim336 / OptiDuck** —— <https://github.com/sim336/OptiDuck>
  - License: Apache-2.0
  - 引用：HD-1910 供电基准修正（5V → 2S 电池）、IMU 固件、ft_regs.py 工具集、整机装配流程
- **AI-FanGe / Microduck-build-tutorial** —— <https://github.com/AI-FanGe/Microduck-build-tutorial>
  - License: MIT
  - 引用：保姆级教程的流程范式
- **LuwuDynamics / xgoduck_rl** —— <https://github.com/LuwuDynamics/xgoduck_rl>
  - License: Apache-2.0
  - 引用：HD-1910 BAM M6 电机参数（kt=0.692 N·m/A）

## 其他参考

- **SaberOnGo / open-microduck** —— <https://github.com/SaberOnGo/open-microduck> —— 官方规格表中文版
- **unergybot / microduck-ubot** —— <https://github.com/unergybot/microduck-ubot> —— 关节参数详解
- **Datawhale / every-embodied** —— <https://github.com/datawhalechina/every-embodied> —— 强化学习教程
- **YDxun / microduck-duck-play** —— <https://github.com/YDxun/microduck-duck-play> —— 云端仿真 + 视觉玩法
- **emwstudio / DuckEMW** —— <https://github.com/emwstudio/DuckEMW> —— 跳舞训练权重
- **rustypot** —— <https://github.com/pollen-robotics/rustypot> —— 飞特协议 Rust 实现
- **microduck-lumi**（本项目自身）—— Radxa Zero 3W + 飞特 HD-1910 主线方案

## 硬件厂商

- **飞特 Feetech** —— <https://www.feetech.cn> —— HD-1910-C001 舵机 / URT-2 调试板 / 协议文档
- **瑞莎 Radxa** —— <https://radxa.com> —— Zero 3W 主控 / Radxa OS / 官方文档
- **Bambu Lab** —— 3D 打印机（参考）

## 论坛 / 社区

- **D-Robotics 论坛** —— <https://forum.d-robotics.cc/> —— RDK X5 路线（虽然我们用 Radxa Zero 3W，但云仿真教程可参考）
- **Bilibili** —— AI-FanGe / JoyandAI / emwstudio / snm0516 等 UP 主

## 免责声明

本项目**与 Pollen Robotics / Hugging Face / fanhao375 / JoyandAI / sim336 / AI-FanGe / LuwuDynamics 等无隶属关系，未获其背书**。

所有引用内容遵循各自原始许可证：

- 软件 / 代码：Apache-2.0
- 机械 3D 模型：CC BY-NC-SA 4.0（**禁止商用**，商用前请联系 Pollen Robotics）
- 训练策略：Apache-2.0
- 文档：本仓库自产部分为 Apache-2.0，引用部分保留原版权

---

最后更新：2026-10-07
