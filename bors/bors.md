# Rust Bots 系列 - bors

在 Rust 官方项目中，bors 用来作为合并前的最后一道检查，由具有合入权限的 committer 或者 member 触发，触发后会按照需要重新执行一遍门禁检查。门禁检查通过后，会自动将代码合入主线，并关闭当前合入请求。

![image-20240407110236637](./images/image-20240407110236637.png)

## bors

[项目地址]: https://github.com/rust-lang/bors

bors 最新版本是由 infra 项目组几个 maintainer 用 Rust 重构的，之前版本是 [homu](https://github.com/rust-lang/homu)，这里只介绍重构后 bors 的部署方式。

### 1.项目准备

项目配置 - 创建分支

项目配置 - 配置 github app 及 webhook

项目配置 - 配置文件 rust-bors.toml

### 2.部署 bors

部署 - db

部署 - 运行

### 3.测试及验证

当前 bors 支持的命令集包括（通过 @bors 触发）：

| **Command**        | **Permissions** | **Description**                                              |
| ------------------ | --------------- | ------------------------------------------------------------ |
| `ping`             |                 | Send a ping to bors to check that it responds.               |
| `try`              | `try`           | Start a try build based on the most recent commit from the main branch. |
| `try parent=<sha>` | `try`           | Start a try build based on the specified parent commit `sha`. |
| `try cancel`       | `try`           | Cancel a running try build.                                  |

效果如下：

