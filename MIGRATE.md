# 上游仓库更新迁移指南

本文档记录了如何在上游仓库更新后,重新应用本地的 Full MPV 配置和自定义修改。

## 迁移场景

当需要从上游 media-kit 仓库拉取最新代码时,需要重新应用以下本地修改:

1. **Full MPV 配置** - 将所有平台切换到 full MPV 版本
2. **自定义功能** - 保留本地开发的功能特性

---

## 快速迁移步骤

### 1. 拉取上游更新并解决冲突

```bash
# 拉取上游最新代码
git pull origin main

# 如果有冲突,解决冲突:
# - 对于 Full MPV 相关文件: 接受远程版本,稍后重新应用
# - 对于自定义功能文件: 保留本地修改
```

### 2. 重新应用 Full MPV 配置

参考 [FULL.md](FULL.md) 文档,按平台应用以下修改:

#### Android 平台

**Video 库** (libs/android/media_kit_libs_android_video/android/build.gradle):
```gradle
// 将 default 改为 full,使用最新版本
def filesToDownload = [
    ["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-arm64-v8a.jar", "md5": "d8142f0317695da2b5970b49232a16fe", ...],
    ["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-armeabi-v7a.jar", "md5": "78d9b7a5875ab8907542cad8319d1761", ...],
    ["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-x86_64.jar", "md5": "be8349d300f2cfaa59670b5b1a0368ce", ...],
    ["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-x86.jar", "md5": "2b46056915db8e1aa8a0e79f39071543", ...]
]
```

**Audio 库** (libs/android/media_kit_libs_android_audio/android/build.gradle):
```gradle
// Audio 保持 default 版本(从 v1.1.7 开始不再提供 full)
// 无需修改,使用上游的最新 default 版本
```

#### iOS 平台

**Audio 库** (libs/ios/media_kit_libs_ios_audio/ios/Makefile):
```makefile
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=5ca0b9551cd7658a04b6b0c9000e2cc5a1ce80fe1110e65fda9acac5f75933ab

# URL 改为 full
libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_ios-universal-audio-full.tar.gz
```

**Video 库** (libs/ios/media_kit_libs_ios_video/ios/Makefile):
```makefile
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=a7bd1a9037ad41877ebd7fa16fe78180ba9d90496f169e18c935fd75b8b8edb2

# URL 改为 full
libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_ios-universal-video-full.tar.gz
```

#### macOS 平台

**Audio 库** (libs/macos/media_kit_libs_macos_audio/macos/Makefile):
```makefile
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=837e8b979eb1101c88531e57dda5a93ef6d965b8960bfb6c2443cdd062aaf6d3

# URL 改为 full
libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_macos-universal-audio-full.tar.gz
```

**Video 库** (libs/macos/media_kit_libs_macos_video/macos/Makefile):
```makefile
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=73e147273a6c7a19bd5535ab2336188d12636caa0d65422f86a5e1ac853747fd

# URL 改为 full
libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_macos-universal-video-full.tar.gz
```

#### 依赖配置

**media_kit_video/pubspec.yaml**:
```yaml
dependencies:
  # media_kit: ^1.2.1
  media_kit:
    path: ../media_kit
```

**libs/universal/media_kit_libs_audio/pubspec.yaml**:
```yaml
dependencies:
  media_kit_libs_android_audio:
    path: ../../android/media_kit_libs_android_audio
  media_kit_libs_ios_audio:
    path: ../../ios/media_kit_libs_ios_audio
  media_kit_libs_linux_audio:
    path: ../../linux/media_kit_libs_linux_audio
  media_kit_libs_macos_audio:
    path: ../../macos/media_kit_libs_macos_audio
  media_kit_libs_windows_audio:
    path: ../../windows/media_kit_libs_windows_audio
```

**libs/universal/media_kit_libs_video/pubspec.yaml**:
```yaml
dependencies:
  media_kit_libs_android_video:
    path: ../../android/media_kit_libs_android_video
  media_kit_libs_ios_video:
    path: ../../ios/media_kit_libs_ios_video
  media_kit_libs_linux:
    path: ../../linux/media_kit_libs_linux
  media_kit_libs_macos_video:
    path: ../../macos/media_kit_libs_macos_video
  media_kit_libs_windows_video:
    path: ../../windows/media_kit_libs_windows_video
```

### 3. 保留本地自定义功能

#### 提交 33f5bfa1: 视频控制器背景渐变开关

**文件**: `media_kit_video/lib/media_kit_video_controls/src/controls/material_desktop.dart`

**位置**: MaterialDesktopVideoControlsThemeData 类

**修改内容**:

```dart
// 1. 添加属性 (约第76行)
/// Whether to draw the default top/bottom background gradients behind controls.
final bool showBackdropGradient;

// 2. 构造函数参数 (约第194行)
MaterialDesktopVideoControlsThemeData({
  // ... 其他参数
  this.showBackdropGradient = true,
  // ... 其他参数
})

// 3. copyWith 方法参数 (约第246行)
MaterialDesktopVideoControlsThemeData copyWith({
  // ... 其他参数
  bool? showBackdropGradient,
  // ... 其他参数
}) {
  return MaterialDesktopVideoControlsThemeData(
    // ... 其他参数
    showBackdropGradient: showBackdropGradient ?? this.showBackdropGradient,
    // ... 其他参数
  );
}

// 4. 使用渐变开关 (约第687行)
// Top gradient
if (_theme(context).showBackdropGradient &&
    _theme(context).topButtonBar.isNotEmpty)
  Container(
    decoration: const BoxDecoration(
      gradient: LinearGradient(...),
    ),
  ),

// Bottom gradient (约第706行)
if (_theme(context).showBackdropGradient &&
    _theme(context).bottomButtonBar.isNotEmpty)
  Container(
    decoration: const BoxDecoration(
      gradient: LinearGradient(...),
    ),
  ),
```

**完整差异**:
```bash
# 查看完整差异
git show 33f5bfa1c5c3d026fad826641a11ed2b33cd32e7
```

**Cherry-pick 应用**:
```bash
# 如果该提交在 reflog 中仍然存在
git cherry-pick 33f5bfa1c5c3d026fad826641a11ed2b33cd32e7

# 如果不存在,手动应用上述修改
```

---

## 自动化迁移脚本

可以创建一个脚本来自动应用这些修改:

```bash
#!/bin/bash
# migrate-full-mpv.sh

echo "=== 开始应用 Full MPV 配置 ==="

# 1. Android Video - 切换到 full
echo "1. 更新 Android Video 库..."
sed -i '' 's|v1.1.[0-9]/default-|v1.1.8/full-|g' libs/android/media_kit_libs_android_video/android/build.gradle
# 更新 MD5 (需要根据实际版本调整)

# 2. iOS/macOS - 更新版本和 URL
echo "2. 更新 iOS Audio 库..."
sed -i '' 's|MPV_XCFRAMEWORKS_VERSION=v0.6.[0-9]|MPV_XCFRAMEWORKS_VERSION=v0.6.2|' libs/ios/media_kit_libs_ios_audio/ios/Makefile
sed -i '' 's|audio-default|audio-full|' libs/ios/media_kit_libs_ios_audio/ios/Makefile

echo "3. 更新 iOS Video 库..."
sed -i '' 's|MPV_XCFRAMEWORKS_VERSION=v0.6.[0-9]|MPV_XCFRAMEWORKS_VERSION=v0.6.2|' libs/ios/media_kit_libs_ios_video/ios/Makefile
sed -i '' 's|video-default|video-full|' libs/ios/media_kit_libs_ios_video/ios/Makefile

echo "4. 更新 macOS Audio 库..."
sed -i '' 's|MPV_XCFRAMEWORKS_VERSION=v0.6.[0-9]|MPV_XCFRAMEWORKS_VERSION=v0.6.2|' libs/macos/media_kit_libs_macos_audio/macos/Makefile
sed -i '' 's|audio-default|audio-full|' libs/macos/media_kit_libs_macos_audio/macos/Makefile

echo "5. 更新 macOS Video 库..."
sed -i '' 's|MPV_XCFRAMEWORKS_VERSION=v0.6.[0-9]|MPV_XCFRAMEWORKS_VERSION=v0.6.2|' libs/macos/media_kit_libs_macos_video/macos/Makefile
sed -i '' 's|video-default|video-full|' libs/macos/media_kit_libs_macos_video/macos/Makefile

# 3. 尝试 cherry-pick 自定义功能
echo "6. 应用自定义功能..."
git cherry-pick 33f5bfa1c5c3d026fad826641a11ed2b33cd32e7 || {
    echo "Cherry-pick 失败,需要手动应用"
    echo "参考 MIGRATE.md 中的步骤 3"
}

echo "=== 完成 ==="
echo "请检查修改并运行测试"
```

---

## 版本更新检查清单

在应用迁移后,检查以下内容:

### Android
- [ ] Video 库使用最新的 full 版本
- [ ] Audio 库使用最新的 default 版本
- [ ] MD5 校验和正确

### iOS
- [ ] Audio 库版本正确,URL 包含 `full`
- [ ] Video 库版本正确,URL 包含 `full`
- [ ] SHA256 校验和正确

### macOS
- [ ] Audio 库版本正确,URL 包含 `full`
- [ ] Video 库版本正确,URL 包含 `full`
- [ ] SHA256 校验和正确

### 依赖配置
- [ ] media_kit_video 使用本地路径
- [ ] universal libs 使用本地路径

### 自定义功能
- [ ] 背景渐变开关功能存在
- [ ] 其他本地功能正常工作

---

## 测试验证

应用迁移后,执行以下测试:

```bash
# 1. 清理构建缓存
flutter clean
rm -rf ios/Pods ios/Podfile.lock
rm -rf macos/Pods macos/Podfile.lock

# 2. 获取依赖
flutter pub get

# 3. Android 构建测试
cd libs/android/media_kit_libs_android_video/android
./gradlew clean
./gradlew assemble
cd -

# 4. iOS 构建测试
cd libs/ios/media_kit_libs_ios_video/ios
make clean
make
cd -

# 5. macOS 构建测试
cd libs/macos/media_kit_libs_macos_video/macos
make clean
make
cd -

# 6. Flutter 构建测试
flutter build apk --debug  # Android
flutter build ios --debug  # iOS
flutter build macos --debug  # macOS
```

---

## 常见问题

### Q1: MD5/SHA256 校验失败

**原因**: 版本号或校验和不匹配

**解决**:
1. 检查 FULL.md 中的最新版本号和校验和
2. 访问 GitHub releases 页面确认
3. 必要时重新下载并计算校验和

### Q2: 依赖路径无法解析

**原因**: pubspec.yaml 中的相对路径不正确

**解决**:
```bash
# 检查目录结构
ls -la libs/android/media_kit_libs_android_audio
ls -la libs/ios/media_kit_libs_ios_audio

# 确保路径正确
cd libs/universal/media_kit_libs_audio
flutter pub get
```

### Q3: Cherry-pick 冲突

**原因**: 上游代码结构变化

**解决**:
1. 查看冲突文件
2. 参考 MIGRATE.md 步骤 3 手动应用修改
3. 测试功能是否正常

---

## 参考文档

- [FULL.md](FULL.md) - Full MPV 配置详细说明
- [上游仓库](https://github.com/media-kit/media-kit) - media-kit 主仓库
- [Android Audio 构建](https://github.com/media-kit/libmpv-android-audio-build/releases)
- [Android Video 构建](https://github.com/media-kit/libmpv-android-video-build/releases)
- [Darwin 构建](https://github.com/media-kit/libmpv-darwin-build/releases)

---

## 维护日志

| 日期 | 操作 | 说明 |
|------|------|------|
| 2025-11-21 | 创建文档 | 初始版本,记录 Full MPV 迁移步骤 |
| 2025-11-21 | 添加自定义功能 | 记录背景渐变开关功能 (33f5bfa1) |

---

**最后更新**: 2025-11-21
**文档版本**: 1.0.0
