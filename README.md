# AutoForensicAI Knowledge Base (小空自己动 — 知识库)

> **骨架仓库**: [xiaokon_autoforensicai](https://github.com/HJSSSRX/xiaokon_autoforensicai)
>
> 本仓库存放知识库数据，与骨架仓库的 `knowledge/` 目录完全同步。

## 目录结构

```
knowledge/
├── solved/          # 结构化题解 (YAML frontmatter + Markdown)
├── skills/          # 角色技能速查 (按 computer/mobile/network/web/stego_crypto 分类)
├── cards/           # 知识卡片 (索引、参考链接)
└── wp_index/        # 历年 writeup 索引 (8 场比赛, grep 友好)
```

## 内容统计

| 目录 | 文件数 | 说明 |
|------|--------|------|
| `solved/` | 19 | 2022长安杯/2024FIC/2026FIC + 解题套路 |
| `skills/` | 7 | Volatility/注册表/Webshell/宝塔/比赛手册 |
| `cards/` | 1 | Forensics-Wiki 外部链接索引 |
| `wp_index/` | 8 | FIC/平航杯/盘古石/美亚杯 题目索引 |

## 使用方式

### 方式 1：配合骨架仓库

```powershell
git clone https://github.com/HJSSSRX/autoforensicai_data.git _data
Copy-Item -Recurse _data\knowledge\* e:\项目\自动化取证\knowledge\ -Force
```

### 方式 2：在骨架仓库中直接更新

```powershell
# 修改骨架仓库的 knowledge/ 后同步到数据仓库
Copy-Item -Recurse e:\项目\自动化取证\knowledge\* e:\项目\autoforensicai_data\knowledge\ -Force
cd e:\项目\autoforensicai_data
git add -A && git commit -m "sync knowledge base" && git push
```

## 数据格式

每篇题解使用 YAML frontmatter + Markdown：
```yaml
---
tags: [memory_forensics, volatility, pslist]
tools: [volatility3]
category: computer_forensics
difficulty: easy
source: example
---
# Title: ...
## Problem
## Solution Steps
## Key Takeaways
```

## 贡献

添加新内容：按对应目录放置，遵循现有格式。
