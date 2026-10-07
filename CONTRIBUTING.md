# 贡献指南

感谢你对 `microduck-lumi` 项目的关注！我们欢迎任何形式的贡献——提 Issue、写文档、修错别字、补缺失章节、分享你的复刻经验。

## 行为准则

请阅读并遵守 [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md)。本项目致力于打造开放友好的社区环境。

## 我可以怎么贡献？

### 1. 报告 Bug

在提 Bug 之前：

- 搜索 [Issues](https://github.com/lumimoon-robotics/microduck-lumi/issues) 看是否已有相同问题
- 确认是**仓库本身**的问题，还是**硬件 / 上游项目**的问题

提交 Bug 时请用 [Bug Report 模板](https://github.com/lumimoon-robotics/microduck-lumi/issues/new?template=bug_report.md)，并附：

- 复现步骤（一步步描述）
- 期望结果
- 实际结果
- 你的环境（舵机型号、主控型号、电池、镜像版本、ONNX Runtime 版本）
- 截图 / 视频 / 日志

### 2. 提功能建议

请用 [Feature Request 模板](https://github.com/lumimoon-robotics/microduck-lumi/issues/new?template=feature_request.md)，说明：

- 这个功能解决什么问题
- 你建议的实现思路
- 是否有上游项目的相关 issue 或 PR

### 3. 改进文档

- 修复错别字
- 补充缺失的说明
- 翻译为英文 / 其他语言
- 增加截图 / 配图

文档修改**最容易被接受**，新手推荐从这里开始。

### 4. 贡献代码

代码修改流程：

1. Fork 本仓库
2. 创建特性分支（`git checkout -b feature/your-feature`）
3. 提交修改（`git commit -m 'feat: add your feature'`）
4. 推送到你的 Fork（`git push origin feature/your-feature`）
5. 创建 Pull Request，使用 [PR 模板](.github/PULL_REQUEST_TEMPLATE.md)

## 提交信息规范

请使用 [Conventional Commits](https://www.conventionalcommits.org/zh-hans/)：

```
<类型>(<作用域>): <简短描述>

[可选的正文]

[可选的脚注]
```

**类型**：

- `feat` — 新功能
- `fix` — Bug 修复
- `docs` — 文档变更
- `style` — 代码风格调整（不影响功能）
- `refactor` — 重构
- `test` — 添加 / 修改测试
- `chore` — 杂项（CI / 工具链等）

**示例**：

- `docs: 修正舵机配 ID 步骤的编号错位`
- `fix: BOM 中 2S 电池链接的 skuId`
- `feat: 新增 ND 滤镜 / 5G 频道的舵机驱动支持`

## 样式规范

### Markdown

- 中文文案使用**全角标点**（，。！？：；）
- 英文与中文之间留一个空格
- 标题用 `##`，不用 `#`（H1 留给页面 title）
- 列表项用 `-` 不用 `*`
- 代码块用三个反引号 + 语言标识
- 表格对齐用 `|------|` 形式

### 链接

- 内部文档用相对路径（`docs/装配/打印清单.md`）
- 外部链接用绝对 URL
- 淘宝链接**只保留 `id=` 和 `skuId=`**，剥掉所有 `spm=` `mi_id=` `xxc=` 等追踪参数

### 引用

- 上游项目引用时给出版本 / commit hash（如果可能）
- 论坛 / 视频教程引用时给出完整 URL
- 内部引用用 `microduck-lumi` 仓库路径

## 审核流程

- 普通文档修改：1 个维护者审核通过即可合并
- 涉及 BOM 变更：1 个维护者 + 1 个用户复刻者审核
- 涉及代码变更：1 个维护者 + CI 通过

## 许可证

向本项目贡献即表示你同意你的贡献以 [Apache-2.0](LICENSE) 协议发布。

---

有任何问题？欢迎在 [Discussions](https://github.com/lumimoon-robotics/microduck-lumi/discussions) 提问。
