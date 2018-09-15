---

版本控制

---

版本控制在程序开发中起到一个不可或缺的作用，所以这里就简单介绍一下多人开发git的简单操作

## git全局设置

```
git config --global user.name "你设置的名字"

git config --global user.email "设置邮箱"
```

## 创建项目

现在托管平台上创建一个新项目,以下皆以github为例

img/

## HTTP协议与SSH协议

可以通过两种方式连接线上版本库

- http方式可以直接克隆,但是上传代码时需要验证用户名与密码

- ssh方式需要在线上和本地配置key与秘钥,若设置了秘钥密码，上传时需要输入面

### 配置本地SSH key

查看系统中是否已配置ssh key

```
# windows 其中%userprofile%替换成用户目录路径 
type %userprofile%\.ssh\id_rsa.pub 

# GNU/Linux/Mac/PowerShell
cat ~/.ssh/id_rsa.pub
```

如果输出了以ssh_rsa 开头的一长串字符串,说明已经配置过ssh key了

生成ssh key

```
ssh-keygen -t rsa -C "你的邮箱地址"
```

执行后按照提示进行操作,最好设置一个密码,也可以不设置直接跳过

复制生成的 ssh key,根据系统不同可以用以下的方式复制,windows下也可以直接复制

```
# Windows Command Line
type %userprofile%\.ssh\id_rsa.pub | clip

# Windows PowerShell
cat ~/.ssh/id_rsa.pub | clip

# Mac
pbcopy < ~/.ssh/id_rsa.pub

# GNU/Linux (requires xclip)
xclip -sel clip < ~/.ssh/id_rsa.pub
```

## 在托管平台中设置ssh key

在设置setting中找到SSH and GPG Keys 选项,点击右上角的 New SSH key按钮

img/

在key 这一栏添加刚复制过的key ,title填什么都可以

img/


## 创建新的本地版本库




