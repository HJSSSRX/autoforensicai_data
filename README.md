# 🔍 Digital Forensics & CTF Knowledge Base

**开源电子取证 + CTF 知识库** — 结构化题解、工具速查、比赛题目索引。

> 无论你用什么框架、什么 AI、什么平台，这些知识都能直接用。
>
> 配套自动化框架: [ForHacker (xiaokon_autoforensicai)](https://github.com/HJSSSRX/xiaokon_autoforensicai) — 可选，知识库本身完全独立。

---

## 快速开始

```bash
git clone https://github.com/HJSSSRX/autoforensicai_data.git
cd autoforensicai_data

# 搜索知识库
python search.py --ask "内存取证怎么查可疑进程"
python search.py --tags memory_forensics volatility
python search.py --tools vol3 strings
python search.py --text "reverse shell"
```

不需要安装任何框架，只需 Python 3.6+ 和 `pyyaml`：
```bash
pip install pyyaml
```

---

## 目录结构

```
knowledge/
├── solved/         22 篇结构化题解（可搜索、可复用）
│   ├── 2022_changancup_*.md      # 2022 长安杯（计算机/手机/服务器）
│   ├── 2024fic_*.md              # 2024 FIC 决赛（6 个板块）
│   ├── 2026fic_*.md              # 2026 FIC
│   └── pattern_*.md              # 通用解题套路（AES/微信/隐写/挂载...）
│
├── skills/         10 篇工具速查与技巧
│   ├── competition_playbook.md   # 比赛实战手册
│   ├── computer/                 # Volatility、注册表取证
│   ├── network/                  # WebShell 流量分析
│   └── web/                      # 宝塔面板取证
│
├── cards/          外部资源索引
│   └── forensics_wiki_writeup_index.md
│
├── wp_index/       8 场比赛题目索引
│   ├── FIC-2024决赛.md / FIC-2025初赛.md / FIC-2025决赛.md
│   ├── 平航杯-2025初赛.md / 盘古石-2025晋级赛.md
│   └── 美亚杯-2024团队赛.md / 美亚杯-2025资格赛.md
│
├── problems/       52 张题目卡（v0.5+ 新增）
│   ├── computer/   # 各板块 question card
│   ├── mobile/     # 含官方答案 vs 我们答案 + 差距分析
│   └── server/
│
├── techniques/     11 张技巧卡（v0.5+ 新增）
│   └── *.yaml      # Volatility, BitLocker, 微信解密, 钱包还原...
│
└── retrospectives/ 19 篇赛后复盘 (v0.5+ 新增, 多机贡献区)
    ├── INDEX.yaml                          # 自动生成, 不要手改
    ├── 20260509_C01__A_main.yaml          # A 机视角
    ├── 20260509_C01__B_second.yaml        # 同题 B 机视角 (双保留)
    └── ...

cases/              历史案件特化资源（脚本/产物/wp_batches）
├── 2025平航杯/      # 139 个 artifacts + 14 个 wp_batches
└── 2026FIC/         # 6 个文档 + disk_map
```

---

## 数据格式

每篇题解使用 **YAML frontmatter + Markdown**，方便 grep、AI、脚本解析：

```yaml
---
tags: [memory_forensics, volatility, pslist, windows]
tools: [volatility3, strings]
category: computer_forensics
difficulty: medium
source: 2024FIC_决赛_板块1
date: 2026-05-06
verified: true
---
# 标题

## Problem
题目描述 + 检材说明

## Solution Steps
1. 具体命令 + 输出
2. 分析过程
3. 得到答案

## Key Takeaways
- 可复用的经验

## Answer
具体答案
```

### 标签体系

| 类别 | 常用 tags |
|---|---|
| 计算机取证 | `disk_forensics`, `memory_forensics`, `registry`, `event_log`, `browser`, `ntfs` |
| 手机取证 | `android`, `ios`, `wechat`, `call_log`, `apk_analysis` |
| 网络分析 | `pcap`, `http`, `dns`, `bluetooth`, `usb`, `webshell_traffic` |
| Web 渗透 | `sql_injection`, `xss`, `ssrf`, `upload`, `webshell` |
| 隐写密码 | `steganography`, `lsb`, `aes`, `xor`, `base64`, `hash` |
| 逆向工程 | `exe_reverse`, `apk_reverse`, `upx`, `ida`, `ghidra` |

---

## 搜索方式

### 命令行搜索

```bash
# 自然语言（中英文均可）
python search.py --ask "如何解密微信数据库"
python search.py --ask "how to mount E01 image"

# 按标签
python search.py --tags registry windows

# 按工具
python search.py --tools volatility3

# 全文搜索
python search.py --text "reverse shell"

# 按分类
python search.py --category computer_forensics
```

### 直接 grep

```bash
# 找所有用了 volatility 的题解
grep -rl "volatility" knowledge/solved/

# 找所有 medium 难度
grep -rl "difficulty: medium" knowledge/solved/

# 找所有和微信相关的
grep -rl "wechat\|微信" knowledge/
```

### AI 使用

直接告诉你的 AI：
> "读一下 knowledge/solved/ 目录，找找有没有关于 XXX 的解题方案"

---

## 贡献指南

### 添加题解

1. 在 `knowledge/solved/` 下创建 `比赛名_板块_描述.md`
2. 按上面的 YAML frontmatter 格式填写
3. 确保 `tags` 和 `tools` 字段完整（这是搜索的关键）
4. `verified: true` 仅用于已确认正确的答案
5. 提交 PR 或直接 push

### 添加技能速查

1. 在 `knowledge/skills/对应角色/` 下创建 `.md`
2. 内容以 CLI 命令为主，附简要说明

### 添加比赛索引

1. 在 `knowledge/wp_index/` 下创建 `比赛名-年份.md`
2. 列出所有题目编号、类型、关键词

---

## 覆盖的比赛

| 比赛 | 年份 | 内容 |
|---|---|---|
| 长安杯 | 2022 | 计算机/手机/服务器取证完整题解 |
| FIC 决赛 | 2024 | 6 板块 68 题（计算机/PVE/OpenWrt/云手机/服务器/数据分析） |
| FIC | 2025/2026 | 初赛 + 决赛题目索引 + 部分题解 |
| 平航杯 | 2025 | 初赛题目索引 |
| 盘古石 | 2025 | 晋级赛题目索引 |
| 美亚杯 | 2024/2025 | 团队赛 + 资格赛题目索引 |

---

## License

知识库内容以 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可共享。
搜索脚本以 MIT 许可发布。


---

## 🤝 多机协作 (v0.5+)

### 背景
本仓库设计为**多台 AI 机器同时贡献**. 为避免合并冲突, 必须遵守:

### 文件命名规则
所有 etrospectives/ 复盘必须带机器 ID 后缀:
```
✓ 20260509_C01_os_version__A_main.yaml      ← A 机贡献
✓ 20260509_C01_os_version__B_second.yaml    ← B 机同题不同视角 (合并不冲突)
✗ 20260509_C01_os_version.yaml              ← 缺机器后缀, 多机时必冲突
```

### INDEX 自动生成
INDEX.yaml / INDEX.md 由 [	ools/build_kb_index.py](https://github.com/HJSSSRX/xiaokon_autoforensicai/blob/main/tools/build_kb_index.py) 自动重建.
**不要手改**, pre-commit hook 会拒绝.

### 上手步骤 (新机器)
1. clone 框架库: `git clone https://github.com/HJSSSRX/xiaokon_autoforensicai`
2. 运行: `python tools/setup_machine.py` (设 MACHINE_ID + 装 hooks)
3. 在框架库加 data remote: `git remote add data file:///path/to/autoforensicai_data`
4. 写新复盘到 `knowledge/retrospectives/<日期>_<编号>__<MACHINE_ID>.yaml`
5. 一键 push 三库: `python tools/safe_push.py`

完整规则: [docs/MULTI_MACHINE_CONTRIBUTION.md](https://github.com/HJSSSRX/xiaokon_autoforensicai/blob/main/docs/MULTI_MACHINE_CONTRIBUTION.md)

### 三库联动
- **本仓库 (utoforensicai_data)**: 知识 + 案件资料 (本仓库, 多机共写)
- **框架库 (xiaokon_autoforensicai)**: 工具代码 + 火眼适配 + Hub
- **全量库 (xiaokon-all)**: 两者合并的快速部署镜像
