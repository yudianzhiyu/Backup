how to use git 

http://luozhaoyu.iteye.com/blog/1461705


linux下git与github简单使用

gitgithub 
个人觉得github蛮好用，但是帮助系统还是不够人性化，东一句西一句，让新手看着头晕。所以稍稍整理下主要的步骤。

首先是在github上创建一个账户：luozhaoyu
然后我个人的主页就是github.com/luozhaoyu了。
然后在github上创建一个test仓库，进行基本配置后需要在test仓库中添加可以提交代码的电脑的公钥。

这个公钥怎么生成呢？这就要转到我们PC这一边进行设定了。
在linux上有一个ssh-keygen的工具，使用命令
Java代码  收藏代码

    ssh-keygen -t rsa -C "committer_email@committermail.com"  


设定存放目录和密码后把.ssh/id_rsa.pub的文件内容粘贴进github的test仓库里。
测试是否成功
Java代码  收藏代码

    ssh -T git@github.com  


如果出现
引用
Agent admitted failure to sign using the key

则使用
Java代码  收藏代码

    ssh-add id_rsa  


并输入passphrase

在本机安装git
Java代码  收藏代码

    apt-get install git  



配置用户名和邮箱
Java代码  收藏代码

    git config --global user.name 'The Name'  
    git config --global user.email anyemail@mail.com  


这个等效与home下.gitconfig文件中的
Java代码  收藏代码

    [user]                                                                            
    >---name = LZY under Ubuntu with Hasee  
    >---email = luozhaoyu90@gmail.com  

这里应该是随便配置用户名和邮箱都可以，这个事方便大家联系

成功后变在本机创建一个git仓库。
Java代码  收藏代码

    git init  


在远程初始一个git仓库
Java代码  收藏代码

    git --bare init  


新建一个文件夹test_git，在里面添加若干文件
Java代码  收藏代码

    git add *  


提交并评论
Java代码  收藏代码

    git commit -m 'your comment'  


设置github的仓库地址并取名为origin(可能可以取其它名字？)
Java代码  收藏代码

    git remote add origin git@github.com:luozhaoyu/test.git  


最后把master提交到origin服务器上
Java代码  收藏代码

    git push origin master  



复制一个git项目
Java代码  收藏代码

    git clone git://github.com/luozhaoyu/test.git  


更新项目
Java代码  收藏代码

    git pull  



创建一个分支
git init之后默认的分支叫做master，在commit之后可以使用
Java代码  收藏代码

    git branch  

查看现在所在的branch分支
Java代码  收藏代码

    git branch newbranchname  

创建一个新分支
Java代码  收藏代码

    git checkout branchname  

切换到其它分支OOXX

回滚
引用

撤销本地commit
Java代码  收藏代码

    git reset --soft HEAD^  
    git reset HEAD .  


这么做git log里也不会有痕迹

回滚有两种方法，一种是留痕迹的git revert
Java代码  收藏代码

    git revert cc3a9d3a5820b16bca3c1761efb5885b90371e94  


这是通过又一次的commit中和之前不要的commit达到回滚的目的。所以revert后面跟着的commit-ish就是需要被回滚的那次commit的值

另一种是不留痕迹的，也就是时光机
Java代码  收藏代码

    git reset d5bb1731bf32fb62dc7eedc573da41fa31e27151 --hard  

直接回到commit-ish那时的状态，之后发生了什么都不会出现在commit log里

Java代码  收藏代码

    建议使用checkout + merge代替回滚  



撤销不小心pull之后造成的merge
Java代码  收藏代码

    git merge --abort  






永久删除不小心commit的文件
https://help.github.com/articles/remove-sensitive-data
Java代码  收藏代码

    git filter-branch --index-filter 'git rm --cached --ignore-unmatch FOLDER/*' --prune-empty --tag-name-filter cat -- --all  
    git push origin master --force  
    # 完成上一步就以及删除了文件历史，注意要往每一个分支push，可以使用--all --tags  
    # 下面是在本地删除多余文件  
    rm -rf .git/refs/original/  
    git reflog expire --expire=now --all  
    git gc --prune=now  
    git gc --aggressive --prune=now  




查看commit的change
Java代码  收藏代码

    git show 91d71384d8f7592e206003047bb4f9c56f2a4caf  
    git log -p --color  
    git diff -p  


