明明 `NVENC` 还是 `AMDGPU` `VAAPI`，都已经装上了

打开 `about:support` 一看，却总是没有硬件解码视频，太不优雅了\
这是因为 Firefox 为 Wayland 兼容性考虑索性直接 Block 了 N卡 的硬解

![2024-09-06-005251_hyprshot](https://github.com/user-attachments/assets/6b84011b-6bca-4e1c-b1af-b6aeaa930372)

直接在 `地址栏` 上输入 `about:config`

![2024-09-06-005406_hyprshot](https://github.com/user-attachments/assets/48ab6aec-94de-46cc-bdf4-bb26ffc49e4e)

无视风险继续启动，输入下图 `media.hardware-video-decoding.force-enabled` 改为 `true` 来强制硬解

    gfx.webrender.all=true
    gfx.webrender.compositor=true
    gfx.webrender.compositor.force-enabled=true
    
    media.ffmpeg.vaapi.enabled=true
    media.hardware-video-decoding.force-enabled=true

![image](https://github.com/user-attachments/assets/871ee6da-f127-4034-a69b-8e9481b2cdb5)

重启一下 Firefox，播放一下视频进入 `about:support` 就可以看到已经启用硬件解码了：

![2024-09-06-011332_hyprshot](https://github.com/user-attachments/assets/5d301c74-f4dc-4c3d-811a-f4c1aab69c56)

前提是要先随便播放一下视频来激活一下 `Video Codec`，不然支持页面这边很可能出现一些奇奇怪怪的错误，比如这样（

![2024-09-06-010214_hyprshot](https://github.com/user-attachments/assets/782cd26b-39c5-4170-bcd4-ae26ebb3a781)

BTW, 最近 Firefox 的音频解码后端也换成了 pulse-rust ，\
如果还是没有正确切换到硬件解码，需要检查 编解码器 是否激活

![image](https://github.com/user-attachments/assets/00419dd0-318b-4d9f-bcfd-98be213bd540)

这里 我们的 闭源 NVENC 驱动就可以看到被正确识别且被激活，

    BTW，AzureCanvas 用的是 谷歌收购的 Skia 图形 API，和 微软的d2d mesa 类似，性能更好，avalonia也在使用 Skia 图形库

然后下方 Wayland 的运行时硬解给我们拉入了黑名单，不用担心，这里我们 `about:config` 的 `user` 优先级高于 `runtime`，具体调试，我们可以导入环境变量激活 Firefox 的调试信息显示来查看更多详细信息：

    MOZ_LOG="PlatformDecoderModule:5" firefox
![image](https://github.com/user-attachments/assets/8769aae8-c06f-48a9-ad41-8f792c2d9a52)

### 引用

1. https://blog.zhaozuohong.vip/2022/11/16/firefox-vaapi/
  - 推荐用 `btop` 代替 `htop` 更直观
2. https://blog.bgme.me/posts/2023/how-to-enable-firefox-hardware-video-acceleration-on-linux-for-amd-gpu/
  - 目前 N卡 正在极力删除 VAAPI，建议迁移到 NVENC，或者降级驱动到 550 继续使用，详细 Arch wiki
