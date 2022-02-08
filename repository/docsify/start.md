# docsify部署与使用
## 阿里云服务器部署docsify
### 配置yum源
1. 将本地的yum.repo文件移动至bak目录
2. 下载阿里云centos8yum仓库
```
# wget
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-8.repo
```
3. 由于CentOS 8 将在2021年底结束维护，接班的正是 CentOS Stream，如果下载的repo文件中的baseurl为https://mirrors.aliyun.com/centos/$releasever/BaseOS/$basearch/os/，需要将$releasever替换为8-stream
```
sed -i s/\$releasever/8-stream/g CentOS-Base.repo
```

### 下载npm
```
yum install npm
```

### 下载docsify客户端
```
npm i docsify-cli -g
``` 
### 初始化
```
docsify init ./docs
```
初始化后会在docs下生成三个文件：
- index.html as the entry file
- README.md as the home page
- .nojekyll prevents GitHub Pages from ignoring files that begin with an underscore

## docsify的使用
### 新增侧边栏
首先将侧边栏的开关打开，vim index.html, 添加loadSidebar: true
```
<!-- index.html -->

<script>
  window.$docsify = {
    loadSidebar: true
  }
</script>
<script src="//cdn.jsdelivr.net/npm/docsify/lib/docsify.min.js"></script>
```
创建_sidebar.md, 后续的侧边栏目录均在此文件中添加，示例：
```
<!-- docs/_sidebar.md -->

* [Home](/)
* [Guide](guide.md "The greatest guide in the world")
* [docsify](docsify/start.md "init")
* * [docsify部署](docsify/start.md "init")
```

## 配置nginx(可选)
下载nginx
```
yum install nginx
```
在nginx配置文件目录conf.d中添加docsify配置文件，docsify_repository即上文中的docs目录，我自己当时创建的目录是docsify_repository
```
server{
        listen 8080;
        server_name localhost;

        location / {
                root /home/docsify_repository;
                index index.html index.htm;
        }
}
```
重新加载nginx配置
```
nginx -s reload
```
接下来就可以在浏览器输入ip:8080访问自己的docsify空间了。
ps:阿里云服务器记得在安全组添加端口开发策略，以及在自己的服务器添加对应的端口开放
```
firewall-cmd --add-port=8080/tcp --permanent
```

