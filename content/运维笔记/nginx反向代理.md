## 目录说明

|名字|作用|日常操作建议|
|---|---|---|
|`conf.d/`|**通用附加配置** 目录，放任何 `*.conf` 都会被主配置 `include` 进来。|**推荐**把新站点或全局片段直接扔这里，省事。|
|`modules-available/`|**已安装但尚未启用的动态模块**（*.so）列表。|只读，不用手动碰。|
|`modules-enabled/`|**真正生效的动态模块** 软链，对应上一目录的文件。|`sudo ln -s ../modules-available/ngx_http_geoip2.conf` 启用模块。|
|`sites-available/`|**虚拟主机草稿区** ，写了不一定会被加载。|建站点先写这里，再软链到 `sites-enabled/`。|
|`sites-enabled/`|**已启用虚拟主机** 软链集合。|`ln -s ../sites-available/myapp.conf` 即上线；`rm` 即下线。|
|`snippets/`|**可复用的小片段**（SSL 参数、安全头、反代头等）。|把常用配置拆成 snippet，然后在虚拟主机里 `include snippets/ssl-params.conf;`|

|名字|作用|是否可改|
|---|---|---|
|`nginx.conf`|**全局主配置**；决定 worker 数、日志格式、全局 `include` 等。|**谨慎改**，改前先备份。|
|`mime.types`|文件扩展名 → Content-Type 映射表。|一般不动。|
|`fastcgi_params` / `fastcgi.conf`|FastCGI 变量集合；后者多一个 `SCRIPT_FILENAME`。|按需 `include`，通常不动。|
|`proxy_params`|反向代理常用变量集合。|同上，直接 `include proxy_params;` 即可。|
|`scgi_params` / `uwsgi_params`|SCGI / uWSGI 协议变量集合。|几乎不用改。|
|`koi-utf` / `koi-win` / `win-utf`|早期俄语/Windows 字符集映射文件。|历史遗留，现代环境可忽略。|

## 相关命令
```
# 立即启动  
sudo systemctl start nginx   
 #开机自启  
sudo systemctl enable nginx  
#优雅重载（改配置不中断）  
sudo nginx -s reload   
sudo systemctl reload nginx   
# 快速停止  
sudo nginx -s stop   
sudo systemctl stop nginx   
#优雅退出(处理完当前请求再停)  
sudo nginx -s quit  
# 语法检查  
sudo nginx -t     
# 显示版本/编译参数  
nginx -v / nginx -V  
​  
## 编辑文件  
sudo nano 文件路径  ## 例如 sudo nano /etc/nginx/nginx.conf

### 新增站点

# linux（Ubuntu）系统内输入  
sudo nano /etc/nginx/conf.d/站点名称.conf

# 新增配置文件  
server {  
    listen       80;  
    server_name  example.com;          # ← 你的域名  
​  
    location / {  
        proxy_pass         http://192.168.10.25;   # ← 目标 IP（可带端口 192.168.10.25:8080）  
        proxy_set_header   Host $host;  
        proxy_set_header   X-Real-IP $remote_addr;  
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;  
        proxy_set_header   X-Forwarded-Proto $scheme;  
    }  
}

# 检查语法  
sudo nginx -t   
​  
# 重新加载配置文件  
sudo systemctl reload nginx

### 站点改名

# 新进入目录在改名  
cd /etc/nginx/conf.d  
sudo mv inspection.conf inspection.bak  
sudo mv zjzt.conf        xxzt.conf  
​  
# 更新后 重新加载配置文件  
sudo nginx -t && sudo systemctl reload nginx
```

