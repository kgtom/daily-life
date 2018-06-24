---


---

<h3 id="必备软件工具">必备软件工具</h3>
<blockquote>
<p>Server OS：CentOS 7.3 服务端操作系统；<br>
Node.js：v6.14.3<br>
NPM:3.10.10<br>
Yarn：v1.6.0 用于替代 Npm，特点是速度比 Npm 快，提高效率，拉取的安装包的版本适用于生产环境使用，不容易产生 Bug。<br>
Ghost-CLI version: 1.8.1 用于管理 Ghost 的 CLI 工具<br>
Nginx version: nginx/1.14.0</p>
</blockquote>
<p><strong>安装前必读</strong><br>
使用Ghost-CLI 不能用root用户安装，所以需要先建立新用户，然后切换到这个用户，之后逐个安装Node 、NPM 、Ghost-CLI Nginx</p>
<h3 id="一、新建用户">一、新建用户</h3>
<p>添加一个常规用户、密码。</p>
<pre><code>[root@~]# useradd guser
[root@~]# passwd guser
</code></pre>
<p><strong>重点</strong><br>
为了使这个用户能够执行一些需要用到 root 权限的指令，可以通过 sudo 来完成，在 CentOS 系统中默认有一个用户组 wheel 就有这样的权限，只需要将刚才创建的用户添加到该组就可以了。</p>
<p>正在将用户“guser”加入到“wheel”组中</p>
<pre><code>[root@~]# gpasswd -a guser wheel
</code></pre>
<h3 id="二、安装-node--npm">二、安装 Node &amp; NPM</h3>
<p>不能忘记切换用户</p>
<pre><code>su - guser

</code></pre>
<p>如果安装官方文档来加入安装Node.js源，因为不可描述的原因，你会发现安装得很慢。所以这儿我推荐大家直接在仓库源安装吧：<br>
不过在安装之前你得需要查看Node.js版本是否符合官方的版本(V6)：</p>
<pre><code>yum info nodejs #Centos
sudo yum update
sudo yum install -y nodejs
</code></pre>
<h3 id="三、安装-yarn">三、安装 Yarn</h3>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ sudo yum install -y yarn
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Resolving Dependencies
--&gt; Running transaction check
---&gt; Package yarn.noarch 0:1.7.0-1 will be installed
--&gt; Finished Dependency Resolution

Dependencies Resolved

============================================================================
 Package         Arch              Version            Repository       Size
============================================================================
Installing:
 yarn            noarch            1.7.0-1            yarn            918 k

Transaction Summary
============================================================================
Install  1 Package

Total download size: 918 k
Installed size: 4.1 M
Downloading packages:
yarn-1.7.0-1.noarch.rpm                                | 918 kB   00:12
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : yarn-1.7.0-1.noarch                                      1/1
  Verifying  : yarn-1.7.0-1.noarch                                      1/1

Installed:
  yarn.noarch 0:1.7.0-1

Complete!
[guser@iz2ze71m2nxbuwib8avpwsz ~]$ yarn -v
1.7.0
</code></pre>
<p>更换 Yarn 源为淘宝提供的源。</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ yarn config set registry https://registry.npm.taobao.org/
yarn config v1.7.0
success Set "registry" to "https://registry.npm.taobao.org/".
Done in 0.03s.
[guser@iz2ze71m2nxbuwib8avpwsz ~]$ yarn config get registry
https://registry.npm.taobao.org/
</code></pre>
<p>检查 Yarn 是否全局可用</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ yarn global bin
/home/writer/.yarn/bin
</code></pre>
<p>修改 Yarn 系统变量</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ yarn config set prefix /usr/local
yarn config v1.7.0
success Set "prefix" to "/usr/local".
Done in 0.04s.
[guser@iz2ze71m2nxbuwib8avpwsz ~]$ yarn global bin
/usr/local/bin
</code></pre>
<p>检查 Node.js</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ node -v
v6.14.3
[guser@iz2ze71m2nxbuwib8avpwsz ~]$ which node
/bin/node

</code></pre>
<h3 id="四、安装-ghost-cli。">四、安装 Ghost-CLI。</h3>
<p>安装ghost-cli</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ sudo yarn global add ghost-cli
yarn global v1.7.0
[1/4] Resolving packages...
warning ghost-cli &gt; read-last-lines &gt; fs-promise@0.5.0: Use mz or fs-extra^3.0 with Promise Support
[2/4] Fetching packages...
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Installed "ghost-cli@1.8.1" with binaries:
      - ghost
Done in 26.70s.
[guser@iz2ze71m2nxbuwib8avpwsz ~]$ ghost -v
Ghost-CLI version: 1.8.1

</code></pre>
<p>创建一个文件夹，注意文件夹的权限必须为 rwxrwxr-x （对应的值为：775），否则不能进行安装。</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ sudo mkdir /opt/ghost
[guser@iz2ze71m2nxbuwib8avpwsz ~]$ sudo chown guser:guser /opt/ghost
[guser@iz2ze71m2nxbuwib8avpwsz ~]$ sudo chmod 775 /opt/ghost

</code></pre>
<p>开始进行 Ghost 安装向导，这里使用 MySQL 作为 Ghost 数据库。</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ~]$ cd /opt/ghost
[guser@iz2ze71m2nxbuwib8avpwsz ghost]$ ghost install
✔ Checking system Node.js version
✔ Checking logged in user
✔ Checking current folder permissions
System checks failed with message: 'Linux version is not Ubuntu 16'
Some features of Ghost-CLI may not work without additional configuration.
For local installs we recommend using `ghost install local` instead.
? Continue anyway? Yes
ℹ Checking operating system compatibility [skipped]
✔ Checking for a MySQL installation
✔ Checking memory availability
✔ Checking for latest Ghost version
✔ Setting up install directory
✔ Downloading and installing Ghost v1.24.5
✔ Finishing install process
? Enter your blog URL: http://localhost:2368
? Enter your MySQL hostname: localhost
? Enter your MySQL username: ghost
? Enter your MySQL password: [hidden]
? Enter your Ghost database name: ghost
✔ Configuring Ghost
✔ Setting up instance
Running sudo command: chown -R ghost:ghost /opt/ghost/content
? Password [hidden]
? Password [hidden]
? Password [hidden]
✔ Setting up "ghost" system user
? Do you wish to set up "ghost" mysql user? Yes
MySQL user is not "root", skipping additional user setup
ℹ Setting up "ghost" mysql user [skipped]
? Do you wish to set up Nginx? Yes
Nginx is not installed. Skipping Nginx setup.
ℹ Setting up Nginx [skipped]
Task ssl depends on the 'nginx' stage, which was skipped.
ℹ Setting up SSL [skipped]
? Do you wish to set up Systemd? Yes
✔ Creating systemd service file at /opt/ghost/system/files/ghost_localhost.service
Running sudo command: ln -sf /opt/ghost/system/files/ghost_localhost.service /lib/systemd/system/ghost_localhost.service

▽
server {
listen 88;
server {
listen 88;
Running sudo command: systemctl daemon-reload
✔ Setting up Systemd
Running sudo command: /opt/ghost/current/node_modules/.bin/knex-migrator-migrate --init --mgpath /opt/ghost/current
✔ Running database migrations
? Do you want to start Ghost? No

</code></pre>
<p>我选择没有启动，因为此时没有配置 Nginx.<br>
启动 ghost_localhost(安装完成后服务名称)服务，检查当前 web 服务状态</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz ghost]$ sudo systemctl start ghost_localhost
[guser@iz2ze71m2nxbuwib8avpwsz ghost]$ sudo systemctl is-active ghost_localh

active
</code></pre>
<p>启动 Ghost</p>
<pre><code>ghost start
ghost restart
ghost stop
</code></pre>
<pre><code>
[guser@iz2ze71m2nxbuwib8avpwsz ghost]$ ghost start
Running sudo command: systemctl is-active ghost_localhost
A ProcessError occurred.

Message: Command failed: /bin/sh -c sudo -S -p '#node-sudo-passwd#'  systemctl is-active ghost_localhost

unknown

Exit code: 3


Debug Information:
    OS: CentOS, v7.5.1804
    Node Version: v6.14.3
    Ghost-CLI Version: 1.8.1
    Environment: production
    Command: 'ghost start'

Additional log info available in: /home/guser/.ghost/logs/ghost-cli-debug-2018-06-22T07_54_54_716Z.log

Try running ghost doctor to check your system for known issues.

Please refer to https://docs.ghost.org/v1/docs/troubleshooting#section-cli-errors for troubleshooting.
</code></pre>
<p>Ghost 的安装到这里就结束了，接下来配置 Ngnix 就完工了。</p>
<h3 id="五、安装-nginx">五、安装 nginx</h3>
<p>安装nginx</p>
<pre><code> sudo yum -y install nginx
 nginx -v 
</code></pre>
<p>配置 nginx：</p>
<pre><code>cd /etc/nginx/conf.d 
sudo vi ghost.conf
server {
listen 88;
server_name 39.106.173.209;

location / {
    proxy_set_header   X-Real-IP $remote_addr;
    proxy_set_header   Host      $http_host;
    proxy_pass         http://127.0.0.1:2368;
}
}
</code></pre>
<p>测试nginx是否安装正常</p>
<pre><code>
[guser@iz2ze71m2nxbuwib8avpwsz conf.d]$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
</code></pre>
<p>更改默认监听端口80—&gt;88:</p>
<pre><code>cd /etc/nginx/conf.d 
sudo vi default.conf

</code></pre>
<p>重启Nginx</p>
<pre><code>[guser@iz2ze71m2nxbuwib8avpwsz conf.d]$ sudo systemctl start nginx
[guser@iz2ze71m2nxbuwib8avpwsz conf.d]$ sudo systemctl status nginx
● nginx.service - nginx - high performance web server
   Loaded: loaded (/usr/lib/systemd/system/nginx.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-06-22 16:44:58 CST; 8s ago
     Docs: http://nginx.org/en/docs/
  Process: 4784 ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf (code=exited, status=0/SUCCESS)
 Main PID: 4785 (nginx)
    Tasks: 2
   Memory: 1.5M
   CGroup: /system.slice/nginx.service
           ├─4785 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx.conf
           └─4786 nginx: worker process

Jun 22 16:44:58 iz2ze71m2nxbuwib8avpwsz systemd[1]: Starting nginx - high performance web server...
Jun 22 16:44:58 iz2ze71m2nxbuwib8avpwsz systemd[1]: PID file /var/run/nginx.pid not readable (yet?) after start.
Jun 22 16:44:58 iz2ze71m2nxbuwib8avpwsz systemd[1]: Started nginx - high performance web server.

</code></pre>
<h3 id="访问web前台及后台">访问web前台及后台</h3>
<p>前台：<a href="http://39.106.173.209:88/">http://39.106.173.209:88/</a><br>
默认的几篇介绍ghost文章，学习后，如果不想保留的话，进入后台删除ghost默认用户即可。</p>
<p>后台：<a href="http://39.106.173.209:88/ghost">http://39.106.173.209:88/ghost</a><br>
进入后首先要设置一名后台管理员，然后自己可以随心所欲写文章了。</p>
<h3 id="reference">Reference:</h3>
<p><a href="https://iiong.com/ghost-ver-1-x-installation-notes.html">https://iiong.com/ghost-ver-1-x-installation-notes.html</a><br>
<a href="https://blog.uranium.store/build-a-ghost-on-centos/">https://blog.uranium.store/build-a-ghost-on-centos/</a><br>
<a href="https://www.sungz.com/centosda-jian-ghost-1-0xin-ban-ben-an-zhuang-jiao-cheng/">https://www.sungz.com/centosda-jian-ghost-1-0xin-ban-ben-an-zhuang-jiao-cheng/</a><br>
<a href="https://blog.csdn.net/duzilonglove/article/details/80090677">https://blog.csdn.net/duzilonglove/article/details/80090677</a></p>

