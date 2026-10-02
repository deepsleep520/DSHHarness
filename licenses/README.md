# 许可证全文与来源（随 APK 分发的第三方组件）

本目录存放 DSH Harness 安装包里**实际打包的第三方组件**所要求的许可证全文，以及它们的上游来源。
应用自有的代码与资源不在此列（见 [../LICENSE-APP.md](../LICENSE-APP.md)），第三方组件的许可与归属也不受应用许可影响。

| 文件 | 适用组件 | 许可证 | 上游来源 |
| --- | --- | --- | --- |
| [GPL-2.0.txt](GPL-2.0.txt) | PRoot（`libproot.so`、`libproot-loader.so`） | GNU General Public License v2.0 | <https://github.com/termux/proot> |
| [LGPL-3.0.txt](LGPL-3.0.txt) | talloc（`libtalloc.so.2`） | GNU Lesser General Public License v3.0 | <https://talloc.samba.org/> |
| [MIT-Node.txt](MIT-Node.txt) | Node.js 运行时 | MIT（含 Node 自身打包的第三方许可清单） | <https://nodejs.org/> |
| [MIT-dsh.txt](MIT-dsh.txt) | `@deepseek-ai/dsh` 0.2.0-rc.2 | MIT | <https://www.npmjs.com/package/@deepseek-ai/dsh> |
| [Apache-2.0.txt](Apache-2.0.txt) | Shizuku API、AndroidX / Android SDK | Apache License 2.0 | <https://shizuku.rikka.app/>、<https://developer.android.com/> |
| — | Alpine Linux 用户空间（musl、busybox、bash、coreutils 等） | 各包自身（MIT、BSD、GPL-2.0、LGPL-2.1 …） | <https://alpinelinux.org/> / <https://gitlab.alpinelinux.org/alpine/aports> |

## 关于 Alpine Linux 的许可证

Alpine 的根文件系统由数百个独立软件包组成，每个包有各自的许可证（常见为 MIT、BSD、GPL-2.0、LGPL-2.1）。
逐包声明不便于放在仓库里，权威清单就在**沙箱的包数据库**中：

```
<应用私有目录>/files/rootfs/lib/apk/db/installed      # 已安装包的名称、版本与许可证字段
```

各包的源码地址记录在其 `APKBUILD` 中（见上表的 aports 仓库）。如果你需要某个具体版本对应的源码副本，
请通过 [../SECURITY.md](../SECURITY.md) 里的私下渠道联系作者。

## 关于 GPL-2.0（PRoot）

PRoot 是独立程序（通过 `ptrace` 提供路径映射），以**未修改的上游版本**打包进 APK。
GPL-2.0 要求随二进制分发其许可证全文（见 [GPL-2.0.txt](GPL-2.0.txt)）并提供对应源码的获取途径 ——
源码地址见上表，也可向作者索要与包内二进制对应的源码副本。
