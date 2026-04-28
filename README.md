# ForensicAI Training Data (小空自己动 — 数据仓库)

> **骨架仓库**: [xiaokon_autoforensicai](https://github.com/HJSSSRX/xiaokon_autoforensicai)
>
> 本仓库仅存放训练数据，不含框架代码。

## 内容

| 目录 | 说明 |
|------|------|
| `knowledge/solved/` | 结构化题解 (YAML 格式，按比赛分目录) |
| `knowledge/wp_index/` | 历年 writeup 索引 (8 场比赛) |
| `AI_BRAIN/solved_patterns/` | 已验证解法模式 (Markdown) |
| `share/reference_case/` | FIC2026 参考案例 (完整答案批次) |

## 使用方式

### 方式 1：配合骨架仓库自动下载

```powershell
# 在骨架仓库目录下运行
.\bootstrap.ps1 -DataSource github
```

`bootstrap.ps1` 会自动克隆本仓库并合并数据到正确位置。

### 方式 2：手动合并

```powershell
git clone https://github.com/HJSSSRX/autoforensicai_data.git _data
Copy-Item -Recurse _data\knowledge\* knowledge\ -Force
Copy-Item -Recurse _data\AI_BRAIN\* AI_BRAIN\ -Force
```

## 数据格式

每道题解为一个 YAML 文件，Schema 见骨架仓库 `share/SYSTEM_DESIGN.md` §3.1。

## 贡献

添加新题解：按 `knowledge/solved/<比赛ID>/<题号>.yaml` 命名，遵循现有 YAML 格式。
