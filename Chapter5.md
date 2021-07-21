# VScode配置

#### 配置vscode和远程服务器实现ssh连接

+ commit:ID位于vscode->help->about
+ 本地vscode config文件

> C:\\Users\\{username}\\.ssh\config
>
> Host 10.23.129.67
>
> ​	HostName 10.23.129.67
>
> ​	User yuchao

#### 本地vscode安装插件

+ remote-ssh
+ linux-tools
+ 注：名字不一定对，商店里找即可

####  服务器安装离线环境

1. 在线安装的时候，远程登录时就会联网自动将服务器环境配置好，离线的情况没有网络，我们需要手动配置服务器的环境，首先根据在线安装的方式，尝试登录一次（必须），这时肯定会登录失败，接下来我们配置服务器

2. 进入服务器，输入命令cd ~进入用户目录，输入命令la查看是否存在.vscode-server或.vscode-server-insiders文件夹（根据你开发机安装的vsCode类型而定），按照图2的步骤操作，记住那个ID（注意你的ID不一定和我的一样）

3. 在可以上网的电脑上输入下面的网址，下载离线包：

正式版：（注意将ID替换为前面说的那个ID号）

https://update.code.visualstudio.com/commit:ID/server-linux-x64/stable

4. 离线包下载后将其拷贝到服务器里面以ID为名字的目录里面，可能里面存在一个同名的文件，但是文件是空的，因为联网下载失败了，覆盖它就行了，见图3：

5. 重新使用vsCode登录远程服务器，就可以成功登录了，第一次登录的话理论上会弹出两次对话框，第一次直接回车就行了，第二次就是密码，失败的话重新登下试试，反正我是一次性成功

#### 配置免密

+ 本地和服务器都ssh-keygen -t rsa,拷贝本地文件id_rsa.pub到服务器，生成~/.ssh/authorized_keys

+ chmod 700 /home/username
+ chmod 700 ~/.ssh
+ chmod 600  ~/.ssh/authorized_keys
