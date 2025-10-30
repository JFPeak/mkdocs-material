# 垃圾节点通过CF反代实现提速的方法
## 参考链接🔗：
1. [https://www.bulianglin.com/archives/newcdn.html](https://www.bulianglin.com/archives/newcdn.html)
2. youtube播放地址：[https://youtu.be/NbruiJShUCE](https://youtu.be/NbruiJShUCE)
3. 相关youtube教程：

https://youtu.be/r2WunEyqMeQ	

https://youtu.be/Azj8-1rdF-o

https://youtu.be/x6B5JEwXSEg

https://youtu.be/uKXVXaa5_YI

https://youtu.be/fHJDvJIptts

## 用到的工具
CDN优选工具（网页）：[https://bulianglin.com/archives/cdn.html](https://bulianglin.com/archives/cdn.html)

节点测速工具：[https://github.com/bulianglin/demo/blob/main/nodesCatch-V2.0.rar](https://github.com/bulianglin/demo/blob/main/nodesCatch-V2.0.rar)

搜索引擎：[https://fofa.info](https://fofa.info)

临时邮箱：[http://24mail.chacuo.net](http://24mail.chacuo.net)

cloudflare：[https://www.cloudflare-cn.com/](https://www.cloudflare-cn.com/)

免费域名（us.kg）申请：[https://www.linyafeng.com/archives/2147/](https://www.linyafeng.com/archives/2147/)

> 相关工具已下载到百度云盘，所在目录: 全部文件>工作&学习>翻


## 安装X-UI
```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

用户名/密码：`nucleu` /   `N3yyWbrSqf`  
面板端口：`3877`

## X-UI 基础设置
x-ui 面板支持这些协议：vmess、vless、trojan、shadowsocks、dokodemo-door、socks、http等。

x-ui 面板 > 入站列表 > “+” 号

protocol 选择vmss ；传输选ws，路径选择/asdf，其他一切默认即可。当然也可以设置 “到期时间” 或者是 “总流量” 的限制。

重要！这里配置的端口80，需要在云服务器的安全组放行，并且如果要用cf代理的话，必须选择Cloudflare 支持对外开放的端口。

```bash
#Cloudflare 支持的 HTTP 端口：
80
8080
8880
2052
2082
2086
2095
#Cloudflare 支持的 HTTPS 端口：
443
2053
2083
2087
2096
8443
#Cloudflare 支持的端口，但禁用了缓存：
2052
2053
2082
2083
2086
2087
2095
2096
8880
8443
```

PS:CloudFlare对外提供服务的端口，在企业用户开启使用大陆CDN节点的情况下（域名备案后），就只能使用80/443端口

## fofa的搜索语法
```bash
国内反代IP：server=="cloudflare" && port=="80" && header="Forbidden" && country=="CN"
剔除CF：asn!="13335" && asn!="209242"
阿里云：server=="cloudflare" && asn=="45102"
甲骨文韩国：server=="cloudflare" && asn=="31898" && country=="KR"
搬瓦工：server=="cloudflare" && asn=="25820"
```

## V2rayN各平台客户端
Windows（v2rayN）：[https://github.com/2dust/v2rayN/releases/](https://github.com/2dust/v2rayN/releases/)

Android（v2rayNG）：[https://github.com/2dust/v2rayNG/releases/](https://github.com/2dust/v2rayNG/releases/)

IOS（shadowrocket）：https://apps.apple.com/app/shadowrocket/id932747118

## 改进/补充：
1. x-ray未运行，在面板设置中将`listen address`由`127.0.0.1`改成`0.0.0.0`
2. 建议每半年添加一批优选IP

## 优选CF IP的方法二
参考链接：

1. [https://github.com/byJoey/cfy](https://github.com/byJoey/cfy)
2. [https://www.youtube.com/watch?v=TVPDahF47oU&t=26s](https://www.youtube.com/watch?v=TVPDahF47oU&t=26s)

节点优选生成器 (cfy)，可快速生成优选VMess节点。

一个强大且易于使用的 Bash 脚本，用于批量生成基于 Cloudflare IP 的 vmess 节点链接。脚本会自动替换服务器地址，并可智能生成优选节点。

```bash
bash <(curl -l -s https://raw.githubusercontent.com/byJoey/cfy/main/cfy.sh)
```

生成模式选`云优选`，自动从第三方源抓取已优选的 IPv4 和 IPv6 地址。

卸载脚本: 只需删除安装好的文件即可。

```bash
sudo rm /usr/local/bin/cfy
```

如果优选出来的节点不可用，那就只用优选的IP地址，用上面不良林的[web工具](https://bulianglin.com/archives/cdn.html)进行节点修改后再导入v2ray里面。（原始节点要能用的）

## 优选CF IP的方法三
[https://v2rayssr.com/cfip/](https://v2rayssr.com/cfip/)  
这个网站实时更新不同运营商线路的CloudFlare优选IP，用上面不良林的[web工具](https://bulianglin.com/archives/cdn.html)进行节点后再导入v2ray里面。（原始节点要能用的）

