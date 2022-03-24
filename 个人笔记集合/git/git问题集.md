[Toc]

# 1

- 某天`git push`后突然出现这个错误

- > $ git push -u origin main
  > ssh: connect to host github.com port 22: Connection timed out
  > fatal: Could not read from remote repository.

- 尝试重新设置ssh钥匙, 但是还是没能解决

- 解决方法:

  - 输入以下的代码检查SSH是否能够连接成功

    - `ssh -T git@github.com`

  - 如果继续报错如下

    - `ssh: connect to host github.com port 22: Connection timed out`

  - 输入代码查看是否存在 id_rsa  id_rsa.pun known_hosts 三个文件

    - `cd ~/.ssh`
    - `ls`

  - 创建一个config文件

    - `vim config`

  - 复制以下内容到里面去

    - ```
      Host github.com
      User YourEmail@163.com
      Hostname ssh.github.com
      PreferredAuthentications publickey
      IdentityFile ~/.ssh/id_rsa
      Port 443
      ```

    - 注意这里复制的时候前面会有一些莫名的符号要删掉

    - 其中user 要做修改为自己的账号

    - `i`键是输入,`:wq`是退出

  - 再次执行`ssh -T git@github.com`查看是否连接成功