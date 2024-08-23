包上报错查找的安装时长：2个小时（服了） 写文章：半个小时

官网拔下来压缩包然后unzip

开始非常顺利昂 后面不讲武德昂 
```
sudo chmod +x ./DaVinci_Resolve_19.0b6_Linux.run
./DaVinci_Resolve_19.0b6_Linux.run
```
草 然后就悲剧了（原因就是pkexec这个superuser-agent大神)

```
==== AUTHENTICATING FOR org.freedesktop.policykit.exec ====
Authentication is needed to run `/usr/bin/env PATH=/home/bemly/.cargo/b ... mp/.mount_DaVincYTYBgI /tmp/pDCdqp.sh' as the super user
Authenticating as: bemly
Password: 
==== AUTHENTICATION FAILED ====
Error: Installer failed
 polkit-agent-helper-1: error response to PolicyKit daemon: GDBus.Error:org.freedesktop.PolicyKit1.Error.Failed: No session for cookie
Error executing command as another user: Not authorized
```
我明明密码都输入对了的 可恶

看上去是pkexec的daemon守护进程有点问题昂 按照惯例我就去重启这个服务

```
systemctl restart polkit.service
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ====
重新启动“polkit.service”需要认证。
Authenticating as: bemly
Password: 
==== AUTHENTICATION COMPLETE ====
```
草怎么这里验证完成了噗 看了下status也是没有任何的问题

好端端的然后重复上面的操作 啊啊啊啊 怎么还算FAILED

stack overflow启动！然后上面说要X11的DE桌面环境

我这个只有hyprland混成器的遂放弃了修复pkexec的这条路

看了下/tmp/pDCdqp.sh，里面全是uninstall的脚本，后面的没有输出看不了，遂放弃tmp这条路

转向研究官方的这个.run文件

https://hksanduo.github.io/2020/05/28/2020-05-28-crack-run-program/

跟着大佬的教程我用 https://github.com/WerWolv/ImHex 一看 这不对吧
![图片](https://github.com/user-attachments/assets/38dd2214-0d57-4b0f-827b-51c9d1e4f2df)

我大shell脚本的shebang行怎么没了（shebang：#!/bin/fish）

看到ELF的标识符就知道.run这玩意是二进制的可执行文件了，嗐

然后就找是哪种封装的，先用file扫一下

```
mkdir chk_tye | cd chk_tye
cp $(command -v bash) ./
touch install.sh
zeditor install.sh | bat install.sh

───────┬────────────────────────────────────────────────────
       │ File: install.sh
───────┼────────────────────────────────────────────────────
   1   │ #!/bin/bash
   2   │ ./bash
   3   │ exit 0
───────┴────────────────────────────────────────────────────

cat install.sh bash > myrun.run
sudo chmod +777 myrun.run
sh myrun.run
./myrun.run
```
![图片](https://github.com/user-attachments/assets/bcd78510-0ff2-4cba-aa71-f1aa6ab232c2)

这玩意.run类型 不一样 气死了www

用imhex看了好半天，原来是appimage格式的草
```
./DaVinci_Resolve_19.0b6_Linux.run --appimage-extract 2>&1 > output.txt
./DaVinci_Resolve_19.0b6_Linux.run -h
```
先按照惯例看看appimage的格式，squashfs-root文件看了下

直接Root环境下面 QT有问题，无法运行安装程序
```

 bemly  bemlyEOS ~/davinci  sudo ./DaVinci_Resolve_19.0b6_Linux.run -h                                  ✔   29s  15:58:36 
egrep: warning: egrep is obsolescent; using grep -E
egrep: warning: egrep is obsolescent; using grep -E
egrep: warning: egrep is obsolescent; using grep -E
Authorization required, but no authorization protocol specified

qt.qpa.xcb: could not connect to display :0
qt.qpa.plugin: Could not load the Qt platform plugin "xcb" in "" even though it was found.
This application failed to start because no Qt platform plugin could be initialized. Reinstalling the application may fix this problem.

Available platform plugins are: linuxfb, minimal, offscreen, xcb.

/tmp/.mount_DaVincbYiBL6/AppRun: 第 348 行：75076 已中止               （核心已转储）"${CURRENT_DIR}/installer" "${CURRENT_DIR}" "$@"
```
诶，办法这不就来了，nonroot不能复制一些特权文件和创建链接，所以解决pkexec的痛点就行了

众所周知pkexec在root态就不用再验证了，有agent加持图形化提权也不会中断，然后就是套娃
```
sudo fakeroot pkexec
[sudo] bemly 的密码：
[root@bemlyEOS ~]# /home/bemly/davinci/DaVinci_Resolve_19.0b6_Linux.run -h
```
![图片](https://github.com/user-attachments/assets/15b9a787-adcf-489e-afa9-40bf4e1bb979)

```
./DaVinci_Resolve_19.0b6_Linux.run -iay
```

安装就ok了

然后搞笑的就来了
![图片](https://github.com/user-attachments/assets/020c44a0-66d2-49ca-93a5-476949880de5)

麻了 ，不过还好，是默认lib32的库把达芬奇害了，给他指定lib64就行
https://bbs.archlinux.org/viewtopic.php?id=295687

```
sudo cp /usr/lib/libgio-2.0.so /opt/resolve/libs/ 
sudo cp /usr/lib/libgmodule-2.0.so /opt/resolve/libs/ 
sudo cp /usr/lib64/libglib-2.0.so /opt/resolve/libs/ 

/opt/resolve/bin/resolve
```

![图片](https://github.com/user-attachments/assets/7934312d-ee4a-4c14-86b0-1347f3ca167c)

诶！完美打开（（（（

目前达芬奇作为linux最优秀的剪辑软件，暂时还是x11 xwayland支持，再等等看（

火狐原生支持了wayland是我没想到的