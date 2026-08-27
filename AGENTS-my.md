# RustDesk Guide 

1.win 版本以及 linux x86_64 版本 主要界面代码在flutter 文件夹下；重点分析、理解 flutter 下各界面功能


### Testing

### Platform-Specific Build Scripts
flutter 版本编译脚本是.github\workflows\flutter-nightly.yml

## Project Architecture

### Directory Structure
- **`src/`** - Main Rust application code
  - `src/ui/` - Legacy Sciter UI (deprecated, use Flutter instead)
  - `src/server/` - Audio/clipboard/input/video services and network connections
  - `src/client.rs` - Peer connection handling
  - `src/platform/` - Platform-specific code
- **`flutter/`** - Flutter UI code for desktop and mobile
- **`libs/`** - Core libraries
  - `libs/hbb_common/` - Video codec, config, network wrapper, protobuf, file transfer utilities
  - `libs/scrap/` - Screen capture functionality
  - `libs/enigo/` - Platform-specific keyboard/mouse control
  - `libs/clipboard/` - Cross-platform clipboard implementation

  -中文-------------
## 文件结构
- **[libs/hbb_common]: 视频编解码, 配置, tcp/udp 封装, protobuf, 文件传输相关文件系统操作函数, 以及一些其他实用函数
- **[libs/scrap]: 屏幕截取
- **[libs/enigo]: 平台相关的鼠标键盘输入
- **[libs/clipboard]: Windows、Linux、macOS 的文件复制和粘贴实现
- **[src/ui]: 过时的 Sciter UI（已弃用）
- **[src/server]: 被控端服务音频、剪切板、输入、视频服务、网络连接的实现
- **[src/client.rs]: 控制端
- **[src/rendezvous_mediator.rs]: 与[rustdesk-server]保持UDP通讯, 等待远程连接（通过打洞直连或者中继）
- **[src/platform]: 平台服务相关代码
- **[flutter]: 适用于桌面和移动设备的 Flutter 代码
- **[flutter/web/js]: Flutter Web版本中的Javascript代码

### 关键 组件
- **Remote Desktop Protocol**: Custom protocol implemented in `src/rendezvous_mediator.rs` for communicating with rustdesk-server
- **Screen Capture**: Platform-specific screen capture in `libs/scrap/`
- **Input Handling**: Cross-platform input simulation in `libs/enigo/`
- **Audio/Video Services**: Real-time audio/video streaming in `src/server/`
- **File Transfer**: Secure file transfer implementation in `libs/hbb_common/`

### UI 架构
- **Legacy UI**: Sciter-based (deprecated) - files in `src/ui/`
- **Modern UI**: Flutter-based - files in `flutter/`
  - Desktop: `flutter/lib/desktop/`
  - Mobile: `flutter/lib/mobile/`
  - Shared: `flutter/lib/common/` and `flutter/lib/models/`

## Important Build Notes

### Dependencies
- Requires vcpkg for C++ dependencies: `libvpx`, `libyuv`, `opus`, `aom`
- Set `VCPKG_ROOT` environment variable
- Download appropriate Sciter library for legacy UI support

### Ignore Patterns
When working with files, ignore these directories:
- `target/` - Rust build artifacts
- `flutter/build/` - Flutter build output
- `flutter/.dart_tool/` - Flutter tooling files

### 跨平台 考虑
- Windows builds require additional DLLs and virtual display drivers
- macOS builds need proper signing and notarization for distribution
- Linux builds support multiple package formats (deb, rpm, AppImage)
- Mobile builds require platform-specific toolchains (Android SDK, Xcode)

### Feature Flags
- `hwcodec` - Hardware video encoding/decoding
- `vram` - VRAM optimization (Windows only)
- `flutter` - Enable Flutter UI
- `unix-file-copy-paste` - Unix file clipboard support
- `screencapturekit` - macOS ScreenCaptureKit (macOS only)

### Config
All configurations or options are under `libs/hbb_common/src/config.rs` file, 4 types:
- Settings
- Local
- Display
- Built-in

## Rust Rules


## 编辑原则

- Do not introduce formatting-only changes.
- Do not run repository-wide formatters or reflow unrelated code unless the
  user explicitly asks for formatting.
- Keep diffs limited to semantic changes required for the task.
