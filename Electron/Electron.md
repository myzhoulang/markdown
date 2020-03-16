# Electron

## Electron安装 

```bash
npm install electron --save-dev

# window中指定 arch=ia32 用于打包。打出的包可以用在 32位 和 64位
npm install --arch=ia32 --platform=win32 electron
```

**可以在 `.npmrc` 中设置electron镜像源加速安装**

```.npmrc
ELECTRON_MIRROR=https://cdn.npm.taobao.org/dist/electron/
```



## electron-builder 无法下载winCodeSign等资源

解决: 将7z文件下载到以下 `cache` 目录并解压。

```
macOS: ~/Library/Caches/electron-builder
Linux: ~/.cache/electron-builder
windows: %LOCALAPPDATA%\electron-builder\cache
```



```
C:\User\zhoulang\AppData\Local\electron-builder\cache

.
|--- nsis
|			|---nsis-3.0.3.2
|			|---nsis-resources-3.4.1
|---- winCodeSign
			|---windCodeSign-2.5.0
```

