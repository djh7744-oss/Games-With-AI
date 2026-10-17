# CLAUDE.md — 七国（SevenKingdoms）

## 项目概述

- **游戏名**：七国
- **类型**：战国背景开放世界沙盒 RPG + 骑砍式军团战斗 + 国家策略，PC 单机
- **一句话**：在战国乱世中，以任意出身开启人生，用你的方式改变历史。
- **引擎**：Unreal Engine 5.5+；C++（核心游戏逻辑）+ Blueprint（流程编排/UI/快速迭代）
- **参考对标**：骑马与砍杀2（战斗手感）、三国群英传（战略大地图）、三国志（国家策略）、太阁立志传5（角色成长）

## 权威文档（重要）

- `100_系统设计/` + `200_数据表/` 是唯一权威设计文档，任何冲突以本仓库为准
- `200_数据表/` 中 `DT_` 前缀的 CSV 是 UE5 DataTable 唯一导入来源
- 所有系统设计、数值、叙事内容均在此仓库，开发前先查对应文档
- 关键原则：**文档即真相、先文档后代码、数据驱动、AI 辅助人审定**

## 当前阶段：Phase 1 MVP（务必遵守边界）

详见 `000_GDD_概要/010_MVP开发边界与优先级.md`。核心原则：**先做 1%，让 1% 可玩**。

MVP 只做（3 件事）：
- 战斗系统基础：四方向攻击+格挡（M&B 核心手感）、骑马+马上战斗、100 人战场、3 兵种（步/弓/骑）
- 角色系统基础：六维属性（统率/武勇/智略/魅力/经营/工艺）、1 种出身、简单成长
- 地图：1 张 2km×2km 平原战场 + 1 座 Lv3 城池（市集/酒馆/城墙）

MVP 明确不做（做了就是浪费）：
- 经济、外交、内政、任务、科技树、学派、名剑、存档、DLC、UI 美化、音效、本地化
- 79 位历史将领（MVP 只需 1 个敌将）、40+ 城池（只需 1 城）
- 精细 3D 模型（白盒/商店资产即可）、对话系统（文字显示即可）

验收标准：RTX3060 上 100 人战场稳定 60FPS；骑砍式战斗手感达标；循环"骑马出城→战斗→回城"成立；安装包 <10GB。每 2 周一个可玩里程碑。

## 已定技术决策（详见 800_技术设计/800_技术架构概述.md）

| 决策点 | 选择 |
|--------|------|
| 大世界 | World Partition |
| 千人战场渲染 | Mass Entity（远景）+ Skeletal Mesh（近景） |
| AI 框架 | Behavior Tree + State Tree + EQS |
| 存档 | SaveGame + SQLite |
| 对话 | 自研 Dialogue Tree |
| UI | Common UI + UMG |
| 网络 | 单机首发，预留多人接口 |
| 音频 | MetaSounds（预算允许则 Wwise） |

- **性能目标**：1080p/30FPS（100v100）、1440p/60FPS（300v300）、4K/60FPS（1000v1000）
- **模块划分**：`Source/SevenKingdoms/{Core, Player, Combat, AI, Economy, Diplomacy, Strategy, Narrative, TechTree, UI}` + `SevenKingdomsEditor`

## 代码与资产规范

- 严格遵循 UE 命名前缀：A=Actor、U=UObject、F=结构体、E=枚举、T=模板、I=接口、S=Slate
- 类名、资产名、变量名一律英文；中文仅用于 UI 显示文本和 DataTable 内容列
- 数据驱动：一切数值配置进 DataTable/CSV，禁止 magic number 和硬编码
- 注释只写"为什么"（设计意图、性能权衡、历史考据依据），不写"是什么"
- 资产目录按 800 文档的 Content/ 结构组织（Characters/Maps/Weapons/UI/VFX/Audio/DataTables）
- 蓝图命名：`BP_角色名/系统名`，函数用动宾结构

## 文档与 Git 规范

- 文档编号：`目录号_子项号`（如 110=100 系统设计+10 战斗系统），全局唯一
- 新建文档：复制 `Templates/` 对应模板，填写 frontmatter（id/title/version/status/related）
- 版本号：v0.x 草稿 → v1.0 初版 → v2.0 重大修订 → vFINAL 锁定
- 提交信息：`[GDD]/[DATA]/[NARR]/[MAP]/[ART]/[AUDIO]/[UI]/[TECH]/[REF]/[TEMPLATE] + 说明`
- 分支：`main`（发布）/ `develop`（开发主线）/ `feature/xxx`
- 状态流转：draft → review → approved → implemented

## 与 Claude 协作约定

1. **.uasset 是二进制文件，我无法直接编辑**。涉及蓝图/资产改动时，我给出详细操作步骤由你在编辑器中完成；批量资产操作（重命名、批量导入、生成蓝图）我可以写 Editor Utility / Python 脚本
2. **C++ 代码我直接改**，你在 Rider/VS 编译或用 Live Coding，编译结果反馈给我
3. **CSV 数据我直接改源文件**，你在编辑器重新导入 DataTable
4. **我无法运行 UE 编辑器**，验证由你进行：把运行现象、截图、日志（Saved/Logs/）反馈给我，我来定位和修复
5. 删除或覆盖资产前必须先备份
6. 不要擅自扩大 MVP 范围——超出 010 文档边界的功能先提出讨论，不做
7. 修改设计文档前先确认改动意图，改完同步更新文档 frontmatter 的 version 和 last_modified

## 当前任务

Phase 0/1 起步：搭建 UE5.5 项目骨架（目录结构、模块划分、白盒场景），随后按 MVP 周计划推进四方向攻击原型。
