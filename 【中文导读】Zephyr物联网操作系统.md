# Zephyr（物联网操作系统）学习导读

> 目录：`01-RTOS操作系统/zephyr-Zephyr物联网操作系统`
> 来源：https://github.com/zephyrproject-rtos/zephyr （压缩包：zephyr-main.zip）

## 这是什么

Zephyr 是 Linux 基金会托管的现代化物联网 RTOS，支持 800+ 开发板（STM32、nRF5x/53/91、ESP32、RP2040 全覆盖），采用**内核与硬件完全解耦的 devicetree 描述 + Kconfig 配置**体系，被视为"嵌入式界的 Linux"。 Nordic（nRF Connect SDK）、各大厂新品纷纷以 Zephyr 为基础，是当前就业市场最值得学的现代嵌入式框架。

⚠️ 注意：本压缩包只是 Zephyr 本体（内核+驱动+子系统）。完整开发需要 west 工具拉取 modules（hal、mbedtls、littlefs 等都在外部模块仓库里）。

## 目录导读

| 目录 | 内容 |
| --- | --- |
| `kernel/` | 内核：调度器、线程、信号量、消息队列、堆管理 |
| `drivers/` | 全部设备驱动（serial、gpio、spi、i2c、sensor、display、net...） |
| `dts/` 和 `boards/` | devicetree 绑定和板级描述——**Zephyr 灵魂所在** |
| `subsys/` | 子系统：bluetooth、net、logging、shell、fs、usb、settings... |
| `samples/` | 数百个官方示例，学习入口（`samples/hello_world`、`samples/bluetooth/...`） |
| `tests/` | ztest 单元测试框架与内核测试 |
| `arch/` | CPU 架构相关代码（arm64、riscv、x86、posix...） |
| `scripts/` | west、devicetree 生成器、Kconfig 处理等构建机器 |

## 学习路径（建议）

1. 先读官方文档 "Zephyr Getting Started"（`doc/develop/getting_started/index.rst`），在 Linux 上装 west。
2. `west build -b nucleo_f103re samples/basic/blinky` 体验一次完整构建，理解 build 目录里生成的 `devicetree_generated.h` 和 `autoconf.h`。
3. 按 samples 顺序学：blinky(GPIO) → blink_led(PWM) → hello_world(logging) → shell → sensor → bluetooth/peripheral_ht。
4. 读 `kernel/` 源码之前，先读官方 "Kernel services" 文档章节，再对照 `kernel/sched.c`（调度器单文件核心）。

## 学习要点

1. **devicetree overlay 思维**：板级差异全部用 dts 描述，应用代码用 `DEVICE_DT_GET()` 拿设备——告别手写引脚宏。
2. **Kconfig 与菜单配置**：类似 Linux menuconfig，`prj.conf` 只写增量配置。
3. **线程与调度**：合作式/抢占式线程、meta-IRQ；`K_NO_WAIT/K_FOREVER` 超时语义贯穿所有 API。
4. **蓝牙代码是教科书级**：`subsys/bluetooth/` + `samples/bluetooth/` 是学 BLE 协议栈实现的最佳开源材料之一。

## 建议购买/准备

- **零成本路线**：QEMU（`west build -b qemu_x86`）或 native_sim 在电脑上直接跑，什么都不用买。
- **推荐入手硬件**：
  - 树莓派 Pico / Pico W（¥20-40，rp2040/rp2350 支持很好）
  - nRF52840 DK（¥200 左右，学 BLE 必备，Nordic 官方全套 Zephyr 例程）
  - STM32 Nucleo-F103RE / Nucleo-G071RB（¥100-150）
- USB 线（数据线）、杜邦线若干。

## 学习目标（自测）

- [ ] 能独立完成 west build/flash/debug 三件套
- [ ] 能为一块新板写最小 dts 并点亮 LED
- [ ] 理解 overlay 在应用里的作用
- [ ] 跑通一个 BLE peripheral 示例并用手机连接
