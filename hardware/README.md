# hardware/

本目录包含 PCB 工程、原理图、机械图纸。

## 子目录

- `pcb/` - 印刷电路板工程
  - `zero-robot-hat/` - Zero Robot HAT（4 层，65×30.9 mm）
  - `imu-to-dxl-v2/` - IMU 转接板（STM32G031 + LSM6DSV16X）
- `mechanical/` - 机械图纸
  - `solidworks/` - SolidWorks 源文件
  - `stl/` - STL 打印件
  - `step/` - STEP 通用格式
  - `3mf/` - 拓竹 Studio 切片文件

## 当前状态

⚠️ **本目录的 PCB 工程为占位文件**，请参考以下开源资源：

1. **Zero Robot HAT**（Apache-2.0）
   - [pollen-robotics/elec_RPI_Robot_HAT](https://github.com/pollen-robotics/elec_RPI_Robot_HAT)
   - KiCad 9 工程 + Gerber + BOM + 贴片坐标
   - 完整开源，可直接打样

2. **imu_to_dxl v2**（需自绘）
   - 协议已完整还原：[fanhao375/microduck-replica/docs/硬件方案逆向.md](https://github.com/fanhao375/microduck-replica)
   - 配套固件：[fanhao375/microduck-replica/hardware/imu_to_dxl/firmware](https://github.com/fanhao375/microduck-replica)

3. **替代方案（不打 HAT）**：[fanhao375/microduck-replica/docs/不打HAT.md](https://github.com/fanhao375/microduck-replica/blob/master/docs/%E4%B8%8D%E6%89%93HAT.md)
   - ¥22 半双工转接板 + ¥15 UBEC 5V/3A
   - 5 分钟接好，省去打板成本

## 打印件来源

- **官方仿真模型**（CC BY-NC-SA 4.0）
  - [pollen-robotics/microduck](https://github.com/pollen-robotics/microduck)
  - 38 种网格 / 75 实例
- **飞特版适配**（CC BY-NC-SA 4.0）
  - [JoyandAI/OpenMicroDuck](https://github.com/JoyandAI/OpenMicroDuck) `print/`
  - 47 种网格 / 75 实例
  - 8 个舵盘配合件专为 HD-1910 设计
- **完整复刻装配**
  - [fanhao375/microduck-replica-cad](https://github.com/fanhao375/microduck-replica-cad) 飞特版 v2.1
  - SolidWorks / STEP 源文件
  - 带 -FT 后缀的 8 个重新设计配合件
