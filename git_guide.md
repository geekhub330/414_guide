# git教学:帮助你了解如何将本地仓库的代码或者文件上传到远端github上
- 通常拉取一个github上的项目只需要git clone + 项目链接
- 同时你也可以把你自己的相关文件以及代码上传到你自己的github页面
# 
## 获取一个github账号
在Linux环境下默认为Ubuntu
# 
- 打开终端输入`ssh-keygen -t rsa -C "你创建github的邮箱"`
- 使用`ctrl+h`会在主目录下打开隐藏文件，找到`.ssh`文件夹，进去后你会看到生成的两个文件如下


![git1.png](/pics/git1.png)
- 其中`id_rsa`是你的私钥，不用管，`id_rsa.pub`是公钥，一会我们就需要把公钥添加到github上
- 打开终端输入`cat id_rsa.pub`会出现下面一串代码，将所有的复制


![git2.png](/pics/git2.png)
- 打开你的github账号，点击头像`settings-SSH and GPG keys`，点击`New SSH key`


![git3.png](/pics/git3.png)

- 将刚才复制的那一串代码填写到Key中，Title随便写，保存，这样在后续往github上上传东西时就不用再输入账号密码
# 
## 如何把本地文件和云端仓库建立链接
- 我在本地新创建了一个文件夹名为`414_guide`，里面有一个文件叫`git_guide.md`
- 我想把这个文件夹里的文件上传到github云端
1. 首先我们需要也在github上创建一个仓库，名称就叫`414_guide`
2. 打开你的github账号页面，创建一个新的仓库`New repository`


![git4.png](/pics/git4.png)
3. 名称就为`414_guide`，`description`可以选择不写，然后默认`public`公开，如果你想为该仓库创建一个说明文档，则勾选`Add a README file`，最后创建
4. 创建好后，此时的仓库时为空的，如果上一步勾选了添加说明文档，则此时会多出来一个`README.md`文件
5. 现在我们需要将本地的文件夹与刚刚创建的云端仓库建立链接
6. 分别有两种办法
### 方法一
复制刚刚创建好仓库的ssh链接，记住一定要是ssh，不能是https，因为第一步添加ssh key的操作就是为了这一步，然后在本地`git clone ssh链接`，如果克隆不下来那么查看一下clash中的`TUN mode`是否开启，然后你的本地就会有一个和云端一样的文件夹并且已经建立好了之间的链接
### 方法二
你已经在本地建好了一个名为`414_guide`的文件夹，那么我们只需要把它和云端仓库建立起链接就好
- 进入到该文件夹下，打开终端运行`git init`，初始化`git`
- `git remote add origin ssh链接`，这里的ssh链接还是刚刚克隆的那个链接，必须是以git开头的


![git5.png](/pics/git5.png)

- 然后使用命令`git remote -v`查看是否已经创建，如下图所示


![git6.png](/pics/git6.png)
- 创建好链接后我们就可以开始上传东西了
- 比如文件夹里有一个名为`git_guide.md`的文件我想把它上传上去，那么直接运行命令`git add git_guide.md`，如果是想一次性上传文件夹下所有文件包含子文件夹，那么使用`git add .`，同时需要注意，如果后续你的本地文件发生改变，你想保持和云端的同步，那么还是要重新执行这一系列步骤，即使是删除文件或文件夹，也要使用`git add 删除文件/文件夹`
- 到这一步就完了吗？还没有
- 刚才的操作其实只是将要上传同步的文件存到了一个服务器的缓存区，还没有正式提交
- 如果要执行提交操作，那么则继续刚才的步骤后执行`git commit -m "first commit"`，双引号中需要写上这次上传了什么或者有什么改动，以便日后作为日志查看，最好不要留空，尽量使用英文，中文可能会乱码
- 至此还有最后一步推送，执行命令`git push -u origin main`，`-u`的命令只在第一次上传使用，后续可以省略，如果这一步卡住同样检查你的`clash`的`TUN mode`
- 然后我们就完成了将本地文件推送到github云端的默认分支`main`
- 打开github页面刷新就可以看到刚刚上传的文件里
- 如果后续需要创建新的分支，则通过`git branch 新分支名称`，然后利用`git checkout 新分支`，就会自动切换过去，同时本地新分支中的文件和之前的`main`分支互不干扰，通过查看文件夹可以发现，新分支中没有任何文件
#### tips
撤销git commit `git reset --soft HEAD^`
撤销git add在暂存区文件`git restore --staged`


