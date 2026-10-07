# 项目目录

```
microduck-lumi/
├── README.md                              # 项目入口（从这里开始）
├── LICENSE                                # Apache-2.0
├── NOTICE.md                              # 归属与许可证说明
├── CONTRIBUTING.md                        # 贡献指南
├── CODE_OF_CONDUCT.md                     # 社区行为准则
├── CHANGELOG.md                           # 变更日志
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md                  # Bug 报告模板
│   │   ├── feature_request.md             # 功能建议模板
│   │   └── question.md                    # 提问模板
│   ├── workflows/
│   │   └── lint.yml                       # Markdown 链接检查 CI
│   ├── PULL_REQUEST_TEMPLATE.md           # PR 模板
│   └── CODEOWNERS                         # 维护者
├── docs/
│   ├── 采购/
│   │   └── BOM-含淘宝链接.md              # 物料清单（含淘宝实链）
│   ├── 装配/
│   │   ├── 打印清单.md                    # 3D 打印指南
│   │   └── 整机集成上电流程.md            # 5 步逐级上电
│   └── 调试/
│       ├── 舵机配ID与方向校正.md          # 配 ID、零位、方向
│       ├── 上位机镜像烧录.md              # Radxa Zero 3W 系统配置
│       ├── 舵机控制台与协议层.md          # Python / Rust / URT-2 三种实现
│       ├── 策略部署.md                    # 训练 / 导出 / 板载推理
│       ├── 安全与故障保护.md              # 硬性安全规则
│       └── 常见坑位.md                    # 全网已知问题
├── hardware/                              # 硬件相关（KiCad 工程、HAT 设计等占位）
└── assets/                                # 图片、3D 模型预览等
```

---

## 快速链接

- **🛒 下单**：[docs/采购/BOM-含淘宝链接.md](docs/采购/BOM-含淘宝链接.md)
- **🖨️ 打印**：[docs/装配/打印清单.md](docs/装配/打印清单.md)
- **🔌 装配**：[docs/装配/整机集成上电流程.md](docs/装配/整机集成上电流程.md)
- **🎛️ 配舵机**：[docs/调试/舵机配ID与方向校正.md](docs/调试/舵机配ID与方向校正.md)
- **💻 烧系统**：[docs/调试/上位机镜像烧录.md](docs/调试/上位机镜像烧录.md)
- **🐍 协议层**：[docs/调试/舵机控制台与协议层.md](docs/调试/舵机控制台与协议层.md)
- **🧠 训练部署**：[docs/调试/策略部署.md](docs/调试/策略部署.md)
- **🛡️ 安全**：[docs/调试/安全与故障保护.md](docs/调试/安全与故障保护.md)
- **🪤 踩坑**：[docs/调试/常见坑位.md](docs/调试/常见坑位.md)

---

## 维护者

- **组织**：[lumimoon-robotics](https://github.com/lumimoon-robotics)（启月探微）
- **项目作者**：[AiNewView](https://github.com/AiNewView)
- **关联项目**：[qiyue-tanwei-website](https://github.com/lumimoon-robotics/qiyue-tanwei-website)（启月探微官方网站）

## 反馈

- 提交 [GitHub Issue](https://github.com/lumimoon-robotics/microduck-lumi/issues)
- 微信群 / B 站评论区
- 发邮件给启月探微官方
