本文档是专业阶段 Rust 方向的实验手册。

实验内容待定。

## 环境搭建

第一步，安装 QEMU 开发依赖和 Rust 工具链。

```bash
# Ubuntu 24.04
sudo sed -i 's/^Types: deb$/Types: deb deb-src/' /etc/apt/sources.list.d/ubuntu.sources
sudo apt-get update
sudo apt-get build-dep -y qemu

# 安装 Rust 工具链（版本要求 >= 1.85）
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
. "$HOME/.cargo/env"
cargo install bindgen-cli
```

!!! note "提示"

    安装 QEMU 开发环境，请参考导学阶段的 Step0。

第二步，通过讲师提供的 GitHub Classroom 邀请链接加入实验，系统会自动将仓库 fork 到组织下并赋予你 maintainer 权限。

!!! warning "注意"

    请通过 Classroom 邀请链接获取仓库，**不支持手动 fork**。

第三步，clone 仓库到本地：

```bash
git clone git@github.com:gevico/qemu-camp-2026-exper-<你的 github 用户名>.git
```

第四步，添加上游远程仓库，用于同步上游代码变更：

```bash
git remote add upstream git@github.com:gevico/qemu-camp-2026-exper.git
git pull upstream main --rebase
```

第五步，配置并编译：

```bash
make -f Makefile.camp configure
make -f Makefile.camp build
```

## 提交代码

待补充。

## 测评验收

本地运行全部测题：

```bash
make -f Makefile.camp test-rust
```

待补充。
