# BLEUnlock

## 请注意：本应用不在 Mac App Store 上架，你可以在这里免费获取！

![CI](https://github.com/ts1/BLEUnlock/workflows/CI/badge.svg)
![Github All Releases](https://img.shields.io/github/downloads/ts1/BLEUnlock/total.svg)
[![Buy me a coffee](img/buymeacoffee.svg)](https://www.buymeacoffee.com/tsone)

BLEUnlock 是一个小巧的菜单栏工具，它通过 iPhone、Apple Watch 或任何其他低功耗蓝牙（BLE）设备与 Mac 的距离来锁定和解锁你的 Mac。

本文档同时提供 [英文版](README.md) 和 [日文版](README.ja.md)。

## 功能特性

- 无需在 iPhone 上安装任何 App
- 适用于任何会从[静态 MAC 地址](#关于-mac-地址)周期性发送信号的 BLE 设备
- 当 BLE 设备靠近 Mac 时，自动解锁 Mac，无需输入密码
- 当 BLE 设备远离 Mac 时，自动锁定 Mac
- 可选择在锁定/解锁时运行自定义脚本
- 可选择在设备靠近时唤醒处于睡眠状态的显示器
- 可选择在离开和返回时暂停/继续音乐或视频播放
- 密码安全地存储在钥匙串中

## 系统要求

- 支持低功耗蓝牙的 Mac
- macOS 10.13 (High Sierra) 或更高版本
- iPhone 5s 或更新机型、Apple Watch（所有型号），或其他具有[静态 MAC 地址](#关于-mac-地址)并会周期性发送信号的 BLE 设备

## 安装

### 使用 Homebrew Cask

```sh
brew install bleunlock
```

### 手动安装

从 [Releases](https://github.com/ts1/BLEUnlock/releases) 下载 zip 文件，解压后移动到“应用程序”文件夹。

## 初始设置

首次启动时，应用会请求以下权限，请务必全部允许：

权限 | 说明
----|---
蓝牙 | 显然，BLEUnlock 需要访问蓝牙。请选择“好”。
辅助功能 | 解锁锁定屏幕时需要此权限。点击“打开系统偏好设置”，点击左下角的锁图标解锁，然后开启 BLEUnlock。
钥匙串 | （不一定每次都会询问）如果系统询问，必须选择**始终允许**，因为锁屏状态下仍然需要访问密码。
通知 | （可选）BLEUnlock 在锁定屏幕时会在锁屏界面显示一条消息，方便你确认它是否正常工作。此外，要在锁屏界面看到消息，需要在“通知”偏好设置中将“显示预览”设置为“始终”。

> 注意：随着 macOS 版本的更新，所需的权限数量可能会增加。如果你使用的是较旧的系统，可能不会看到上述某些权限请求。

然后，应用会要求你输入登录密码，用于解锁锁屏界面。密码会安全地存储在钥匙串中。

最后，从菜单栏图标中选择 *设备*。应用会开始扫描附近的 BLE 设备。选择你的设备，即可完成设置！

## 选项说明

选项 | 说明
----|---
立即锁定屏幕 | 无论 BLE 设备是否在附近，都会立即锁定屏幕；当 BLE 设备离开后再靠近时，屏幕会自动解锁。适合在离开座位前确保屏幕已锁定。
解锁 RSSI | 解锁所需的蓝牙信号强度。数值越大，BLE 设备需要离 Mac 越近才能解锁。选择 *禁用* 可关闭自动解锁。
锁定 RSSI | 锁定所需的蓝牙信号强度。数值越小，BLE 设备离 Mac 越远时就会锁定。选择 *禁用* 可关闭自动锁定。
延迟锁定 | 检测到 BLE 设备离开后，等待多长时间再锁定 Mac。如果设备在此时间内重新靠近，则不会锁定。
无信号超时 | 从最后一次收到信号到锁定之间的时间。如果你经常遇到误报“信号丢失”而锁定，请增大此值。
靠近唤醒 | 锁定期间，当 BLE 设备靠近时唤醒处于睡眠状态的显示器。
唤醒但不解锁 | 当显示器从睡眠中唤醒时（无论是通过“靠近唤醒”自动唤醒还是手动唤醒），BLEUnlock 都不会解锁 Mac。这样可兼容 macOS 自带的 Apple Watch 解锁功能（该功能可在 BLEUnlock 唤醒屏幕后立即工作），或者适合你希望更快显示锁屏界面但不想自动解锁的场景。
锁定时暂停“正在播放” | 锁定/解锁时，暂停/继续由 *正在播放* 小组件或键盘上的 ⏯ 键控制的音乐或视频播放（包括 Apple Music、QuickTime Player 和 Spotify）。
使用屏幕保护程序锁定 | 如果启用此选项，BLEUnlock 会启动屏幕保护程序而不是直接锁定。要使其正常工作，需要在 *安全性与隐私* 偏好设置中将 *进入睡眠或开始屏幕保护程序后要求输入密码* 设置为 **立即**。
锁定时关闭屏幕 | 锁定时立即关闭显示器。
设置密码… | 如果你更改了登录密码，请使用此选项更新。
被动模式 | 默认情况下，BLEUnlock 会主动连接 BLE 设备并读取 RSSI。大多数情况下推荐使用默认模式，工作更稳定。但是，如果你正在使用键盘、鼠标、触控板，尤其是蓝牙个人热点等其他蓝牙设备，默认模式可能会造成相互干扰，2.4GHz Wi-Fi 也可能造成干扰。如果遇到蓝牙不稳定的情况，请开启被动模式。
登录时启动 | 登录系统时自动启动 BLEUnlock。
设置最小 RSSI | RSSI 低于此值的设备不会显示在设备扫描列表中。

## 故障排除

### 在列表中找不到我的设备

如果你的 BLE 设备不是 Apple 设备，BLEUnlock 可能无法获取设备名称。
这种情况下，设备会显示为 UUID（一长串由连字符分隔的十六进制数字）。
要识别设备，可以尝试将设备靠近或远离 Mac，观察 RSSI（dB 值）是否相应变化。

如果列表中**完全看不到任何**设备，请尝试下面介绍的重置蓝牙模块的方法。

### 无法解锁

请确认在 *系统偏好设置* > *安全性与隐私* > *隐私* > *辅助功能* 中已开启 BLEUnlock。
如果已经开启，请尝试先关闭再重新开启。

如果系统询问是否允许访问钥匙串中的密码，必须选择 *始终允许*，因为锁屏状态下仍然需要该密码。

### 经常出现“信号丢失”

请增大 *无信号超时*。
或者尝试开启 *被动模式*。

### 蓝牙键盘、鼠标、个人热点等设备工作异常

首先，按住 Shift + Option 点击菜单栏或控制中心中的蓝牙图标，然后点击 *重置蓝牙模块*。

在 macOS 12 Monterey 中，该选项已不再提供。
请改为在“终端”中执行以下命令来重置蓝牙模块：

```sh
sudo pkill bluetoothd
```

该命令会要求输入登录密码。

如果问题仍然存在，请开启 *被动模式*。

### 长时间运行后，设备列表一直显示“扫描中…”

这是 macOS CoreBluetooth 的一个已知问题：在长时间扫描或系统睡眠唤醒后，系统可能不再上报蓝牙广播包，但应用仍被告知“正在扫描”。BLEUnlock 会在打开 *设备* 菜单时强制重新开始扫描，并会定期检查扫描是否仍然有效，必要时自动重启扫描。

如果设备列表仍然为空，可以尝试：

1. 关闭再重新打开 Mac 的蓝牙；
2. 执行 `sudo pkill bluetoothd` 重置蓝牙模块；
3. 退出并重新启动 BLEUnlock。

## 关于 MAC 地址

与经典蓝牙不同，低功耗蓝牙设备可以使用*私有* MAC 地址。
私有地址是随机的，并且可能会不时变化。

近年的智能设备（无论是 iOS 还是 Android）倾向于使用大约每 15 分钟变化一次的私有地址，这很可能是为了防止被追踪。

而 BLEUnlock 要跟踪你的设备，设备的 MAC 地址必须是静态的。

幸运的是，在 Apple 设备上，如果你使用与 Mac 相同的 Apple ID 登录，MAC 地址会被解析为真实的（公共）地址。

对于包括 Android 在内的其他设备，目前尚不清楚如何解析地址。
如果你的非 Apple 设备会随时间更改 MAC 地址，那么很遗憾，BLEUnlock 无法支持该设备。

要检查 MAC 地址是否被正确解析，请将 BLEUnlock 的 *设备* 扫描列表中显示的 MAC 地址与设备上显示的 MAC 地址进行比较。

## 在锁定/解锁时运行脚本

BLEUnlock 会在锁定和解锁时运行以下位置的脚本：

```sh
~/Library/Application Scripts/jp.sone.BLEUnlock/event
```

根据事件类型，脚本会收到不同的参数：

| 事件 | 参数 |
|------|------|
| 因 RSSI 过低而被 BLEUnlock 锁定 | `away` |
| 因无信号而被 BLEUnlock 锁定 | `lost` |
| 被 BLEUnlock 解锁 | `unlocked` |
| 手动解锁 | `intruded` |

> 注意：要让 `intruded` 事件正常工作，需要在 *安全性与隐私* 偏好设置中将 *进入睡眠后要求输入密码* 设置为 **立即**。

### 示例

下面是一个示例脚本：当有人手动解锁 Mac 时，它会通过 LINE Notify 发送一条消息，并附上 Mac 前人物的照片。

```sh
#!/bin/bash

set -eo pipefail

LINE_TOKEN=xxxxx

notify() {
    local message=$1
    local image=$2
    if [ "$image" ]; then
        img_arg="-F imageFile=@$image"
    else
        img_arg=""
    fi
    curl -X POST -H "Authorization: Bearer $LINE_TOKEN" -F "message=$message" \
        $img_arg https://notify-api.line.me/api/notify
}

capture() {
    open -Wa SnapshotUnlocker
    ls -t /tmp/unlock-*.jpg | head -1
}

case $1 in
    away)
        notify "$(hostname -s) is locked by BLEUnlock because iPhone is away."
        ;;
    lost)
        notify "$(hostname -s) is locked by BLEUnlock because signal is lost."
        ;;
    unlocked)
        #notify "$(hostname -s) is unlocked by BLEUnlock."
        ;;
    intruded)
        notify "$(hostname -s) is manually unlocked." $(capture)
        ;;
esac
```

`SnapshotUnlocker` 是使用“脚本编辑器”创建的 .app，其脚本内容如下：

```applescript
do shell script "/usr/local/bin/ffmpeg -f avfoundation -r 30 -i 0 -frames:v 1 -y /tmp/unlock-$(date +%Y%m%d_%H%M%S).jpg"
```

需要这个 App 是因为 BLEUnlock 没有相机权限。将该权限授予这个 App 即可解决此问题。

## 赞助

Apple Developer Program 的年费由捐赠资助。

如果你喜欢这个应用，欢迎通过 [Buy Me a Coffee](https://www.buymeacoffee.com/tsone) 或 [PayPal Me](https://www.paypal.com/paypalme/my/profile) 进行捐赠，以支持我继续维护它。

## 致谢

- [peiit](https://github.com/peiit)：中文本地化
- [wenmin-wu](https://github.com/wenmin-wu)：最小 RSSI 和移动平均
- [stephengroat](https://github.com/stephengroat)：CI
- [joeyhoer](https://github.com/joeyhoer)：Homebrew Cask
- [Skyearn](https://github.com/Skyearn)：Big Sur 风格图标
- [cyberclaus](https://github.com/cyberclaus)：德语、瑞典语、挪威语（Bokmål）和丹麦语本地化
- [alonewolfx2](https://github.com/alonewolfx2)：土耳其语本地化
- [wernjie](https://github.com/wernjie)：唤醒但不解锁
- [tokfrans03](https://github.com/tokfrans03)：语言修正

图标基于从 materialdesignicons.com 下载的 SVG 文件制作。
这些图标最初由 Google LLC 设计，并依据 Apache License 2.0 授权。

## 许可证

MIT

Copyright © 2019-2022 Takeshi Sone.
