# Changelog

所有对本项目的重大变更都会记录在此文件。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，
本项目遵循 [语义化版本](https://semver.org/lang/zh-CN/)。

## [Unreleased]

### 计划中
- 整合 `microduck-lumi` 的 RDK X5 + 微雪 ESP32 备选方案（作为独立分支）
- 补全 HAT 板 KiCad 工程（参考 Pollen 官方）
- 补全 imu_to_dxl v2 协议实现
- 翻译 README 与核心文档为英文

## [0.1.0] - 2026-10-07

### Added
- 项目初始发布
- 完整 README（项目介绍 / 选型 / 硬件 / 双供电方案 / BOM / 调试 / 训练 / 安全）
- 物料清单（docs/采购/BOM-含淘宝链接.md）
- 6 张淘宝店铺链接（已剥掉追踪参数，保留 id + skuId）
- 3D 打印清单（docs/装配/打印清单.md）
- 整机集成上电流程（5 步逐级推进）
- 舵机配 ID 与方向校正指南
- Radxa Zero 3W 上位机镜像烧录指南
- 舵机控制台与协议层（Python / Rust / URT-2 三种实现）
- 训练 / 导出 / 板载推理部署指南
- 安全与故障保护（硬性安全规则 + 应急方案）
- 常见坑位与排查（9 大类问题）
- 13 个上游开源仓库的归功与许可证（NOTICE.md）
- 完整 Apache-2.0 LICENSE

[Unreleased]: https://github.com/lumimoon-robotics/microduck-lumi/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/lumimoon-robotics/microduck-lumi/releases/tag/v0.1.0
