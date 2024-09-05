之前视频的补充，如何优雅的启用 不安全的 TLS 访问，最近 Windows Home Server 2011 的 IIS 遇到过 TLS 过低无法访问的问题

### Chrome
75版本以下直接使用\
87版本以下修改flags启用\
91版本以下修改注册表的谷歌政策（仅Windows）
| Registry Hive       | Registry Path                               | Value Name   | Value Type | Value  |
|---------------------|--------------------------------------------|--------------|------------|--------|
| HKEY_LOCAL_MACHINE  | Software\Policies\Google\Chrome            | SSLVersionMin| REG_SZ     | tls1   |
| HKEY_CURRENT_USER   | Software\Policies\Google\Chrome            | SSLVersionMin| REG_SZ     | tls1   |

![image](https://github.com/user-attachments/assets/252fbb3d-046b-4f00-82a0-b8f6dbd19ccf)

其他版本洗洗睡吧

### EdgeHTML's Edge/Internet Explorer

修改注册表：
HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\
HKEY_CURRENT_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Internet Settings\
修改 `SecureProtocols` 注册表项 `REG_DWORD` 值为下列表格所示即可
| Protocol | Decimal Value | Remarks |
|---|---|---|
| SSL 2.0 | 8 | Use only SSL 2.0 |
| SSL 3.0 | 32 | Use Only SSL 3.0 |
| SSL 2.0 + SSL 3.0 | 40 | Use SSL 2.0 and SSL 3.0 (8+32) |
| TLS 1.0 | 128 | Only use TLS 1.0 |
| TLS 1.1 | 512 | Only Use TLS 1.1 |
| TLS 1.2 | 2048 | Only Use TLS 1.2 |
| TLS 1.1 + TLS 1.2 | 2560 | Use TLS 1.1 and TLS 1.2 |

![image](https://github.com/user-attachments/assets/440e2edd-6fc8-4943-b74d-dcaa526f6ffe)

直接修改IE网络选项：
![image](https://github.com/user-attachments/assets/c61789d8-9899-42ce-b0cc-c07747e70d90)
![2024-09-06-030513_hyprshot](https://github.com/user-attachments/assets/14603b08-9129-4cb7-a220-4c3ed833f462)

### Firefox

至今版本有效：\
进入 `about:config` 修改 `security.tls.version.min` 为 `1`

![image](https://github.com/user-attachments/assets/c05ec853-9376-4039-bb74-03cd3bdd9e78)
![2024-09-06-030347_hyprshot](https://github.com/user-attachments/assets/f4cb9f58-ccfc-4ed0-9b32-84b1e5e0639d)

### 手机同理

![image](https://github.com/user-attachments/assets/3bbd9941-c544-4419-ab84-33bf7c6f91e2)
![2024-09-06-030754_hyprshot](https://github.com/user-attachments/assets/d034c822-5c7f-4a7f-8706-d57e5616179a)
![image](https://github.com/user-attachments/assets/17349bf9-7046-4496-9a0a-150cec899594)


### 引用

1. https://www.cnblogs.com/jackadam/p/16207642.html
2. https://www.bilibili.com/video/BV1vt421L71d
3. https://admx.help/?Category=Chrome&Policy=Google.Policies.Chrome::SSLVersionMin&Language=zh-cn
4. https://techpress.net/disable-tls-1-0-and-tls-1-1-on-windows-10/