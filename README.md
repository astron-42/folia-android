# Folia Android 发布包

版本 `0.7.19`，均为已签名的 release 包。两个 APK 使用同一张证书（**CN=Folia**），
签名 SHA-256 指纹一致，因此可以**互相覆盖安装**（无需先卸载）。

## 下载

| 文件 | 说明 | 大小 |
| --- | --- | --- |
| [`apk/Folia-0.7.19-android-release-v32.apk`](apk/Folia-0.7.19-android-release-v32.apk) | 修复版 v32 | 13.6 MB |
| [`apk/Folia-0.7.19-android-fixed-v31.apk`](apk/Folia-0.7.19-android-fixed-v31.apk) | 微优化版 v31（见下） | 13.6 MB |
| [`index.html`](index.html) | 下载页（可用 GitHub Pages 托管） | — |

## 仓库结构

```text
folia-android/
├── README.md     # 本文件
├── index.html    # 下载页，按钮指向 apk/ 下的安装包
└── apk/
    ├── Folia-0.7.19-android-release-v32.apk
    └── Folia-0.7.19-android-fixed-v31.apk
```

## 签名与完整性校验

| 项目 | 值 |
| --- | --- |
| 签名方案 | APK Signature Scheme v2（无 v1 JAR 签名，需 Android 7.0+ 安装） |
| 证书主体 | `C=CN, ST=Unknown, L=Unknown, O=Personal, OU=Personal, CN=Folia` |
| 证书 SHA-256 | `52:68:F6:85:93:9E:82:49:29:93:23:26:43:12:64:77:B0:E0:D9:C7:A4:2F:B7:4C:75:5C:FE:6F:95:61:B3:B0` |
| 有效期 | 2026-10-18 至 2054-03-05 |

复核签名指纹：

```bash
apksigner verify --print-certs apk/Folia-0.7.19-android-release-v32.apk
```

文件 SHA-256（用于校验下载是否完整）：

```text
eef07d54d4481e4143a75236b71e3114aca2c42d9b96cf12adda43dd388c58e4  apk/Folia-0.7.19-android-fixed-v31.apk
21576680a1e8accba69d7b5b09cdf955197506408d2aa3bf5b187d073dc7669c  apk/Folia-0.7.19-android-release-v32.apk
```

PowerShell 计算本地文件的哈希：

```powershell
Get-FileHash .\apk\Folia-0.7.19-android-release-v32.apk -Algorithm SHA256
```

## 微优化版 v31 说明

仅在 **原生 Android 壳内**（`html.is-native-app`）生效，桌面端与浏览器版完全无影响：

1. 弹窗中大面积装饰光斑（`blur-[70px]` 起）改用径向渐变遮罩，不再走模糊 pass；
2. 铺满视口的遮罩层（弹窗/命令面板/扫码登录/全屏页）关闭 `backdrop-blur`；
3. 手指滑动期间临时关闭所有毛玻璃，停手约 0.16s 后恢复——静止观感不变。

**没有加帧率上限，120Hz 保留。**

对应源码：

- `src/index.css`（三条 `html.is-native-app` 降级规则）
- `src/utils/nativeScrollActivity.ts`（滚动/拖动期间挂 `folia-scroll-active` 类）
- `src/index.tsx`（调用 `installNativeScrollActivity()`）

## 安装

用系统浏览器打开 `index.html`（或 GitHub Pages 地址），点对应按钮下载 APK，
在文件管理器中点开安装（需允许「安装未知应用」）。

> 若浏览器提示「不支持的文件类型」，请改用系统浏览器打开本页，或在文件管理器中找到下载好的
> `.apk` 手动安装。

## 在线下载页

在仓库 **Settings → Pages** 中把 Source 设为 `Deploy from a branch`、分支选 `main`、目录选 `/ (root)`，
发布后即可通过下面的地址访问下载页（APK 也在同一站点下，页面里的相对链接可直接下载）：

```text
https://astron-42.github.io/folia-android/
```
