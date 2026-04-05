本阶段包含四个实验方向，所有方向共用同一个实验仓库，均基于 RISC-V 架构。

## 实验方向

| 方向 | 测试框架 | 测试位置 | 满分 |
|------|---------|---------|------|
| [CPU 建模](cpu/index.md) | TCG 测题 | `tests/gevico/tcg/` | 100 |
| [SoC 建模](soc/index.md) | QTest | `tests/gevico/qtest/` | 100 |
| [GPGPU 建模](gpu/index.md) | QTest (QOS) | `tests/qtest/gpgpu-test.c` | 100 |
| [Rust 建模](rust/index.md) | QTest + 单元测试 | 待定 | 待定 |

## 获取实验仓库

所有方向共用同一个实验仓库 `qemu-camp-2026-exper`。

**第一步**，通过 GitHub Classroom 邀请链接加入实验（链接由讲师提供），系统会自动将仓库 fork 到组织下并赋予你 maintainer 权限。

!!! warning "注意"

    请通过 Classroom 邀请链接获取仓库，不支持手动 fork。

**第二步**，clone 仓库到本地：

```bash
git clone git@github.com:gevico/qemu-camp-2026-exper-<你的 github 用户名>.git
```

**第三步**，添加上游远程仓库，用于同步上游代码变更：

```bash
git remote add upstream git@github.com:gevico/qemu-camp-2026-exper.git
git pull upstream main --rebase
```

## 环境搭建

参考各方向实验手册中的环境搭建说明。统一的编译配置命令：

```bash
make -f Makefile.camp configure
make -f Makefile.camp build
```

## 运行测试

```bash
make -f Makefile.camp test-cpu    # CPU 方向
make -f Makefile.camp test-soc    # SoC 方向
make -f Makefile.camp test-gpgpu  # GPGPU 方向
make -f Makefile.camp test-rust   # Rust 方向
make -f Makefile.camp test        # 全部方向
```

## 评分规则

- 每次推送到 `main` 分支，CI 自动编译、运行测试并计算得分
- 测试失败不会导致 CI 报错，只会降低得分
- 得分为 0 时不上传到排行榜
