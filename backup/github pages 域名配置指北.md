先进入 个人设置 > Pages 里面添加域名 Add a domain

![图片](https://github.com/user-attachments/assets/8ec6662d-da0c-4a7e-9150-74f89ce12446)

进入 `DNS 解析` 里面添加 `TXT记录` 来 Verified 验证

如果我们要给多个仓库记录不同的第三方二级域名，可以使用

![图片](https://github.com/user-attachments/assets/638a02b9-eeef-4978-99fd-08ab7ff6006b)
![图片](https://github.com/user-attachments/assets/002c6704-aaa7-4720-bedd-cc4fd6e43c97)
![图片](https://github.com/user-attachments/assets/0ed2c104-4928-4b55-bf8a-9fa760e91180)

先在 Apex 顶级域 上面用 `A` `AAAA` 记录上 github 的 IPV4 和 IPV6

然后在 CNAME记录上 `bemly.github.io` 等待 仓库的 Github Pages 验证 DNS 完成即可

![图片](https://github.com/user-attachments/assets/26410e69-d26c-4610-937f-1734ed512fd8)




### 链接

https://docs.github.com/zh/pages/configuring-a-custom-domain-for-your-github-pages-site/about-custom-domains-and-github-pages