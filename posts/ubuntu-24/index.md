# Ubuntu20.04修复openweather


前天gnome的openweather插件突然无法更新
在gitlab找到了[解决方案](https://gitlab.com/jenslody/gnome-shell-extension-openweather/-/issues/272)
在这里记录一下方法
<!-- more-->

1. Download SectigoRSADomainValidationSecureServerCA.crt from Sectigo RSA Domain Validation Secure Server CA
2. Drop the file to /usr/local/share/ca-certificates
3. Run sudo update-ca-certificates
4. Restart shell Alt + F2, press r, press Enter.

下载网址: [Sectigo RSA Domain Validation Secure Server CA](https://support.sectigo.com/Com_KnowledgeDetailPage?Id=kA01N000000rfBO)

**ps:**
文件在页面的下面
可以在页面中搜索"SectigoRSADomainValidationSecureServerCA"
