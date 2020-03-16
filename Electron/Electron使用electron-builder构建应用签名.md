# Electron 使用 electron-builder构建应用签名



* 签名准备

  >1. 购买证书或`MakeCert.exe`和`pvk2pfx.exe`(生成`.pfx`的自签名证书)
  >2. 使用`electron-builder`构建的`Electron`项目。
  >3. 需要有外网环境
  >
  >**自签名只能在本地开发或测试的时候使用，生产环境还是需要购买权威机构的证书。**

  * 证书

    1. 本地开发可以使用 `MakeCert.exe`工具创建自签名证书。

       * -n:  主题名称
       * -r : 指定证书将自签名
       * -sv: 指定包含私钥容器的文件

       ```bash
       # 生成 .cer 证书
       ./makecert.exe -n "CN=TempCA" -r -sv TempCA.pvk TempCA.cer
       
       # 将 .cer 证书 转换成 .pfx 证书
       ./pvk2pfx.exe -pvk ./TempCA.pvk -spc ./TempCA.cer -pfx ./cert3.pfx
       
       ```

  * `electron-builder`签名的时候，会到远程的服务器获取一个时间。所以需要有外网环境。



* 遇到的问题

  1. `Can't sign app: The specified timestamp server either could not be reached or returned an invalid response`

     解决方案：

      1. 查看是否有网络。

      2. 查看时间戳服务器是否正常工作。

      3. 查看`package.json`配置项中 `signingHashAlgorithms`、`rfc3161TimeStampServer`、`timeStampServer`的属性。

         `signingHashAlgorithms`的值为`sha1`就从`timeStampServer`指定的服务器中获取时间戳。

         `signingHashAlgorithms`的值为`sha256`就从`rfc3161TimeStampServer`指定的服务器中获取时间戳。

         在[electron-builder](https://github.com/electron-userland/electron-builder)/[packages](https://github.com/electron-userland/electron-builder/tree/master/packages)/[app-builder-lib](https://github.com/electron-userland/electron-builder/tree/master/packages/app-builder-lib)/[src](https://github.com/electron-userland/electron-builder/tree/master/packages/app-builder-lib/src)/[codeSign](https://github.com/electron-userland/electron-builder/tree/master/packages/app-builder-lib/src/codeSign)/[windowsCodeSign.ts](https://github.com/electron-userland/electron-builder/blob/master/packages/app-builder-lib/src/codeSign/windowsCodeSign.ts#L199)文件有判断。

         

