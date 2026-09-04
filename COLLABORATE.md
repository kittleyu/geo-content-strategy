# 协作说明（方法论层 skill）

本仓库是 GEO 内容生文策略的方法论 skill（`geo-content-strategy`），定位为 GEO Skill 生态的**策略层**：回答「怎么分析词条、怎么写、发哪里、怎么验证」，与执行层引擎分工协作。

## 〇、GEO Skill 生态（三方分工，先看这里）

| 仓库 | 层 | 职责 | 可见性 |
|---|---|---|---|
| `geo-content-strategy` | 方法论/策略层（本仓库） | 分析(反查法)-生文(黄金结构)-发布(三层平台)-验证(盲评/收录) 全流程方法论 | Public |
| `geo-article-generator` | 执行层·通用引擎 | 客户资料→实体知识库→批量生文 | Private |
| `lawyer-geo-content` | 执行层·垂直应用 | 刑事辩护客户落地（引擎子类） | Private |

- 方法论 → 引擎：先按本仓库定策略，再用 `geo-article-generator` 批量执行。
- 引擎 → 垂直：`lawyer-geo-content` 复用 `geo-article-generator` 的 scripts/templates，仅换 config 与资料。
- 垂直/引擎 → 方法论：实战沉淀的新占位角度、平台经验、验证结论回写本仓库。
- 仓库地址：`https://github.com/kittleyu/geo-article-generator`、`https://github.com/kittleyu/lawyer-geo-content`（均私有）。

## 一、本仓库信息
- 角色：GEO 内容生文策略方法论（geo-content-strategy）
- GitHub：`https://github.com/kittleyu/geo-content-strategy.git`
- 本机 skill 路径：随使用环境而定（豆包侧位于 `workspace/.user_skills/geo-content-strategy`；WorkBuddy 侧可置于 `C:\Users\daixiaoyu\.workbuddy\skills\geo-content-strategy`）
- 仓库为 Public，可公开浏览。

## 二、如何使用（读取 / 调用）
- WorkBuddy 类 agent：用 Skill 工具加载，name = `geo-content-strategy`，按 `SKILL.md` 操作。
- 核心流程：分析 → 生文 → 发布 → 验证 → 迭代；每步触发时读 `references/` 对应文件（analysis / writing / publishing / verification / prompts）。
- 与引擎衔接：策略定好后，批量出稿交给 `geo-article-generator`（客户资料 + config）；垂直客户见 `lawyer-geo-content`。

## 三、如何参与更新（改源码并推回 GitHub）
本目录已是 git 仓库，remote 已配好。改动后执行：
```
git pull --rebase origin main
git add -A
git commit -m "改动说明"
git push origin main
```

## 四、协作规范
- 以 GitHub 为唯一真相源：任何改动都 push，另一方 `git pull` 拉最新。
- 禁止 force push。
- 不提交客户专属资料。
- 改前先 `git pull --rebase`，改完先 commit 再 push。
