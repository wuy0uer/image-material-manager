# Electron 媒体组件的源码与替换

图片素材库 V1（1.0.0）附带 Electron 38.8.6，其媒体支持包括动态加载的 `ffmpeg.dll`。本软件自有部分的销售限制不限制 FFmpeg 及其他第三方许可证授予的权利。

## 版本

- Electron：38.8.6。
- Chromium：140.0.7339.249，取自该 Electron 标签的 `DEPS`。
- FFmpeg Chromium 分支：`d2d06b12c22d27af58114e779270521074ff1f85`，取自上述 Chromium 的 `DEPS`。
- `ffmpeg.dll` 直接随官方 Electron 二进制分发，未作本地修改。

同版本 GitHub Release 的独立附件《第三方组件源码.zip》中的 `FFmpeg-Chromium.source.tar.gz` 是该固定提交的源码，包含其 Chromium 配置和构建文件；`Electron-38.8.6.source.tar.gz` 包含 Electron 对应版本的构建脚本、配置和补丁。另附两份 `DEPS`，保留版本对应关系。获取指引见“许可文件/第三方源码获取说明.txt”。具体许可证见 `LGPL-2.1.txt`、源码中的 `COPYING*` 及程序“许可文件”目录中的 `LICENSES.chromium.html`。

## 构建与替换

按 Electron 对应版本源码的 `docs/development/build-instructions-gn.md` 以及 Windows 构建说明准备 depot_tools、GN、Ninja、Windows SDK 和编译环境，通过该版本的 `DEPS` 获取固定依赖并应用对应补丁，构建 Windows x64 的 `ffmpeg` 目标。FFmpeg 源码中的 `BUILD.gn`、`chromium/config` 和 `chromium/scripts` 是该分支的配置、构建参考。

构建工具链及完整 Chromium 构建环境需要按官方说明自行准备；本次仅核对官方分发文件与版本关系，没有在本机重建 Electron/FFmpeg，也不声称更换编译器后输出字节一致。

退出软件后，可备份并替换软件根目录的 `ffmpeg.dll`，使用保持相同导出接口和 ABI 的修改版本，再重新启动。软件不要求作者密钥、签名或网络许可来替换该文件。接口不相容可能导致媒体功能或启动异常，操作前应保留原件。修改第三方组件不需要清除个人 `data`、模型或图库。

允许依其许可修改及替换有关组件，并为调试这些修改进行必要的逆向工程。本软件自有部分的许可不得被解释为限制这些权利。

来源：

- https://github.com/electron/electron/tree/v38.8.6
- https://github.com/chromium/chromium/blob/140.0.7339.249/DEPS
- https://chromium.googlesource.com/chromium/third_party/ffmpeg/+/d2d06b12c22d27af58114e779270521074ff1f85
