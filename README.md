> 提醒： 滥用可能导致账户被BAN！！！   
  
* 使用[xray](https://github.com/XTLS/Xray-core)+caddy同时部署通过ws传输的vmess vless trojan shadowsocks socks等协议  
* 支持tor网络，且可通过自定义网络配置文件启动xray和caddy来按需配置各种功能  
* 支持存储自定义文件,目录及账号密码均为AUUID,客户端务必使用TLS连接  
  
[![Deploy](https://www.herokucdn.com/deploy/button.png)](https://dashboard.heroku.com/new?template=https://github.com/rewdaw/xray-web)  
  
### 服务端
点击上面紫色`Deploy to Heroku`，会跳转到heroku app创建页面，填上app的名字、选择节点、按需修改部分参数和AUUID后点击下面deploy创建app即可开始部署  
如出现错误，可以多尝试几次，待部署完成后页面底部会显示Your app was successfully deployed  
  * 点击Manage App可在Settings下的Config Vars项**查看和重新设置参数**  
  * 点击Open app跳转[欢迎页面](/etc/CADDYIndexPage.md)域名即为heroku分配域名，格式为`appname.herokuapp.com`，用于客户端  
  * 默认协议密码为$UUID，WS路径为$UUID-[vmess|vless|trojan|ss|socks]格式
  
### 客户端
* **务必替换所有的appname.herokuapp.com为heroku分配的项目域名**  
* **务必替换所有的8f91b6a0-e8ee-11ea-adc1-0242ac120002为部署时设置的AUUID**  
  
<details>
<summary>xray</summary>

```bash
* 客户端下载：https://github.com/XTLS/Xray-core/releases
* 代理协议：vless 或 vmess
* 地址：appname.herokuapp.com
* 端口：443
* 默认UUID：8f91b6a0-e8ee-11ea-adc1-0242ac120002
* 加密：none
* 传输协议：ws
* 伪装类型：none
* 路径：/8f91b6a0-e8ee-11ea-adc1-0242ac120002-vless // 默认vless使用/$uuid-vless，vmess使用/$uuid-vmess
* 底层传输安全：tls
```
</details>
  
<details>
<summary>trojan-go</summary>

```bash
* 客户端下载: https://github.com/p4gefau1t/trojan-go/releases
{
    "run_type": "client",
    "local_addr": "127.0.0.1",
    "local_port": 1080,
    "remote_addr": "appname.herokuapp.com",
    "remote_port": 443,
    "password": [
        "8f91b6a0-e8ee-11ea-adc1-0242ac120002"
    ],
    "websocket": {
        "enabled": true,
        "path": "/8f91b6a0-e8ee-11ea-adc1-0242ac120002-trojan",
        "host": "appname.herokuapp.com"
    }
}
```
</details>
  
<details>
<summary>shadowsocks</summary>

```bash
* 客户端下载：https://github.com/shadowsocks/shadowsocks-windows/releases/
* 服务器地址: appname.herokuapp.com
* 端口: 443
* 密码：password
* 加密：chacha20-ietf-poly1305
* 插件程序：xray-plugin_windows_amd64.exe  //需将插件https://github.com/shadowsocks/xray-plugin/releases下载解压后放至shadowsocks同目录
* 插件选项: tls;host=appname.herokuapp.com;path=/8f91b6a0-e8ee-11ea-adc1-0242ac120002-ss
```
</details>
  
<details>
<summary>cloudflare workers example</summary>

```js
const SingleDay = 'appname.herokuapp.com'
const DoubleDay = 'appname.herokuapp.com'
addEventListener(
    "fetch",event => {
    
        let nd = new Date();
        if (nd.getDate()%2) {
            host = SingleDay
        } else {
            host = DoubleDay
        }
        
        let url=new URL(event.request.url);
        url.hostname=host;
        let request=new Request(url,event.request);
        event. respondWith(
            fetch(request)
        )
    }
)
```
</details>
  
> [更多来自热心网友PR的使用教程](/tutorial)
