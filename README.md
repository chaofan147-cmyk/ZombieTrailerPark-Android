# 建筑工大战僵尸 — Android v0.7 ARM64（原版 SWF + Ruffle）

这版把上一版工作流里最关键的三个问题修掉了：

1. **固定 Ruffle Android 版本 `0.260715`**，避免以后直接拉 `main` 导致源码接口变化而构建失败。
2. **显式安装 Rust 的 `aarch64-linux-android` target**，避免 GitHub Actions 中出现 `can't find crate for core`。
3. **按 Ruffle 官方 CI 的方式先编译 native library，再打 Android APK**；上一版只跑 `assembleRelease`，而 Ruffle 在 GitHub Actions 中会关闭 Gradle 内部的 Cargo 构建，因此单独跑 Gradle 不够。

另外，本版只构建 **ARM64 (`arm64-v8a`)**，适合绝大多数现代 Android 手机，也能明显缩短 GitHub Actions 构建时间和 APK 体积。

## 架构

```text
zombie-trailer-park.swf
        ↓
Ruffle Android 0.260715
        ↓
ARM64 native library
        ↓
Android APK
```

游戏本身仍然是**原版 SWF**，不是用 Godot 重写的简化版；原版 ActionScript、Box2D、动画、音效、关卡数据等由 Ruffle 执行。

## 构建

1. 把整个目录上传到一个 GitHub 仓库。
2. 打开 GitHub → **Actions**。
3. 选择 **Build Zombie Trailer Park APK (Ruffle ARM64)**。
4. 点击 **Run workflow**。
5. 构建完成后下载 Artifact：
   `ZombieTrailerPark-Ruffle-Android-ARM64`
6. 里面的 `ZombieTrailerPark-Ruffle-ARM64.apk` 即可安装到 ARM64 Android 手机。

同时会生成 `SHA256SUMS.txt` 用于校验 APK。

## 原版文件校验

工程中的 SWF 是此前提取的原始文件：

- 文件：`original/zombie-trailer-park.swf`
- 大小：4,657,139 bytes
- SHA-256：`8c518cfe4b0077bff3f1ed8f4c67627b63c4c5a8df6c3044f6ab019b234595a1`

## 兼容性说明

Ruffle 官方 Android 项目目前提供 ARM64、ARM32、x86_64、x86 等架构；官方文档也明确指出现代 Android 设备通常应使用 ARM64 版本。Ruffle 对 AS1/AS2/AS3 已有较好的支持，但项目仍在持续完善，因此某些旧 Flash 游戏可能出现渲染、输入或音频兼容性问题。

如果 APK 能启动但游戏出现：

- 黑屏 / 白屏
- 按钮无法点击
- 画面比例异常
- 音效或音乐异常
- 某个关卡逻辑异常

下一步就不是重新做游戏，而是针对这个 SWF 的 Ruffle 兼容性继续定位。

## 版权

原版游戏程序和素材可能受到版权保护。此工程用于个人研究、学习和本地移植测试；公开分发 APK 前请自行确认授权情况。
