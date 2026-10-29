# 页屿 PageIsle · 固件 / 刷机站 / 源码 / 指南

这是页屿 PageIsle（RDP-G01-W · ESP32-S3 · 16 MB flash）的**唯一仓库**，同时承担：

- **刷机站**：本仓库根目录就是 GitHub Pages 站点，浏览器打开
  <https://aleksandrov-eink.github.io/pageisle/> 可直接用 Web Serial 刷机（Chrome / Edge 89+）。
- **正式版下载**：TF 卡升级镜像、完整交接包、全部源码与中文指南都在 `downloads/`。
- **版本与校验**：以本仓库 `firmware/SHA256SUMS.txt` 与本文件记录的 sha256 为准。

## 当前正式版

- **页屿系统 1.15.26**（2026-10-29），上游 kiikoread 0.3.3-rc99 应用层定制
- 应用镜像：`4,076,160` 字节，ota_0 余量 `118,144` 字节
- sha256：`812b00efad377560d35ea51bd453f993c09e0751d0e94bd5aa3edc73b9055db3`
- 固件文件：`firmware/Pico-update.bin`（TF 卡升级）；`firmware/firmware.bin` 等为 USB 整包分段
- 升级指南：`downloads/改版日志与刷机指南.md`；功能与设置：`downloads/使用说明.txt`

## 仓库目录

```
index.html                刷机站首页（纯静态，esp-web-tools 已内置）
firmware/                 固件与 ESP Web Tools manifest、SHA256SUMS
downloads/                源码包、完整交接包、TF 卡镜像、中文指南
docs/                     使用说明、刷机指南、变更清单（站点内副本）
assets/ promo 图           站点宣传图
web-flasher/              内置的 esp-web-tools 静态资源（不依赖 CDN）
beta/                     隐藏内测通道（5 次点击标志可见）
```

## TF 卡本地升级（推荐）

1. 把 `downloads/Pico-update-1.15.26.bin` 复制到 TF 卡根目录（FAT32）。
2. 设备开机后在「设置 → 升级和恢复」里选择本地升级/从 TF 卡升级（不同版本菜单名略有差异）。
3. 升级只写应用镜像分区，书、设置与阅读记录保留；升级完可在「更新说明」看到本版条目。

## 源码构建

- 工具链：**ESP-IDF v6.1**，目标 `esp32s3`。
- 解压 `downloads/PageIsle-1.15.26-source.zip`，在工程根目录执行：
  ```
  idf.py set-target esp32s3
  idf.py build
  ```
- 产物：`build/Read_Pico.bin`（应用镜像）、`build/bootloader/bootloader.bin`、`build/partition_table/partitions.bin`。
- 分区表、引导程序与 OTA 元数据布局与上游 kiikoread 官方一致，未改动。

## 版本历史与说明

- 从 1.15.8 到 1.15.26 的主要变化：深色白闪修复（1k LUT 保持深色等灰对 + 满幅压黑补扫）、
  偶发整屏缺字兜底（字形失败不缓存/双向回退/内部 RAM 兜底 + TF 卡诊断日志）、坚果云联网前内存
  门控与阅读进度快照上限、反色到页黑闪整屏清理、主页滑动/设置界面/阅读工具栏菜单/横屏阅读自动
  去残影、小游戏进页全刷与分数叠影修复；1.15.18–1.15.26 又完成：深睡唤醒小锁界面、反黑算法恢复
  （逐页压黑+整屏 GC16 复位）、浅色淡墨/残影治理（活动期清理、低频整屏 GC16、每页白底平衡复位）、
  阅读页插图边框、书架删除/最近阅读修复、WiFi 传书/蓝牙翻页器修复、面板文字抗锯齿修复。
- 每一版的改动与真机注意事项见 `downloads/改版日志与刷机指南.md`（含 1.12.0 起的系统版本线）。
- 相对上游 rc99 的逐项改动见 `downloads/变更清单-从原始版到最终版.md`。

## 许可与致谢

- 固件基于 **kiikoread 0.3.3-rc99**（及其 rc100 版本的部分功能）做应用层定制，按其原有许可发布；
  仓库内保留上游许可与第三方声明（`THIRD_PARTY_NOTICES.md`）。
- 刷机能力来自 [esp-web-tools](https://github.com/esphome/esp-web-tools) 与 esptool-js
  （© Espressif Systems，Apache-2.0），已内置在本仓库。
- 图标来自 Lucide（ISC）。

> 本仓库的固件为个人定制版本，刷机有风险；请确认设备型号并优先使用 TF 卡升级。所有校验值都是本仓库构建产物，
> 没有真机测试记录被当作已验证结论。
