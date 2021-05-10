# Git
Git可以在任何时间点，把文档的状态作为更新记录保存起来。因此可以把编辑过的文档复原到以前的状态，也可以显示编辑前后的内容差异。
### 配置git

先输入ssh-keygen –t rsa –C “邮箱地址”

> 注意ssh-keygen之间是没有空格的,其他的之间是有空格的

回车之后,这样密钥就生成了,可以打开id_rsa.pub（C:\Users\liuxiatian\.ssh）来查看,我使用的是记事本直接打开的这个文件,里面的所有内容就是这个密钥,一会需要使用的时候,就直接全选复制就可以了

到github网站上去配置一下ssh key点击箭头指示的三角图标，选择Settings然后点击左侧的SSH Keys之后点击右侧的Add SSH Key这样就会出现添加SSH Key的界面，在Title这一栏填一个名字，名字随意起，之后打开刚才生成的那个文件id_rsa.pub全选复制里面的内容到Key这一栏中，点击Add Key按钮完成操作

验证配置是否成功:

ssh –T git@github.com

如果你是第一次，会让你输入yes或no,这时输入yes就可以了，其它显示就和我这个是一样的。如果你的是出现不是这些内容，有可能是显示权限问题什么的，就应该是我上面提到的那种情况，你看一下你生成密钥时是否操作正确，目录下是否有那个known_hosts这个文件

**现在配置一下用户名和邮箱：**

git config –global user.name “用户名”

git config –global user.email “邮箱”

拉取: 

```
git pull git@github.com:okbutsowhat/myTypora
```

增加文件:

```
git add .
```

提交:

```
git commit –m "commit message"
git push --set-upstream git@github.com:okbutsowhat/myTypora.git master
```

 