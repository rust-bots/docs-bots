## 开源 Bot 系列之 - crater

[开源项目地址]: https://github.com/rust-lang/crater

### 一：背景

**讲故事**

crater 是一个在整个 Rust 生态系统中运行 `实验` 的工具。其主要目的是检测 Rust 编译器中的回归，它通过构建大量 crate、运行测试套件比较两个版本的 Rust 编译器之间的结果来实现这一点。

在 crater 中，通常一次回归会被视为一次`实验`，常见的例子就是PR提交，由 commitA 到 commitB。

### 二：分析

**模块 · 存储**

基于 r2d2 和 rusqlite 第三方 crater 实现的 SQLite 数据库文件存储。r2d2 实现了连接池，rusqlite 实现了对 SQLite 的组织管理。

**`crater.db`** 存储的表  (后续可能有更改)

- experiments

- experiments_crates

- results

- shas

- agents

- saved_names

### 三：使用

**环境准备**

参考[crater/Dockerfile at master · rust-lang/crater (github.com)](https://github.com/rust-lang/crater/blob/master/Dockerfile)

建议先构建好 `crater` 二进制文件，再将其分发到 server 和 agent 服务器上。

**配置 Bot**

1.创建一个 Github 账户作为 bot 触发的账号(可以将名字取为 craterbot)。

2.进入 bot 账户的 setting，点击 Developer settings。

3.创建 Personal access tokens -> Tokens (calssic)。

4.为需要配置 craterbot 的项目创建 webhook（Payload url 为下面配置的 server 服务器的`公有IP地址`  + `/webhook`）。

**配置server**

1.按照个人需要准备配置文件：

- config.toml

  按需修改（示例中保持原状）

- token.toml

  配置 `bot.webhooks-secret` 值对应上述第四步；

  配置 `bot.api-token` 值对应上述第三步；

  配置 `agents` 这一步有一点反人类，对应的值的格式是 

  - 左边：agent 连通 server 所需要的 token 值；
  - 右边：agent 名称；

2.运行服务

- IP 地址必须为私有地址

``````shell
crater server --bind 172.29.48.173:8000
``````

**配置agent**

1.再环境准备阶段配置好后，可直接运行 `agent`

- IP 地址必须为公有地址

``````shell
crater agent http://120.25.225.55:8000 crater-slave --capabilities linux
``````

[agent-http-api]: https://github.com/rust-lang/crater/blob/master/docs/agent-http-api.md
[agent-machine-setup-windows]: https://github.com/rust-lang/crater/blob/master/docs/agent-machine-setup-windows.md
[agent-machine-setup]: https://github.com/rust-lang/crater/blob/master/docs/agent-machine-setup.md
[bot-usage]: https://github.com/rust-lang/crater/blob/master/docs/bot-usage.md
[cli-usage]: https://github.com/rust-lang/crater/blob/master/docs/cli-usage.md

### 简单实践

将 HshenBot 添加作为项目的 Collaborator 并给 Admin 权限（需要 HshenBot 账户那边接受邀请）。

``````
@HshenBot ping
``````

``````
@HshenBot run start=master#bce525323d98a397426f69b90d443385963e5879 end=master#37f97982caf438f673bffd1d1bc264d63c741bb5 mode=check-only
``````

``````
@HshenBot abort
``````

