本文档是专业阶段 GPGPU 方向的实验手册。

专业阶段 GPGPU 方向的实验围绕 PCIe GPGPU 设备建模展开，你需要按照 [GPU 硬件手册][5] 给出的寄存器映射与 SIMT 架构规格，完成 GPGPU 设备的 MMIO 寄存器、DMA 引擎、SIMT 执行核心（RV32IF 指令解释器）以及低精度浮点（BF16/FP8/FP4）转换指令的实现。

## 环境搭建

第一步，安装 QEMU 开发依赖。

```bash
# Ubuntu 24.04
sudo sed -i 's/^Types: deb$/Types: deb deb-src/' /etc/apt/sources.list.d/ubuntu.sources
sudo apt-get update
sudo apt-get build-dep -y qemu

# 安装 Rust 工具链
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
. "$HOME/.cargo/env"
cargo install bindgen-cli
```

!!! note "提示"

    GPGPU 方向使用 QTest（QOS）测试框架，测题在宿主机侧编译运行，**不需要** RISC-V 交叉编译工具链。

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

GPGPU 设备的源码位于 `hw/gpgpu/` 目录，测试源码位于 `tests/qtest/gpgpu-test.c`。

你需要修改 `hw/gpgpu/gpgpu.c` 和 `hw/gpgpu/gpgpu_core.c` 中标记为 `TODO` 的函数，实现设备的完整功能。头文件 `gpgpu.h` 和 `gpgpu_core.h` 定义了完整的寄存器映射和数据结构，请勿修改。

每次实验完成后，提交代码：

```bash
git add .
git commit -m "feat: subject..."
git push origin main
```

## 测评验收

本地运行全部测题：

```bash
make -f Makefile.camp test-gpgpu
```

GPGPU 方向共 17 个子测试，满分 100 分（score = passed x 100 / 17）。

测题列表：

| # | 测试名 | 验证内容 |
|---|--------|---------|
| 1 | device-id | 设备 ID 和版本寄存器 |
| 2 | vram-size | VRAM 容量寄存器 |
| 3 | global-ctrl | 全局控制与状态寄存器 |
| 4 | dispatch-regs | Grid/Block 维度寄存器 |
| 5 | vram-access | BAR2 VRAM 读写 |
| 6 | dma-regs | DMA 引擎寄存器 |
| 7 | irq-regs | 中断使能/状态寄存器 |
| 8 | simt-thread-id | 线程 ID 寄存器 |
| 9 | simt-block-id | Block ID 寄存器 |
| 10 | simt-warp-lane | Warp/Lane ID 寄存器 |
| 11 | simt-thread-mask | 线程活跃掩码 |
| 12 | simt-reset | 软复位清零 SIMT 上下文 |
| 13 | kernel-exec | RV32I 内核执行 |
| 14 | fp-kernel-exec | RV32F 浮点内核执行 |
| 15 | lp-convert | BF16/E4M3 低精度浮点转换 |
| 16 | lp-convert-e5m2-e2m1 | E5M2/E2M1 低精度浮点转换 |
| 17 | lp-convert-saturate | 溢出饱和与边界条件 |

每道测题的得分在排行榜上实时更新，每次 push 到 `main` 分支会触发 CI 自动评测。

[5]: gpu-datasheet.md
