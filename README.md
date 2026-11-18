# macOS 27.3 隐藏状态栏的保护覆盖提示图标

在 macOS 27.3 中，应用获得 **System & App Protection Override（系统与 App 保护覆盖）** 权限后，菜单栏会显示蓝色安全提示图标。

本方法只隐藏该提示图标，**不会撤销应用权限**，因此微信输入法等应用的键盘快捷键权限可以继续使用。

> 注意：这是针对当前 macOS 27.3 测试版的未公开 workaround，后续系统更新可能改变或移除该行为。此设置是全局的，会隐藏所有应用的同类提示，而不仅是微信输入法。隐藏后，你将无法通过该图标直观看到哪些应用正在使用保护覆盖权限。

## 隐藏图标

打开“终端”，逐行执行：

```bash
sudo mkdir -p /Library/Preferences/FeatureFlags/Domain
sudo defaults write /Library/Preferences/FeatureFlags/Domain/TCC AX_TRANSPARENCY_INDICATOR -dict Enabled -bool false
```

输入 Mac 管理员密码。输入密码时终端不会显示字符，这是正常现象。

执行完成后，**重启 Mac**。

## 原理

命令会在以下配置文件中设置 `AX_TRANSPARENCY_INDICATOR` Feature Flag：

```text
/Library/Preferences/FeatureFlags/Domain/TCC.plist
```

该设置只控制系统保护覆盖提示的显示，不修改应用的 TCC 授权，也不需要关闭 SIP。

## 恢复显示

如需恢复 macOS 默认行为，执行：

```bash
sudo defaults delete /Library/Preferences/FeatureFlags/Domain/TCC AX_TRANSPARENCY_INDICATOR
```

然后再次**重启 Mac**。

## 检查应用权限

如需查看或撤销应用权限，请打开：

**系统设置 → 隐私与安全性 → System & App Protection Override**

隐藏图标不会关闭这里的权限。撤销权限可能会影响微信输入法的自定义全局语音快捷键等功能。

## 参考来源

- [How to hide the new macOS 27.3 menu bar indicator — 9to5Mac](https://9to5mac.com/2026/11/17/how-to-hide-the-new-macos-27-3-menu-bar-indicator/)
- [macOS 27.3 Warns Against Giving AI Apps Full Mac Control — MacRumors](https://www.macrumors.com/2026/11/16/macos-27-3-full-desk-access/)
