### 序言

详细写在了 MDN 的文档上面：\
https://github.com/mdn/content/pull/35442#issue-2462764033

为了减少 Theora 安全性方面的维护，谷歌正式移除了 Theora 解码器

![2024-09-06-011332_hyprshot](https://github.com/user-attachments/assets/d492f194-3bf6-469b-8fa3-e96e01ed5c33)

而 Firefox 在一年后也跟进正式移除 Theora 格式的解码，官方表示可以使用基于 `WASM` 技术的 [`ogv.js`](https://github.com/bvibber/ogv.js) 来代替 `native` 原生解码

![2024-09-06-005251_hyprshot](https://github.com/user-attachments/assets/b6ac2c99-4222-4ac1-926c-40d817e4c5b1)

Theora 视频文件 OGG 随着 130 版本的正式推送，在今天被正式移除了\
https://bugzilla.mozilla.org/show_bug.cgi?id=1860492

### 正文

身为老残党，不能不放下这门被 `WIKI` 维基百科使用多年的 `ogv/ogg` 视频格式，那就降级 Firefox 到 129

这里介绍一下几种降级软件的方法

#### Arch Pacman 软件包 降级

先用`pacman -S downgrade` `yay -S downgrade` `paru -S downgrade`等等安装上 `downgrade`，毕竟`pacman -U`大法还是太繁琐了，直接交给软件来处理，对着想要删除的软件源

![image](https://github.com/user-attachments/assets/8168e093-2d2a-4510-aa55-ba27d0b2fb54)

*注意：火狐降级需要重新配置配置文件！*

#### 大部分发行版 Flatpak 软件包 降级

    flatpak list --app # 查找降级软件包对应包名
    flatpak remote-info --log flathub <包名> # 替换包名为你想要降级的软件包名

![160403so7tjrjrfowr772k](https://github.com/user-attachments/assets/4e7d5323-3edb-43f9-b5bd-ac07d3fb5394)

    sudo flatpak update --commit=<commit_code> <包名>

即降级成功

#### 安装 ESR 版本，无需降级

别看今天闹的欢，小心今后拉清单昂（\
现在 ESR 还在 `128`，随便耐造

#### 下载官网的 tar.gz2 压缩包，手动安装

https://ftp.mozilla.org/pub/firefox/releases/

选择一个适合自己的版本，解压，直接运行就行（类似便携版，配置文件和软件在同一路径下）


#### 使用打包好的对应版本.AppImage

https://github.com/srevinsaju/Firefox-Appimage/releases

（不推荐，占环境，不如docker、podman处理了）

### 鸣谢

1. https://support.mozilla.org/zh-CN/kb/%E5%AE%89%E8%A3%85%20Firefox%20%E4%BB%A5%E5%89%8D%E7%9A%84%E7%89%88%E6%9C%AC
2. https://www.cnblogs.com/DarkH/p/16298668.html
3. https://linux.cn/article-9730-1.html
4. https://wiki.archlinuxcn.org/wiki/%E9%99%8D%E7%BA%A7%E8%BD%AF%E4%BB%B6%E5%8C%85
5. https://linux.cn/article-15402-1.html