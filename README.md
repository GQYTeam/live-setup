# live-setup

[![Arch Linux](https://img.shields.io/badge/Arch_Linux-1793D1?logo=archlinux&logoColor=white)](https://archlinux.org/)
[![Bash](https://img.shields.io/badge/Bash-4EAA25?logo=gnubash&logoColor=white)](https://www.gnu.org/software/bash/)
[![ShellCheck](https://img.shields.io/badge/ShellCheck-passed-2F855A?logo=gnu-bash&logoColor=white)](https://www.shellcheck.net/)
[![CI](https://github.com/GQYTeam/live-setup/actions/workflows/ci.yml/badge.svg)](https://github.com/GQYTeam/live-setup/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](install-arch/LICENSE)

面向 Arch Linux Live 环境的半自动安装器，帮助新手快速完成系统安装和基础环境配置。安装器支持加密 LVM、GRUB、命令行环境、Hyprland 工作站以及 VirtualBox 虚拟机环境。

> [!CAUTION]
> 默认安装会清空选中的整块磁盘，不支持双系统、保留现有分区或在已安装系统上升级。运行前请确认目标磁盘和备份状态。

## 功能

- UEFI 启动、LUKS2 加密和 LVM 分区
- 自动安装并配置 GRUB、NetworkManager、UFW 和 fail2ban
- 三种安装模式：最小 CLI、Hyprland 工作站、VirtualBox 工作站
- 自动配置用户、zsh、Docker、Rust 工具链和 dotfiles
- 支持 `--dry-run`、`--test-mode` 和安装失败后的 `--resume`
- 独立诊断日志：`/tmp/install-arch-debug.log`

## 快速开始

请从 Arch Linux 官方 Live ISO 启动，并确认处于 UEFI 模式、拥有 root 权限、网络正常，且 `/mnt` 未被挂载。

---
在 Live 环境中直接下载并执行上游脚本：

```bash
curl -fsSL https://raw.githubusercontent.com/GQYTeam/live-setup/main/install-arch/install-arch.sh | bash
```

----
测试 UEFI：

```bash
test -d /sys/firmware/efi/efivars && echo "UEFI mode confirmed"
lsblk -d -o NAME,SIZE,MODEL
```

运行安装器：

```bash
cd install-arch
./install-arch.sh
```


安装器会在修改磁盘前显示目标设备并要求确认。详细的分区布局、安装模式和恢复流程请参阅 [`install-arch/README.md`](install-arch/README.md)。

## 开发与测试

在仓库根目录执行：

```bash
cd install-arch
bash -n install-arch.sh test/*.sh
shellcheck install-arch.sh test/*.sh
./test/unit_tests.sh
```

循环设备集成测试需要 root 权限，并且只能在可丢弃的测试环境中运行：

```bash
sudo ./test/integration_test.sh
```

## 上游仓库

本项目上游仓库：[sneivandt/install-arch](https://github.com/sneivandt/install-arch)。
