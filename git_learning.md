## git相关
1. 查看本机ssh公钥  
    - 打开git bash 窗口，任意位置皆可  
        1. 进入.ssh文件目录`cd ~/.ssh`  
        2. `ls` 查看目录下所有文件，一个文件对应一个公钥  
        3. 查看公钥`cat id_rsa.pub`或者`vim id_rsa.pub`  
        4. 如果能精确位置，直接一行命令`cat ~/.ssh/id_rsa.pub`  
    - 直接打开文件夹，本机位置 C:\Users\Lenovo\.ssh  
    
2. 查看远程仓库ssh  
    - gitlab  
        1. 点击用户头像下拉找到Perferences进入Settings，最左侧有SSH Keys进入
    - github
        1. 点击用户头像下拉找到Settings进入，最左侧有SSH and GPG Keys进入  
3. 添加ssh
    1. 确保本机有node、git环境
    2. ssh文件目录下打开git bash输入`ssh-keygen -t rsa -C '邮箱地址'`，下一步会要你输入文件名，如果任意位置打开bash则在最后加上 `-f ~/.ssh/文件名`
    3. 后两步是输入密码和确认密码，可直接回车，不要密码
    4. ssh文件下有config文件，没有就新建
        > \# 公司gitlab仓库  
            Host 172.55.5.101 # 只写域名即可，前面加上http或https反而访问不了  
            HostName 172.55.5.101  
            PreferredAuthentications publickey  
            IdentityFile ~/.ssh/gitlab_id_rsa # 本地key文件地址  
          \# gitee  
            Host gitee.com  
            HostName gitee.com  
            PreferredAuthentications publickey  
            IdentityFile ~/.ssh/gitee_id_rsa  
          \# github  
            Host github.com  
            HostName github.com  
            PreferredAuthentications publickey  
            IdentityFile ~/.ssh/github_project # 本机私钥地址  
    5. 找到ssh文件下新生成的.pub后缀的文件，复制里面的内容，根据第二步找到位置，添加公钥  
    6. 测试验证是否连接成功，公司：`ssh -T git@172.55.5.101`，github：`ssh -T git@github.com`，@后面加域名，与config配置文件里保持一致
    7. 验证时，如果出现`Permission denied (publickey)`问题，在config文件中对应的域名下加上`PubkeyAcceptedKeyTypes +ssh-rsa`，目前github验证成功了，公司的域名出现`git@172.55.5.101: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).`，但是能正常clone、pull、push，猜测是公司管理员设置了权限
4. gitlab、gitee、github网站界面与功能有些差异，但大致相同  
   - 建项目仓库，建分支，如果项目上线的话，还有就是公用仓库，至少两个分支，dev、test、main，各分支最好设置权限，把main保护起来，三者保护规则不同
   - 三者clone、commit、pull、push、merge等git命令是通用的
   - gitlab与gitee适合国内使用，速度快，后者比前者更强大，github目前的优势估计只剩下国内外开源项目比较多，范围较广