## 使用方式

1. 下载WinSW
    
    1. 下载地址：[https://github.com/winsw/winsw/releases](https://github.com/winsw/winsw/releases)
        
2. 新建XML文件
    
    <service>  
      <!-- 设置服务id -->  
      <id>yudao-8001</id>  
      <!-- 设置服务名称，一般和id一样 -->  
      <name>yudao-8001</name>  
      <!-- 服务说明、备注 -->  
      <description>这是芋道后端API服务；端口：8001</description>  
      <!-- 指定执行的命令，这里使用java命令 -->  
      <executable>java</executable>  
      <!-- 传递给java命令的参数，包括JAR文件路径 -->  
      <arguments>java -jar D:\wwwroot\code\yudao\8001\yudao-8001.jar</arguments>  
      <!-- 设置服务启动模式为自动 -->  
      <startmode>Automatic</startmode>  
      <!-- 指定日志文件的存储路径 -->  
      <logpath>D:\wwwroot\code\yudao\8001\logs</logpath>  
      <!-- 配置日志滚动模式为按大小滚动 -->  
      <log mode="roll-by-size">  
        <!-- 日志文件大小阈值，超过此值时滚动日志文件 -->  
        <sizeThreshold>10240</sizeThreshold>  
        <!-- 保留的日志文件数量 -->  
        <keepFiles>10</keepFiles>  
      </log>  
      <!-- 设置服务停止超时时间为15秒 -->  
      <stoptimeout>15 sec</stoptimeout>  
      <!-- 设置是否在停止子进程之前终止父进程，默认为true -->  
      <stopparentprocessfirst>true</stopparentprocessfirst>  
      <!-- 指定多长时间内服务应该对SetServiceStatus函数进行下一次调用，否则会被标记为无响应，默认为15秒 -->  
      <waithint>15 sec</waithint>  
      <!-- 服务两次调用SetServiceStatus函数的间隔时间，默认为1秒 -->  
      <sleeptime>1 sec</sleeptime>  
      <!-- 配置服务启动失败后的重试策略 -->  
      <onfailure action="restart" delay="10 sec"/>  
      <onfailure action="restart" delay="20 sec"/>  
      <onfailure action="none"/>  
      <!-- 重置失败计数的时间间隔，默认为1小时 -->  
      <resetfailure>1 hour</resetfailure>  
    </service>
    
3. 说明
    
    1. 将winSW.exe和jar包以及xml 放置同一个目录；
        
    2. 改成相同名称
        
    3. 新建logs文件目录
        
    4. 如图：
        
        ![[Pasted image 20260303171232.png]]
        
    5. XML内容说明
        
        ![[Pasted image 20260303171245.png]]
        

## Q&A

1. 当程序内容更新后如何去更新服务呢？
    
    1. 如果没有下载依赖、可以直接停止服务、替换JAR包、启动服务；
        
    2. 如果有新依赖或上述方式失败更推荐下述方式：
        
        1. 停止服务、卸载服务
            
        2. 替换Jar包
            
        3. 安装服务、启动服务
            
        4. 自此、完成整个流程的程序服务更新；
            

## 示例

### 准备

1. winsw.exe ---> 服务注册软件
    
2. [服务名称].jar ---> 打包的jar或exe应用
    
3. [服务名称].xml ---> winsw注册服务配置
    

### 开始

1. 需要注册服务的根目录输入CMD 回车;
    
2. 输入安装命令：`[服务名称].exe install`
    
3. 按下 `Win + R` 组合键，打开“运行”对话框；
    
4. 在命令提示符窗口中，输入 `services.msc` 命令，然后按下回车键；即可打开服务界面；
    
    ![[Pasted image 20260303171254.png]]
    
5. 输入启动服务命令`[服务名称].exe start`
    
    ![[Pasted image 20260303171304.png]]![[Pasted image 20260303171323.png]]
    
6. 打开网址验证：
    
    ![[Pasted image 20260303171339.png]]
    

## 相关命令

### CMD打开服务管理界面

1. Win+R 输入 CMD 回车；
    
2. 输入 services.msc 回车；
    

### 安装服务

`[服务名称].exe install`

### 启动服务

`[服务名称].exe start`

### 停止服务

`[服务名称].exe stop`

### 卸载服务

`[服务名称].exe uninstall`

### 查看服务状态

`sc query [服务名称]`

### 删除服务

`sc delete [服务名称]`