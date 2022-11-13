# Google Earth Engine



**https://earthengine.google.com/**

首先需要申请一个账号





### Official Get-Started Tutorial

https://developers.google.com/earth-engine/guides/getstarted





## Online Developing Platform

https://code.earthengine.google.com/







## Handle with Python

https://developers.google.com/earth-engine/guides/python_install



```powershell
pip install earthengine-api
```

此外还要安装`gcloud`

```powershell
(New-Object Net.WebClient).DownloadFile("https://dl.google.com/dl/cloudsdk/channels/rapid/GoogleCloudSDKInstaller.exe", "$env:Temp\GoogleCloudSDKInstaller.exe")

& $env:Temp\GoogleCloudSDKInstaller.exe
```

参考: https://cloud.google.com/sdk/docs/install



然后就是激活

```python
ee.Authentiate()
```

也可以在cmd中激活, 但是这种方式容易出现`earthengine`没有被添加到`PATH`的情况

```powershell
earthengine authenticate
```



激活后的密钥放在`%UserProfile%\.config\earthengine\credentials`中. 如果想要反激活的话只需移除密钥文件即可

> 此处`%UserProfile%`为`C:\Users\wcrpk`



### Set Python Proxy

这一步是确保python的流量也走梯子, 以访问Google服务

```python
import os
os.environ['HTTP_PROXY'] = 'http://127.0.0.1:5555'
os.environ['HTTPS_PROXY'] = 'http://127.0.0.1:5555'
```

代理的地址和端口可以打开clash或者在Windows设置中的“代理”查看. 注意clash有随机端口模式, 可以关掉. 



### Initialization

```python
import ee
ee.Initialize()
```



![img](https://zhaoxuhui.top/assets/images/blog/content/2020-10-14-01.PNG)
