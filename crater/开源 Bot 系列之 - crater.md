## 开源 Bot 系列之 - crater

[开源项目地址]: https://github.com/rust-lang/crater

### 一：背景

**讲故事**

crater 是一个在整个 Rust 生态系统中运行 `实验` 的工具。其主要目的是检测 Rust 编译器中的回归，它通过构建大量 crate、运行测试套件比较两个版本的 Rust 编译器之间的结果来实现这一点。

在 crater 中，通常一次回归会被视为一次`实验`，常见的例子就是PR提交，由 commitA 到 commitB。

### 二：分析

**模块 · 存储**

基于 r2d2 和 rusqlite 第三方 crater 实现的 SQLite 数据库文件存储。r2d2 实现了连接池，rusqlite 实现了对 SQLite 的组织管理。

**`crater.db`** (后续可能有更改)

- experiments

| name (PRIMARY KEY) | mode | cap_lint | toolchain_start | toolchain_end | priority | created_at | status | github_issue |
| ------------------ | ---- | -------- | --------------- | ------------- | -------- | ---------- | ------ | ------------ |

| github_issue_url | github_issue_number | assigned_to | started_at | completed_at | repo_url | FOREIGN_KEY (assigned_to) REFERENCES agents(name) ON DELETE SET NULL |
| ---------------- | ------------------- | ----------- | ---------- | ------------ | -------- | ------------------------------------------------------------ |

- experiments_crates

| experiment | crate | skiped | FOREIGN_KEY(experiment) REFERENCES experiments(name) ON DELETE CASCADE |
| ---------- | ----- | ------ | ------------------------------------------------------------ |

- results

| experiment | crate | toolchain | result | log  | FOREIGN_KEY(experiment, crate, toolchain) ON CONFLICT REPLACE | FOREIGN_KEY(expoeriment) REFERENCE experiments(name) ON DELETE CASCADE |
| ---------- | ----- | --------- | ------ | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |

- shas

| experiment | org  | name | sha  | FOREIGN_KEY(experiment) REFERENCES experiment(name) ON DELETE CASCADE |
| ---------- | ---- | ---- | ---- | ------------------------------------------------------------ |

- agents

| name | last_heardbeat | git_version |
| ---- | -------------- | ----------- |

- saved_names

| issue | experiment |
| ----- | ---------- |

### 三：使用

**环境准备**



**配置 Bot**



**配置agent**



**配置server**



[agent-http-api]: https://github.com/rust-lang/crater/blob/master/docs/agent-http-api.md
[agent-machine-setup-windows]: https://github.com/rust-lang/crater/blob/master/docs/agent-machine-setup-windows.md
[agent-machine-setup]: https://github.com/rust-lang/crater/blob/master/docs/agent-machine-setup.md
[bot-usage]: https://github.com/rust-lang/crater/blob/master/docs/bot-usage.md
[cli-usage]: https://github.com/rust-lang/crater/blob/master/docs/cli-usage.md

### 简单实践

用一个最小集的例子来部署