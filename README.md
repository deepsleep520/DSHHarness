<div align="center">

<img src="icon.jpg" width="128" alt="DSH Harness 图标">

# DSH Harness

**把一个完整的 Linux 沙箱和一个 AI 编程助手，装进你的安卓手机。**
不需要 root、不需要 Termux、不需要电脑。

[下载安装包](../../releases) · [更新日志](CHANGELOG.md) · [第三方组件声明](THIRD_PARTY_NOTICES.md) · [安全说明](SECURITY.md)

</div>

---

## 关于源码：本仓库不提供源码

这个仓库只做两件事：

1. **发布安装包**（Releases 里的 APK）；
2. **公示第三方开源组件的许可证**（本应用打包了别人的开源软件，按许可以此方式声明）。

应用自有的代码与资源（Java / JavaScript / Shell / 构建脚本 / 界面素材）**不再公开**，版权归作者所有；使用条款见 [LICENSE-APP.md](LICENSE-APP.md)。

- 想要**能装能用的 App** → 去 Releases 下载。
- 想找**第三方开源项目**（PRoot、Alpine、Node.js、talloc、Shizuku…）→ 见 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) 与 [`licenses/`](licenses/README.md)。

## 下载

- 安装包统一放在 **Releases** 页面，文件名形如 `DSHHarness-v1.0betaN.apk`。
- 每个 Release 的说明里都给出该包的 **sha256 与字节数**，下载后可自行核对。
- APK 大小约 **184 MB**（内置 PRoot + Alpine 根文件系统 + Node 运行时），安装后建议预留 **2 GB** 空间。

## 这是什么

DSH Harness 是一个安卓 App：它在应用私有目录里用 [PRoot](THIRD_PARTY_NOTICES.md) 拉起一个 **Alpine Linux 沙箱**，在沙箱里跑 Node.js 与 AI 编程助手 **dsh**（npm 包 `@deepseek-ai/dsh`），再用内置 WebView 把 dsh 的 Web 界面显示出来。

沙箱里的 `/mnt/sdcard`、`/sdcard`、`/storage/emulated/0` 都指向手机的**真实存储**；在你授权「无障碍 / Shizuku / Root」任一条通道后，沙箱里的 AI 还能直接操作这台手机：点按、输入、截屏、装应用。

一句话：**让没有电脑、也不想折腾 root 的人，在手机上得到一个真 Linux + 真 AI Agent。**

> DSH Harness 与 DeepSeek 官方**没有隶属关系**；`dsh` 由官方维护，本项目只负责把它跑起来并套上手机外壳。

## 能拿它做什么

- **手机上跑 Linux 命令**：`apt`、`python`、`git`、`ffmpeg`… 想装什么装什么（沙箱自带包管理器）。
- **让 AI 直接干活**：整理相册、批量改名、转换格式、抓取整理资料、写脚本并当场运行。
- **让 AI 操作手机本体**：读屏、点按、输入、截图、开关应用（需要你选一条提权通道）。
- **当便携开发机**：SSH、git 仓库、Node/Python 项目，代码放在真机存储里，随时接手。
- **局域网当服务器**：同一 Wi-Fi 下，用电脑浏览器打开手机上的 dsh 界面。
- **换机带走一切**：会话、配置、凭据打包导出，新设备导入即用。

## 亮点

| 能力 | 说明 |
| --- | --- |
| 零 root 起步 | 基础档完全不需要提权，不弹任何授权框 |
| 真机存储直通 | 三个入口路径等价，走 PRoot 绑定而非软链；`DCIM`/`Pictures`/`Movies`/`Android` 可选只读保护 |
| 五档控制通道 | 自动 / 无障碍 / Root / Shizuku / 基础，切换即按新档位重启沙箱 |
| 内置 Web UI 外壳 | 不用装浏览器、不用敲地址；沉浸式全屏、界面缩放、横竖屏、网页缩放都有独立开关 |
| 局域网访问 | 手机当服务器，电脑当终端 |
| 导入导出 | 会话 + 配置 + 凭据打包成 `.zip`，跨设备迁移 |
| 自愈 | WebView 崩溃自动重建、沙箱掉线自动拉起、启动过程有确定性进度条 |

## 安装步骤

1. 从 Releases 下载 `DSHHarness-vX.Y.apk`，允许「安装未知来源应用」后安装。
2. 首次打开：允许**通知**与**所有文件访问**权限。
3. 等它把内置沙箱解压到应用私有目录（几十秒到几分钟，视机型），顶部状态变成「已就绪」后自动进入对话界面；没自动进就点「重试」。
4. 直接说人话，例如「把相册里的截图按月份分好文件夹」。
5. 想让它动手机本体：**设置 → 权限状态**，开启「无障碍服务 / Root / Shizuku」任意一条通道。
6. 想让它常驻后台：**设置 → 常驻通知（保活）** 打开，并把 App 加入电池优化白名单。
7. 换手机：**设置 → 聊天记录 → 导出全部会话（.zip）**，新设备导入。

## 系统要求

| 项 | 要求 |
| --- | --- |
| 系统 | Android 8.0 及以上（`minSdkVersion 24`） |
| CPU | **仅 arm64（arm64-v8a）** |
| 存储 | 安装包约 184 MB，运行后建议预留 2 GB |
| 提权（可选） | Root（KernelSU / Magisk）或 Shizuku 或无障碍服务，任一条即可 |

## 工作原理

```
安卓 App（Java，零第三方 Gradle 依赖）
   └─ 应用私有目录里释放内置 rootfs
        └─ PRoot（ptrace 路径映射）：把真机存储绑定进沙箱
             └─ Alpine Linux + Node.js
                  └─ dsh（AI Agent + Web UI）跑在 127.0.0.1:3280
                       └─ App 内置 WebView 显示它，并注入手机外壳设置
```

- 沙箱只用**应用私有目录**，不需要系统分区，也不改动系统设置。
- dsh 的 Web 服务只监听 `127.0.0.1`；局域网转发要你手动开启。
- 手机控制面（`127.0.0.1:3282`）只在 App 内使用，带随机 token。
- 安装包内的自有脚本以**加密资源**形式存放，仅在签名匹配的原版安装包里才能解密运行。

## 隐私与安全

- **不收集数据**：没有账号、没有埋点、没有上传；会话、凭据、文件全部留在这台手机上。
- **不内置 API Key**：AI 相关的请求由沙箱里的 dsh 按你自己的配置发出。
- **权限可控**：真机存储读写只有在你授予「所有文件访问」后才生效；敏感目录（`DCIM`/`Pictures`/`Movies`/`Android`）可选择只读保护。
- **提权可选**：基础档零提权；无障碍 / Shizuku / Root 都需要你在系统里显式开启，随时可关。
- 安全问题请走 [SECURITY.md](SECURITY.md) 里的私下披露渠道，不要开公开 Issue。

## 第三方组件与许可

通过 Releases 分发的 APK **打包了下列第三方开源组件**，它们的版权与许可证归各自项目所有：

| 组件 | 随包形态 | 许可证 | 许可全文 |
| --- | --- | --- | --- |
| PRoot | `libproot.so`、`libproot-loader.so` | GPL-2.0 | [licenses/GPL-2.0.txt](licenses/GPL-2.0.txt) |
| Alpine Linux（musl / busybox / bash 等用户空间） | `rootfs.tar.gz` | 各包自身（MIT / BSD / GPL-2.0 / LGPL-2.1 等） | [licenses/README.md](licenses/README.md) |
| Node.js | 沙箱内 Node 运行时 | MIT | [licenses/MIT-Node.txt](licenses/MIT-Node.txt) |
| `@deepseek-ai/dsh` 及其依赖 | 沙箱内 npm 包（未修改） | MIT | [licenses/MIT-dsh.txt](licenses/MIT-dsh.txt) |
| talloc | `libtalloc.so.2` | LGPL-3.0 | [licenses/LGPL-3.0.txt](licenses/LGPL-3.0.txt) |
| Shizuku API | 编译期引用（运行时由设备上的 Shizuku 提供） | Apache-2.0 | [licenses/Apache-2.0.txt](licenses/Apache-2.0.txt) |
| AndroidX / Android SDK | App 基础库 | Apache-2.0 | [licenses/Apache-2.0.txt](licenses/Apache-2.0.txt) |

细节、各组件上游地址与源码获取方式见 **[THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)**。

## 许可

| 部分 | 许可 |
| --- | --- |
| 应用自有代码与资源（不随本仓库公开） | [LICENSE-APP.md](LICENSE-APP.md)：免费使用；**禁止反编译、修改、二次打包与再分发** |
| 打包进 APK 的第三方组件 | 各自许可证（见上表），不受本应用许可限制 |

## 常见问题

**Q：为什么不开源了？**
A：作者保留自有代码的版权，只发布可运行的安装包，避免被二次修改后以其它名义分发。第三方组件的许可与源码照旧公示在上面的链接里。

**Q：能覆盖安装旧版本吗？**
A：同包名（`com.dshharness.app`）之间可以直接覆盖安装，数据保留。更名前的旧包（`com.dshbox.*`）不能覆盖，需要用「导出/导入会话」迁移。

**Q：为什么 APK 这么大？**
A：里面装了一整套运行时：PRoot + Alpine 根文件系统 + Node.js + dsh 本体，不是壳套网页。

**Q：必须联网吗？**
A：安装与本地使用不需要联网；只有 dsh 与 AI 服务通信时才需要网络。

**Q：会收集我的聊天记录吗？**
A：不会。记录只写在这台手机的沙箱里，除了你配置的 AI 服务外没有第三方能拿到。

## 更新日志

见 [CHANGELOG.md](CHANGELOG.md)。版本号带 `beta` 的为预览版。

## 给 AI 助手与搜索引擎的摘要

见 [llms.txt](llms.txt)（机器可读的项目事实、能力清单与关键词）。
