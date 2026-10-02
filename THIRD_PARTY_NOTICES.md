# 第三方组件声明

本仓库**不包含应用自有源码**（见 [LICENSE-APP.md](LICENSE-APP.md)）。
但通过 **Releases 分发的 APK 是一个「运行时封装包」**，它内部包含下列第三方开源组件；这些组件的版权与许可证归各自项目所有，DSH Harness 只是把它们打包并正确启动。

> 变更提示：本声明此前写着「DSH Harness 源码以 Apache-2.0 发布」。自有源码自 v1.0beta13 起不再公开，该表述已作废。

## 随 APK 分发的组件

| 组件 | 随包形态 | 许可证 | 许可全文 | 上游 |
| --- | --- | --- | --- | --- |
| PRoot | `libproot.so`、`libproot-loader.so`（应用私有目录） | **GPL-2.0** | [licenses/GPL-2.0.txt](licenses/GPL-2.0.txt) | <https://github.com/termux/proot> |
| Alpine Linux 用户空间 | `rootfs.tar.gz` 内的 musl libc / busybox / bash / coreutils 等 | 各包自身（MIT、BSD、GPL-2.0、LGPL-2.1 等） | [licenses/README.md](licenses/README.md) | <https://alpinelinux.org/> |
| Node.js | 沙箱内 Node 运行时 | MIT | [licenses/MIT-Node.txt](licenses/MIT-Node.txt) | <https://nodejs.org/> |
| `@deepseek-ai/dsh` 及其 npm 依赖 | 沙箱内 npm 包（**未修改**，原样安装） | MIT（`0.2.0-rc.2`） | [licenses/MIT-dsh.txt](licenses/MIT-dsh.txt) | <https://www.npmjs.com/package/@deepseek-ai/dsh> |
| talloc | `libtalloc.so.2` | LGPL-3.0 | [licenses/LGPL-3.0.txt](licenses/LGPL-3.0.txt) | <https://talloc.samba.org/> |
| Shizuku API | 编译期引用；运行时由设备上已安装的 Shizuku 提供 | Apache-2.0 | [licenses/Apache-2.0.txt](licenses/Apache-2.0.txt) | <https://shizuku.rikka.app/> |
| AndroidX / Android SDK | App 基础库 | Apache-2.0 | [licenses/Apache-2.0.txt](licenses/Apache-2.0.txt) | <https://developer.android.com/> |

## GPL / LGPL 组件的源码获取方式

APK 内打包的 **PRoot（GPL-2.0）** 与 **Alpine 用户空间（含 GPL/LGPL 包）** 是未修改的上游版本，对应源码可从这里获取：

- PRoot：<https://github.com/termux/proot>（Termux 维护分支，与包内二进制对应）
- Alpine Linux 软件包：<https://gitlab.alpinelinux.org/alpine/aports>（每个包的源码地址同时记录在其 `APKBUILD` 中）
- talloc：<https://git.samba.org/?p=talloc.git>
- 沙箱内实际安装的包清单（含版本与许可证字段）：设备上 `…/files/rootfs/lib/apk/db/installed`

如果你需要这些组件的具体版本对应的源码副本，请通过 [SECURITY.md](SECURITY.md) 里的私下联系方式索要，作者提供与二进制对应的源码或获取途径。

## 分发注意

- 若你要**重新分发包含上述组件的 APK**，请自行确认所有许可证（尤其是 GPL-2.0 的 PRoot）在分发场景下的要求，并保留各项目的许可证文本与版权声明。
- 本项目与上述所有第三方项目**没有隶属关系**，也不代表它们的立场。

## 本仓库不包含

- 应用自有源码、资源与构建脚本（见 [LICENSE-APP.md](LICENSE-APP.md)）；
- 运行时 rootfs 的构建脚本与配方；
- 任何 API Key、会话数据、私钥或个人凭据。
