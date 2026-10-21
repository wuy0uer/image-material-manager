# HEIC 组件源码与替换说明

图片素材库 V1（1.0.0）使用独立的 libheif-js 解码组件。软件自有部分的销售限制不适用于该组件。允许依其 LGPL 许可修改、再分发、替换和为调试修改而进行必要的逆向工程。

## 对应关系

- libheif-js 1.23.2：`6ca00b818c0ff51cb2a5c75b9ce97d708083335a`。
- libheif-emscripten v1.23.2 tree：`dd5a19b878149437a5a427f6e1afa8bac624a714`。
- libheif 子模块：`ac1cb05c39008f01525c991ff8b88f84ddf70fd2`。
- libde265：1.0.15，按该版 `build-emscripten.sh` 的默认配置构建。
- 上游 CI：Ubuntu 22.04，Emscripten 3.1.61，Node.js 24，TypeScript 5。
- 软件内文件：`resources/app/heic-decoder.bundle.js`。
- 文件 SHA-256：`1e36e2571a8398013ca4b814c87a4c28227e3603329281f0fa3d7b3acaf6d3f3`。

同版本 GitHub Release 的独立附件 `第三方组件源码.zip` 包含上述项目对应源码、上游包装脚本和 workflow、npm 原始发布包及校验记录，获取指引见“许可文件/第三方源码获取说明.txt”。libheif-emscripten 的源码归档不会自动包含 Git 子模块，因此另附 libheif 的准确源码归档，不要只解压前者。

## 构建

1. 在 Linux 构建环境解压各归档，把 libheif 源码放进 libheif-emscripten 的 `libheif` 子目录。
2. 按随附 `.github/workflows/emscripten.yml` 安装 Emscripten 3.1.61、Node.js 及 TypeScript，并设置对应构建环境。
3. 上游 WASM 任务将 `scripts/run-ci.sh` 中 `USE_WASM=0` 改为 `USE_WASM=1 USE_UNSAFE_EVAL=0`，将 `USE_TYPESCRIPT=0` 改为 `USE_TYPESCRIPT=1`。执行该任务的安装、准备、构建和 `dist-prep.sh wasm` 步骤。
4. libde265 依赖由 `build-emscripten.sh` 解压构建；可将随附 `libde265-1.0.15.git-source.tar.gz` 复制到该脚本运行目录并命名为 `libde265-1.0.15.tar.gz`，避免重复下载。该归档来自 v1.0.15 标签，需要 autoconf、automake、libtool 等工具运行源码自带的 `autogen.sh`。
5. 在解压的 libheif-js 项目中安装其 `package.json` 列出的构建依赖，使用 `node scripts/install.js <上一步生成的libheif.tar.gz>` 生成 `libheif-wasm/libheif-bundle.js`。上游源码中的工作流和脚本是具体步骤依据。

构建工具和 JS 构建依赖的具体补丁版本可能影响输出字节；以上是上游构建流程，并非声称已在本次发布环境重建出逐字节一致结果。当前分发的文件直接来自原始 npm 包，已逐字节核对。

## 替换

退出图片素材库，备份原 `resources/app/heic-decoder.bundle.js`，将新生成的 `libheif-bundle.js` 放到该位置并改为同名，再重新启动软件。接口相容版本应保留 CommonJS 工厂导出：`require('./heic-decoder.bundle.js')()`，并提供 `ready`、`HeifDecoder` 等所需接口。

不需要作者签名、密钥或网络授权来替换该文件。替换不会要求清除个人 `data`、模型或素材库。请保留各第三方许可声明。
