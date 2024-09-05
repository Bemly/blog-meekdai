### 选择

首先选择一个合适的 SSL 白嫖网站，市面上的付费 SSL 先略过：

- [Comodo](https://www.comodo.com/)
- [腾讯云](https://cloud.tencent.com/product/ssl)
- [阿里云](https://www.aliyun.com/product/cas)
- [Global Sign](https://www.globalsign.com)
- [Digicert](https://www.digicert.com/) *付费常用
- [moe SSL](https://get.ssl.moe) *SakuraFrp 推荐的ACGN味特浓的证书

下面是一些提供免费 SSL 注册的服务商，因为Let's Encrypt白嫖怪众多，目前信任链会在 今年 9 月左右废弃。

1. [Let's Encrypt](https://letsencrypt.org/)
2. [Google Trust Service](https://pki.goog/) *只支持`ASNII`的域名，中文什么的不行
3. [CloudFlare](https://cloudflare.com) *上面两者的证书整合商
4. [Zero SSL](https://zerossl.com/) *泛域名需付费
5. [亚洲诚信](https://freessl.cn/)

### CF

这里推荐用 Cloudflare 赛博菩萨的 15 年证书，而且泛二级域名也可以随便白嫖\
因为cf现在是申请的前两者的证书，这里就不介绍前两者证书的其他官方申请方式了：

![图片](https://github.com/user-attachments/assets/c029d13c-5fc7-47c3-be58-881138b9732e)

目前 cf 有几种证书的选择模式，详细可以根据下图来进行选择：

![图片](https://github.com/user-attachments/assets/26fff829-58b5-4bbf-9d0c-1b5e02699e99)


![图片](https://github.com/user-attachments/assets/8f1f0e98-e696-4c92-aa7c-1fcd086293d4)

需要先把域名用 `CF DNS` 代理，然后在 dashboard 面板里面找到 `SSL/TLS` 就可以生成证书了

目前证书的生成有LetEnc和google两个渠道申请

![图片](https://github.com/user-attachments/assets/1e6daf93-209d-4370-bd50-1bda6f151ad4)
![图片](https://github.com/user-attachments/assets/d844ace9-bc41-446d-a73f-604c9e589eec)

最后把生成出来的 SSL PEM 证书导出，放进 Nginx、frp 或者其他需要 SSL 隧道的服务即可

![图片](https://github.com/user-attachments/assets/622fb453-3222-415c-abe4-52254fe27a47)

### ZeroSSL

生成方式和 `Let's` 大差不差

![图片](https://github.com/user-attachments/assets/47a7c238-712b-4129-a9a4-95e33f3381f2)

选择好需要添加 SSL 证书的网址，然后登录帐号就可以申请了

![2024-09-05-162317_hyprshot](https://github.com/user-attachments/assets/74c3dcfa-129d-4a5b-8c27-f4cdd0fa5bc3)

这里泛域名不支持免费白嫖，有邮箱、CNAME验证等三种验证方法，这里选择类似`github`的`TXT记录`进行验证\
注意验证的时候需要把 DNS 加速给关掉，不然无法被正确识别

![图片](https://github.com/user-attachments/assets/f0c0dd3f-67d5-4d15-b630-f79946ac4cab)
![2024-09-05-162418_hyprshot](https://github.com/user-attachments/assets/55e77ca7-2818-4c2a-9455-228fa23f234f)

等待小会儿，选择对应服务的证书下载，部署到隧道里就大功告成了，如果使用`frp`来代理`HTTPS 隧道`选择`NGINX`证书下载即可

![2024-09-05-161921_hyprshot](https://github.com/user-attachments/assets/b31f7b42-68e1-4586-82e9-49e0f98bcdc8)

### TrustAsia 亚洲诚信

https://freessl.cn/ 国内的证书颁发平台，使用方法同理

![图片](https://github.com/user-attachments/assets/cebad775-2598-45f9-8640-9e181c8ead9f)

注册完还需要手机号实名认证

![图片](https://github.com/user-attachments/assets/b7ad342d-e9b9-4056-b792-285f00d4c4d1)
![图片](https://github.com/user-attachments/assets/a9dd1718-68a6-4fde-b723-73e0977d965c)


### 鸣谢

1. https://blog.skyju.cc/post/google-cert-acme/
2. https://blog.laoda.de/archives/try-cloudflare-free-15-year-ssl-certificate
3. http://101.43.69.210/index.php/archives/440/
4. https://doc.natfrp.com/frpc/ssl.html#get-cert
5. https://ffis.me/archives/2110.html
