# GitHub的一百个tip

##### 一、提高github访问速度——修改本机DNS

1. 获取github网站的真实ip地址
   查找TTL值较小的ip地址
2. 修改本机的host文件
   打开“C:\Windows\System32\drivers\etc”，使用记事本修改“hosts”文件
   将“[复制得到的最小的TLL值的ip地址填入] www.github.com” 写入hosts文件中
   <img src="D:\洪泽毫\大三\MyLearningNotesBook\img\github1.jpg" alt="github1" style="zoom: 33%;" />
3. 刷新电脑dns缓存
   打开cmd.exe程序，输入“ipconfig/flushdns”
   <img src="D:\洪泽毫\大三\MyLearningNotesBook\img\github2.jpg" alt="github2" style="zoom:33%;" />
4. 浏览器访问“www.github.com”

##### 二、Git出现 SSL certificate problem: unable to get local issuer certificate...

1. 这是由于当你通过HTTPS访问Git远程仓库的时候，如果服务器上的SSL证书未经过第三方机构认证，git就会报错。原因是因为未知的没有签署过的证书意味着可能存在很大的风险。解决办法就是通过下面的命令将git中的sslverify关掉：

   ```markd
   git config --global http.sslverify false
   ```

2. 上面这行命令的影响范围是系统当前用户，如果要设置为全局所有用户，可以改成这样：

   ```markdown
   git config --system http.sslverify false
   ```

3. 如果只是想针对当前仓库进行设置，可以在需要修改的仓库目录下执行：

   ```markdown
   git config http.sslverify false
   ```

4. 如果你的仓库中存在嵌套的git子模块（就是子模块中又引用了子模块），在进行初始化时，仍然有可能遇到self signed certificate in certificate chain的错误，此时可以通过执行下面的命令来解决：

   ```markdown
   npm config set strict-ssl false
   ```

   