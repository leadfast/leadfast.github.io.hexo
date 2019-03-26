---
title: Windows下配置putty私钥ssh登录
date: 2014-07-01 17:00:00
tags: ['linux', 'ssh', 'putty']
---

首先得在你的服务器上生成公钥和私钥

```
ssh-keygen -t rsa -C "ifme.in@gmail.com"
```

创建authorized_keys文件并把公钥放进去

```
touch .ssh/authorized_keys
cat .ssh/id_rsa.pub > .ssh/authorized_keys
```

authorized_keys的文件权限需要是644, 否则会出现`Server refused our key`

```
chmod -R 700 .ssh
chmod -R 644 .ssh/authorized_keys
```

修改/etc/ssh/sshd_config文件, 取消以下3行的注释

```
RSAAuthentication yes
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
```

重启你的sshd

```
sudo /etc/init.d/sshd restart
```

下载你的私钥文件`.ssh/id_rsa`到本地

打开puttygen.exe, 载入刚才下载的私钥, 再点击保存私钥

打开putty.exe , 会话的[连接-数据-自动登录用户名]填写你登录的用户名, [连接-SSH-认证]载入刚才保存的私钥, 保存

或者自己建一个快捷方式

```
"C:\Application\putty_0.60cn\putty.exe" -i "C:\Application\putty_0.60cn\id_rsa_ifme_in.ppk" leadfast@ifme.in
```

最后, 修改/etc/ssh/sshd_config, 重启sshd服务, 可以关闭密码登录的方式了

```
PasswordAuthentication no
```

完
