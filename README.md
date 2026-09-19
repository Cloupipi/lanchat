# 局域网聊天系统

基于 **C++ + Chromium(WebView2)** 的局域网聊天工具，深蓝深色 Win11 风格，纯局域网内即时通讯，零账号依赖、零第三方库。

> GitHub 仓库：<https://github.com/cloupi/lanchat/releases>


---

## 快速开始

1. 任选一台电脑，双击「服务启动器」(`server.exe`)，窗口会显示：
   - 服务已启动
   - 当前局域网 IP（记下这个 IP，保持窗口开着）
2. 其他电脑（或同一台电脑）双击「聊天客户端」(`client.exe`)：
   - 输入服务器 IP（第一步记下的地址）
   - 昵称已自动生成，点「换」可重新生成，也可以手动输入
   - 可选：Apple / 邮箱 / 手机号登录，或直接点「进入聊天」以游客身份使用
   - 点击「进入聊天」
3. 所有设备连接同一个路由器 / Wi-Fi 即可互相聊天。

---

## 登录与跨设备

- **游客**：不登录直接使用，每次进入自动生成新的随机昵称
- **Apple / 邮箱 / 手机号**：在局域网内为本地模拟登录，仅用于标识身份，不会联网
- **跨设备**：同一账号在不同设备登录，昵称自动统一为首次登录时的昵称；发送的消息都会显示发送者名字
- **历史**：服务端保存最近 200 条消息，换设备登录同一账号会自动回放，游客也能看到近期历史

---

## 常见问题

- **连不上**：确认两台设备在同一个局域网；首次运行服务端时，Windows 防火墙弹窗请选择「允许访问」
- **别人看不到你**：关闭系统防火墙拦截或改用其他局域网 IP
- **升级提示**：1.4.0 起协议扩展，请所有设备使用同一版本（安装包内服务端与客户端配套）
- **更新检查**：需要能访问 GitHub（`api.github.com`）；检查失败不影响聊天功能，可点击版本号重试

---

## 目录说明

| 文件 | 说明 |
| --- | --- |
| `server.exe` | 服务启动器（只显示已启动和局域网 IP） |
| `client.exe` | 聊天客户端（Win11 风格深色界面） |
| `WebView2Loader.dll` | 客户端运行依赖（必须与 client.exe 同目录） |
| `server.cpp` / `client.cpp` | 完整源码（可用 build.bat 自行编译） |
| `build.bat` | 一键重新编译脚本（需要 MSYS2/MinGW g++） |
| `README.md` | 本说明文档 |

---

## 卸载

- **方式一**：开始菜单 → 局域网聊天 → 「卸载局域网聊天」
- **方式二**：设置 → 应用 → 已安装的应用 → 局域网聊天 → 卸载
- **方式三**：直接运行安装目录下的 `uninst000.exe`

---

## 自行编译

需要 [MSYS2 / MinGW-w64 g++](https://www.msys2.org/)（16.1.0+）与 [WebView2 SDK](https://www.nuget.org/packages/Microsoft.Web.WebView2)（`sdk\include`、`sdk\bin\WebView2Loader.dll`）。

```bat
:: 服务端
g++ -O2 -std=c++17 server.cpp -o server.exe -lws2_32 -liphlpapi -static

:: 客户端
g++ -O2 -std=c++17 -DUNICODE -D_UNICODE -I"sdk\include" client.cpp -o client.exe ^
    -lws2_32 -luuid -ldwmapi -lshlwapi -lole32 -loleaut32 -lgdi32 -luser32 -lshell32 -lcomctl32 -lwinhttp -municode -mwindows -static
```

或直接运行项目内的 `build.bat`。
