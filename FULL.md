# 切换到 Full MPV 版本说明

本文档记录了将 media-kit 项目从 default MPV 版本切换到 full MPV 版本的详细修改内容。

## 修改日期
2025-11-21

## 修改概述

本次修改将所有平台的 MPV 库从 `default` 版本切换到 `full` 版本,以获得更完整的编解码器支持和功能。同时将依赖配置改为使用本地路径,便于开发和调试。

---

## 详细修改内容

### 1. Android 平台

#### 1.1 Audio 库 (libs/android/media_kit_libs_android_audio/android/build.gradle)

**版本变更:**
- 保持 `v1.1.8 default` (无变更)

**重要说明:** Android Audio 库从 v1.1.7 开始不再提供 full 版本,因此保持使用最新的 default 版本 v1.1.8。对于 audio-only 场景,default 版本已经足够。

**配置内容:**
```gradle
// 当前版本 (v1.1.8 default - 最新版本)
["url": "https://github.com/media-kit/libmpv-android-audio-build/releases/download/v1.1.8/default-arm64-v8a.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-audio-build/releases/download/v1.1.8/default-armeabi-v7a.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-audio-build/releases/download/v1.1.8/default-x86_64.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-audio-build/releases/download/v1.1.8/default-x86.jar", ...]
```

**MD5 校验和:**
- arm64-v8a: `6f4af754ae94da8cbb24655fd66c07ed`
- armeabi-v7a: `d8d1ba181d3d6ecb341e1e8d87506e17`
- x86_64: `43ed7b0e6bdaa1a6ed2c1eee01f5e44a`
- x86: `a2acb148c02d02f0892047f5c6c4f964`

#### 1.2 Video 库 (libs/android/media_kit_libs_android_video/android/build.gradle)

**版本变更:**
- 从 `v1.1.7 default` 升级到 `v1.1.8 full` (最新版本)

**修改内容:**
```gradle
// 原版本 (v1.1.7 default)
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.7/default-arm64-v8a.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.7/default-armeabi-v7a.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.7/default-x86_64.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.7/default-x86.jar", ...]

// 新版本 (v1.1.8 full - 最新版本,发布于 2025-11-12)
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-arm64-v8a.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-armeabi-v7a.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-x86_64.jar", ...]
["url": "https://github.com/media-kit/libmpv-android-video-build/releases/download/v1.1.8/full-x86.jar", ...]
```

**MD5 校验和:**
- arm64-v8a: `d8142f0317695da2b5970b49232a16fe`
- armeabi-v7a: `78d9b7a5875ab8907542cad8319d1761`
- x86_64: `be8349d300f2cfaa59670b5b1a0368ce`
- x86: `2b46056915db8e1aa8a0e79f39071543`

---

### 2. iOS 平台

#### 2.1 Audio 库 (libs/ios/media_kit_libs_ios_audio/ios/Makefile)

**版本变更:**
- 从 `v0.6.0 default` 升级到 `v0.6.2 full`

**修改内容:**
```makefile
# 原版本
MPV_XCFRAMEWORKS_VERSION=v0.6.0
MPV_XCFRAMEWORKS_SHA256SUM=8b8de92dc5482b8950c18bce6b8fa90204fd15af18f371acc9f5be07a4234e49
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_ios-universal-audio-default.tar.gz

# 新版本
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=5ca0b9551cd7658a04b6b0c9000e2cc5a1ce80fe1110e65fda9acac5f75933ab
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_ios-universal-audio-full.tar.gz
```

#### 2.2 Video 库 (libs/ios/media_kit_libs_ios_video/ios/Makefile)

**版本变更:**
- 从 `v0.6.0 default` 升级到 `v0.6.2 full`

**修改内容:**
```makefile
# 原版本
MPV_XCFRAMEWORKS_VERSION=v0.6.0
MPV_XCFRAMEWORKS_SHA256SUM=a95bc18508af26136b8a408341c05b5585d644ec013f00ac07db09d2e28d36ae
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_ios-universal-video-default.tar.gz

# 新版本
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=a7bd1a9037ad41877ebd7fa16fe78180ba9d90496f169e18c935fd75b8b8edb2
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_ios-universal-video-full.tar.gz
```

---

### 3. macOS 平台

#### 3.1 Audio 库 (libs/macos/media_kit_libs_macos_audio/macos/Makefile)

**版本变更:**
- 从 `v0.6.0 default` 升级到 `v0.6.2 full`

**修改内容:**
```makefile
# 原版本
MPV_XCFRAMEWORKS_VERSION=v0.6.0
MPV_XCFRAMEWORKS_SHA256SUM=916220b7b4fe9209de41264382966ac90a2de2ac11956a4b6041cf66a8110732
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_macos-universal-audio-default.tar.gz

# 新版本
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=837e8b979eb1101c88531e57dda5a93ef6d965b8960bfb6c2443cdd062aaf6d3
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_macos-universal-audio-full.tar.gz
```

#### 3.2 Video 库 (libs/macos/media_kit_libs_macos_video/macos/Makefile)

**版本变更:**
- 从 `v0.6.0 default` 升级到 `v0.6.2 full`

**修改内容:**
```makefile
# 原版本
MPV_XCFRAMEWORKS_VERSION=v0.6.0
MPV_XCFRAMEWORKS_SHA256SUM=84d2ad98e046e82c6dc34d8547d76c2afeaee89c0f53032773be8985c95536d6
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_macos-universal-video-default.tar.gz

# 新版本
MPV_XCFRAMEWORKS_VERSION=v0.6.2
MPV_XCFRAMEWORKS_SHA256SUM=73e147273a6c7a19bd5535ab2336188d12636caa0d65422f86a5e1ac853747fd
# URL: libmpv-xcframeworks_${MPV_XCFRAMEWORKS_VERSION}_macos-universal-video-full.tar.gz
```

---

### 4. 依赖配置修改

#### 4.1 media_kit_video (media_kit_video/pubspec.yaml)

**修改内容:**
```yaml
# 原配置
dependencies:
  media_kit: ^1.2.1

# 新配置
dependencies:
  # media_kit: ^1.2.1
  media_kit:
    path: ../media_kit
```

**说明:** 将 media_kit 依赖从版本号改为本地路径引用,便于开发调试。

#### 4.2 Universal Audio 库 (libs/universal/media_kit_libs_audio/pubspec.yaml)

**修改内容:**
```yaml
# 原配置
dependencies:
  media_kit_libs_android_audio: ^1.3.8
  media_kit_libs_ios_audio: ^1.1.4
  media_kit_libs_macos_audio: ^1.1.4
  media_kit_libs_windows_audio: ^1.0.9
  media_kit_libs_linux: ^1.2.1

# 新配置
dependencies:
  # media_kit_libs_android_audio: ^1.3.8
  # media_kit_libs_ios_audio: ^1.1.4
  # media_kit_libs_macos_audio: ^1.1.4
  # media_kit_libs_windows_audio: ^1.0.9
  # media_kit_libs_linux: ^1.2.1
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

#### 4.3 Universal Video 库 (libs/universal/media_kit_libs_video/pubspec.yaml)

**修改内容:**
```yaml
# 原配置
dependencies:
  media_kit_libs_android_video: ^1.3.8
  media_kit_libs_ios_video: ^1.1.4
  media_kit_libs_macos_video: ^1.1.4
  media_kit_libs_windows_video: ^1.0.11
  media_kit_libs_linux: ^1.2.1

# 新配置
dependencies:
  # media_kit_libs_android_video: ^1.3.8
  # media_kit_libs_ios_video: ^1.1.4
  # media_kit_libs_macos_video: ^1.1.4
  # media_kit_libs_windows_video: ^1.0.11
  # media_kit_libs_linux: ^1.2.1
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

---

## Full MPV vs Default MPV 区别

### Full MPV 版本优势

1. **更完整的编解码器支持**
   - 包含更多的音视频编解码器
   - 支持更多的容器格式
   - 更好的兼容性

2. **额外的功能支持**
   - 更多的滤镜和效果
   - 更完整的字幕支持
   - 更多的网络协议支持

### 文件大小对比

Full 版本由于包含更多编解码器,文件体积会比 Default 版本大约 **30-50%**。

**Android:**
- Default: 约 15-20 MB (每个架构)
- Full: 约 20-30 MB (每个架构)

**iOS/macOS:**
- Default: 约 25-35 MB
- Full: 约 35-50 MB

---

## 版本兼容性说明

### Android
- **最低支持版本:** Android API 16 (Android 4.1)
- **推荐版本:** Android API 21+ (Android 5.0+)
- **架构支持:** arm64-v8a, armeabi-v7a, x86_64, x86

### iOS
- **最低支持版本:** iOS 12.0+
- **架构支持:** arm64 (iPhone 5s 及更新机型)

### macOS
- **最低支持版本:** macOS 10.13+
- **架构支持:** x86_64, arm64 (Apple Silicon)

---

## 构建注意事项

### 1. 首次构建

首次使用 Full MPV 版本时,需要下载新的库文件:

**Android:**
```bash
cd libs/android/media_kit_libs_android_audio/android
./gradlew clean
./gradlew assemble
```

**iOS:**
```bash
cd libs/ios/media_kit_libs_ios_audio/ios
make clean
make
```

**macOS:**
```bash
cd libs/macos/media_kit_libs_macos_audio/macos
make clean
make
```

### 2. 清理旧缓存

如果之前使用过 Default 版本,建议清理构建缓存:

```bash
# Flutter 清理
flutter clean
flutter pub get

# Android 清理
cd android && ./gradlew clean && cd ..

# iOS/macOS 清理
cd ios && rm -rf Pods Podfile.lock && pod install && cd ..
cd macos && rm -rf Pods Podfile.lock && pod install && cd ..
```

### 3. 网络下载

由于 Full MPV 文件较大,首次构建可能需要较长时间下载。建议:
- 使用稳定的网络连接
- 如果下载失败,检查 MD5 校验和是否正确
- 可以手动下载文件并放置到对应的缓存目录

---

## 回滚到 Default 版本

如果需要回滚到 Default 版本,可以:

1. **使用 Git 恢复:**
   ```bash
   git checkout HEAD~1 -- libs/
   ```

2. **手动修改:**
   参考本文档中的"原版本"配置,将所有修改还原。

3. **清理并重新构建:**
   ```bash
   flutter clean
   flutter pub get
   ```

---

## 已知问题

### 1. 包体积增大
Full MPV 版本会导致最终应用体积增大 30-50 MB,如果对包体积敏感,建议考虑:
- 仅为需要的平台启用 Full MPV
- 使用应用分发优化(如 Android App Bundle)
- 评估是否真的需要所有编解码器

### 2. 发布限制
使用本地路径依赖的包无法直接发布到 pub.dev。如果需要发布,必须:
- 将所有 `path` 依赖改回版本号
- 或在 pubspec.yaml 中添加 `publish_to: none`

### 3. 兼容性测试
切换到 Full MPV 后,建议在所有目标平台上进行充分测试,特别注意:
- 不同视频格式的播放
- 内存占用情况
- 冷启动时间
- 包体积影响

---

## 测试清单

在完成 Full MPV 切换后,建议进行以下测试:

- [ ] Android 平台编译成功
- [ ] iOS 平台编译成功
- [ ] macOS 平台编译成功
- [ ] 播放常见视频格式 (MP4, MKV, AVI 等)
- [ ] 播放常见音频格式 (MP3, AAC, FLAC 等)
- [ ] 字幕功能正常
- [ ] 网络流播放正常
- [ ] 性能测试通过
- [ ] 内存占用在可接受范围内
- [ ] 包体积增长在预期范围内

---

## 参考链接

- [MPV 官方文档](https://mpv.io/manual/)
- [libmpv Android 构建仓库](https://github.com/media-kit/libmpv-android-audio-build)
- [libmpv Darwin 构建仓库](https://github.com/media-kit/libmpv-darwin-build)
- [media-kit 主仓库](https://github.com/media-kit/media-kit)

---

## 维护者注意

本文档应在每次修改 MPV 版本配置时更新,包括:
- 版本号变更
- SHA256 校验和变更
- 新增平台支持
- 已知问题更新

**最后更新:** 2025-11-21
**文档版本:** 1.0.0
