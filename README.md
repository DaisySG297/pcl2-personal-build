# PCL2 Personal Build — AppID Application Notes

This repository contains **documentation only**. It explains the purpose and authentication
behaviour of a personally compiled build of an open-source Minecraft launcher, for the purpose
of a Minecraft Java Edition AppID review request.

**No binaries, no launcher source code and no credentials are distributed in this repository.**

---

## 1. What this application is

A locally compiled, single-user build of the **open-source edition of Plain Craft Launcher 2
(PCL2) version 2.99.0.0**, a Minecraft: Java Edition launcher.

| Item | Value |
|---|---|
| Application (client) ID | `0c775069-ddd0-4f67-b14e-20ea7cdd3fc2` |
| Supported account type | Personal Microsoft accounts only |
| Public client flows | Enabled (device code flow) |
| Upstream source | https://github.com/Meloong-Git/PCL (tag `2.99.0.0`) |
| Distribution | None. Built for the owner's own machine only. |

The build is compiled locally from the public upstream source on a Windows machine
(VB.NET / WPF, .NET Framework 4.8), using the .NET SDK after converting the project file to
SDK-style. Nothing is offered for download, and the build is not shared with anyone else.

## 2. How authentication is used

The application uses the **standard Microsoft identity platform device code flow** and nothing else:

* Endpoint: `https://login.microsoftonline.com/consumers/oauth2/v2.0/devicecode` and `/token`
* Scopes requested: `XboxLive.signin offline_access`
* Then the documented public Xbox Live / Minecraft chain:
  `user.auth.xboxlive.com` → `xsts.auth.xboxlive.com` → `api.minecraftservices.com/authentication/login_with_xbox`
  → `api.minecraftservices.com/minecraft/profile`

The signed-in account is the owner's own Microsoft account. Authentication is **required** before
the launcher will start the game; the build does not bypass, disable or weaken authentication,
game ownership checks or licence checks, and it cannot be used with an account that does not own
Minecraft: Java Edition.

## 3. Data handling

* No telemetry, no analytics, no crash reporting, no update service.
* No third-party login proxy or intermediate server: every authentication request goes directly
  from the local machine to Microsoft / Mojang endpoints over TLS.
* No account credentials, tokens or personal data are collected, stored remotely or transmitted to
  the author of this repository.

## 4. Modifications relative to the public upstream source

1. Project file converted to SDK-style so it can be built with the .NET SDK; behaviour unchanged.
2. Compile-time feature constants of the open-source branch adjusted so that functionality that
   already exists in the public source (version list, installation, launching) is reachable in the
   locally built binary.
3. Application secret values (an AppID and a third-party mod-service API key) are supplied locally
   at build time. **They are not committed to this repository.**

No code was added that changes how authentication, ownership verification or licence checks work.

## 5. Attribution and licensing

* Plain Craft Launcher (PCL) is created by **龙腾猫跃 (LongTengMaoYue)** —
  https://github.com/Meloong-Git/PCL
* PCL is distributed under the *PCL 分发有限许可* together with the
  *PCL 存储库合理使用指南*; see the `LICENCE` file in the upstream repository.
* This repository is **not** a distribution of PCL, contains no PCL code, and is not affiliated
  with, endorsed by or supported by the PCL author.
* Any third-party build of PCL must follow the upstream naming and attribution rules; this
  document makes no claim to the PCL name.

## 6. Contact

`daisysq297@163.com`

---

## 中文说明（摘要）

本仓库仅含文档，不含任何程序、源码或密钥。它说明的是一个**个人本地编译**的 PCL2 开源版
（2.99.0.0）构建：仅本人使用、不对外分发，认证使用微软官方设备代码流
（`XboxLive.signin offline_access`），不收集任何数据、不经过任何第三方中转服务器，
且不绕过正版验证与所有权检查。PCL 版权归龙腾猫跃所有，本仓库与其无关。
