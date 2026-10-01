# 渡OS (DuOS v1.0) - Redmi Note 8 (ginkgo) 专属底包构建工程

本项目为 **渡OS (DuOS v1.0)** 针对 **Redmi Note 8 (`ginkgo` / 6GB RAM + 64GB ROM / Android 16 底包)** 的专用系统级固化构建工程。

通过 GitHub Actions 云端流水线构建，无需本地占用几百GB存储和编译资源，直接生成可由第三方 Recovery (如 OrangeFox / TWRP) 一键刷入的系统特权注入卡刷包。

---

## 一、 系统架构设计（底包特权固化）

渡OS与普通的“安装 APK”不同，它直接植入系统底层（`/system/priv-app/`）：

1. **渡桌面 (`DuZhuoMian`)**：
   - 固化至 `/system/priv-app/DuZhuoMian/`
   - 注册为系统默认桌面与 HOME 键接收者，开机首屏直接呈现，绝不调起原生 AOSP 桌面。
2. **渡步 (`DuStep`)**：
   - 固化至 `/system/priv-app/DuStep/`
   - 底层一步手势底栏与全局快捷调度，免用户手动去“无障碍”或“悬浮窗”授权，直接获得特权级穿透。
3. **渡写 (`DuXie`)**：
   - 固化至 `/system/priv-app/DuXie/`
   - 系统唯一输入法（`BIND_INPUT_METHOD`），开机默认激活。
   - **离线大模型固化**：将 228MB SenseVoice 模型 (`model.int8.onnx`) 写入系统只读目录 `/system/etc/duxie/bundled_model/`，零联网、防误删、开箱即用。
4. **渡口 (`DuKou`)**：
   - 固化至 `/system/priv-app/DuKou/`
   - 跨设备局域网自发现与文件摆渡中枢，系统级开机自启，不受系统 LMK 杀后台限制。
5. **渡札 (`DuZha`) & 渡链 (`DuLian`)**：
   - 本地卡片知识库与端侧向量检索节点，预装随底包交付。
6. **硬件不适配剔除**：
   - Redmi Note 8 缺乏硬件 S-Pen 手写笔支持，**已严格剔除「渡画」和「渡拍」**，避免空耗内存。

---

## 二、 物理剔除原生冗余 (Debloat)

刷入时，安装脚本将自动物理移除下列 AOSP 原生垃圾，释放内存与存储：
- 原生桌面：`Trebuchet`, `Launcher3`
- 原生输入法：`LatinIME`
- 原生媒体应用：`Eleven` (音乐), `Gallery2` (相册), `Jelly` (简陋浏览器)

---

## 三、 刷入教程（OrangeFox / TWRP）

1. 从 [Releases](https://github.com/yuxiaotao721/du-os-ginkgo/releases) 下载最新的 `DuOS-v1.0-Ginkgo-Core-Installer.zip`。
2. 手机关机，长按 `音量上 + 电源键` 进入 Recovery (推荐 OrangeFox)。
3. 将 ZIP 包传入手机（通过 OTG、TF卡或 `adb push DuOS-v1.0-Ginkgo-Core-Installer.zip /sdcard/`）。
4. 在 Recovery 中点击该 ZIP，滑动确认刷入。
5. 脚本运行约 15 秒完成固化注入，点击 **Reboot System** 重启手机。
6. 开机直接进入纯净渡OS系统！
