# AVD模拟真实Pixel设备指纹

### 1. 创建虚拟机并解锁OEM锁
在安卓Studios里创建虚拟机，选Pixel 10 Pro,系统镜像选择Google API的，不要选择Play Store
然后再终端执行
```bash
~/Android/Sdk/emulator/emulator -avd "Pixel_10_Pro" -writable-system -no-snapshot-load
```

### 2. 获取root、禁用AVB并挂载系统读写
```bash
adb root
adb remount
```

> [!NOTE]
> 如果最后一行是  
> Now reboot your device for settings to take effect  
> 则还需要  
> ```bash
> adb reboot
> ```
> 重启后再获取root并重新挂载  
> ```bash
> adb root
> adb remount
> ```

- 注入真机的指纹(pixel 10 pro):
```bash
adb shell "echo -e 'ro.product.model=Pixel 10 Pro\nro.product.brand=google\nro.product.name=blazer\nro.product.device=blazer\nro.build.fingerprint=google/blazer/blazer:17/CP3A.260905.009/16091614:user/release-keys\nro.build.type=user\nro.build.tags=release-keys\nro.serialno=58201FDKD001B3' >> /system/build.prop"
```

> [!NOTE]
> 有能力者可前往 [https://developers.google.com/android/images](https://developers.google.com/android/images) 下载模拟机型下虚拟机操作系统相近的版本的刷机包提取 `/system/build.prop` 进行diff自行修改。

- 重启命令:
```bash
adb reboot
```

- 验证指纹:
```bash
adb shell getprop ro.product.model
adb shell getprop ro.build.fingerprint
```

- 可选: 删除su:
```bash
adb shell "rm -f /system/bin/su /system/xbin/su /sbin/su /vendor/bin/su"
```

- 验证su是否有残留:
```bash
adb shell "which su"
```
