【重要网址】
1.reachapi   https://community.teamviewer.com/Chinese/kb/articles/32318-%E8%BF%9C%E7%A8%8B%E8%AE%BF%E9%97%AEapi%E9%9B%86%E6%88%90%E6%8C%87%E5%8D%97#vendorexampleapp
2.控制台     https://login.teamviewer.com/
3.TV详解     https://www.evget.com/article/2019/09/09/32165.html
【Teamview】
App created successfully
Never share this information with someone you don't know or fully trust!
Client ID 445104-FhykGcpjzBVkngboComn
Client secret hqQNXGWaCwVrCFvNaBYW

令牌已成功创建。
切勿与任何您不认识或不完全信任的人共享该信息！
令牌
13149034-BNIJYP315AOAwgb0DfKe


令牌已成功创建。
切勿与任何您不认识或不完全信任的人共享该信息！
令牌
13189039-dcRS4FH31QwsUsRFKLz6

【Code】
UZlsobg3
SlmDG8ym
AmNAmytb
一、获取code
1.访问接口
https://webapi.teamviewer.com/api/v1/oauth2/authorize?
2.参数 
response_type=code&client_id=445104-FhykGcpjzBVkngboComn&display=popup&%26redirect_uri%3D=www.163.com
经客户确认授权，取得code（在浏览器弹出窗口中获得）

二、获取token
1.接口
~~~xml
  POST  https://webapi.teamviewer.com/api/v1/OAuth2/token
request data
{
  "code":"AmNAmytb", 
  "grant_type": 0,
  "client_id": "445104-FhykGcpjzBVkngboComn",
  "username": "chuanchao.xu@nanovision.com.cn",
  "password": "Nano@frh",
  "client_secret": "hqQNXGWaCwVrCFvNaBYW"
}
response data
{
    "access_token": "13197945-crPWfHa3GB1aQQ3GzgwK",
    "token_type": "bearer",
    "expires_in": 86400,
    "refresh_token": "12860917-azU9ooKXcNH23GJ7IKj4"
}
~~~
三、查询API
1.查询账户
GET https://webapi.teamviewer.com/api/v1/account  Script/User Token

2.查询用户
GET https://webapi.teamviewer.com/api/v1/users    Company Token

3.查询计算机
Get https://webapi.teamviewer.com/api/v1/devices  Script/User Token
4.查询联系人
Get https://webapi.teamviewer.com/api/v1/contacts Script/User Token



其他问题：
问题1：
我们是否有升级ubuntu的计划，因为：
https://community.teamviewer.com/Chinese/discussion/116165/ubuntu-16-04-%E4%BD%BF%E7%94%A8%E6%9C%8D%E5%8A%A1%E7%BB%88%E6%AD%A2%E5%85%AC%E5%91%8A
大家好，
我们在此宣布，自 2020 年 6 月 22 日起，TeamViewer 将不再支持 Ubuntu 16.04使用服务。
停止对 Ubuntu 16.04 的主动支持意味着：
• 不会实施特定于这些操作系统的更新或修复
• 比 TeamViewer 15.18 版本更新的 TeamViewer 版本将不兼容
• 在该操作系统上使用 TeamViewer 的情况下，TeamViewer 支持团队无法提供任何类型的支持
我们通常建议仅运行当前的操作系统和软件版本。 所以请确保更新到最新版本的 Ubuntu 和 TeamViewer。
要查看 TeamViewer 支持的操作系统的完整列表，请在此处查看我们的知识库文章：TeamViewer 支持的操作系统

2.对MRS的远程访问是否会超过3个并发？（技师或者Support是否有在mcs上访问MRS的需求）

