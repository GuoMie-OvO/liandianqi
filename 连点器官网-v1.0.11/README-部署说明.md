# 连点器下载官网部署说明

## 一、项目内容

这是一个无需数据库、无需后端的静态网站，包含：

- `index.html`：下载官网首页
- `privacy.html`：隐私政策
- `terms.html`：用户协议
- `disclaimer.html`：免责声明
- `assets/downloads/clicker-v1.0.11.apk`：正式 APK
- `CHECKSUMS.txt`：APK 的 SHA-256 校验值

请保持文件夹结构不变，否则图片、样式或下载链接可能失效。

## 二、本地预览

直接双击 `index.html` 即可预览。

也可以在本目录打开终端，运行：

```bash
python -m http.server 8080
```

然后在浏览器打开：

```text
http://127.0.0.1:8080
```

## 三、最简单的部署方式

将本文件夹中的全部文件和文件夹上传到网站根目录，例如 `public_html`、`wwwroot` 或 `htdocs`。上传完成后，确认域名访问时默认打开 `index.html`。

注意：不要只上传 `index.html`，必须同时上传 `assets` 文件夹和三个法律页面。

## 四、GitHub Pages 部署

1. 新建一个公开仓库，将本目录全部内容上传到仓库根目录。
2. 在仓库的 Pages 设置中选择从分支发布，并选择包含网站文件的分支和根目录。
3. 发布完成后，使用平台提供的网址访问。
4. 项目中已包含 `.nojekyll`，中文页面和静态资源可直接发布。

## 五、服务器 / Nginx 部署示例

将所有文件上传到 `/var/www/clicker-site`，Nginx 可使用类似配置：

```nginx
server {
    listen 80;
    server_name 你的域名;
    root /var/www/clicker-site;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~* \.apk$ {
        default_type application/vnd.android.package-archive;
        add_header Content-Disposition 'attachment';
    }
}
```

配置后检查语法并重载 Nginx。正式对外推广时建议启用 HTTPS。

## 六、更换 APK

1. 用新 APK 替换 `assets/downloads/clicker-v1.0.11.apk`。
2. 建议保留英文文件路径，避免部分服务器对中文 URL 兼容不佳。
3. 修改 `index.html` 中的版本号、文件大小、功能说明和下载链接。
4. 重新计算 SHA-256：

Windows PowerShell：

```powershell
Get-FileHash .\assets\downloads\clicker-v1.0.11.apk -Algorithm SHA256
```

macOS / Linux：

```bash
sha256sum assets/downloads/clicker-v1.0.11.apk
```

5. 同步更新首页校验值和 `CHECKSUMS.txt`。

## 七、更换联系方式或收款码

- 微信号在 `index.html` 中搜索 `19584503226` 后统一替换。
- 收款码文件：`assets/images/donate-wechat-original.jpg`
- 首页缩略图：`assets/images/donate-wechat.jpg`

更换收款码时，建议同时替换以上两个文件，并保持文件名不变。

## 八、上线前检查

- 电脑和手机均测试首页显示。
- 点击所有“立即下载”，确认能够下载 APK。
- 打开隐私政策、用户协议和免责声明。
- 测试微信号复制、SHA-256 复制和打赏弹窗。
- 扫描收款码确认收款账户正确。
- 确认服务器支持至少约 24 MB 的 APK 文件下载。
- 对外发布前备份 APK 签名文件，并确保后续版本使用相同签名。

当前 APK 信息：

- 版本：1.0.11
- 大小：23.16 MB（24,288,552 字节）
- SHA-256：`b3cbb664f3dc751ca727020022ddf12ae05dd7108b75890c93cb7840f6a3e6bf`
