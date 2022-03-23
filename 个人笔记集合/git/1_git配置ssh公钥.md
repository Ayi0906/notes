1. 删掉`C:\用户\[userName]\.ssh`文件
2. 命令行执行 `ssh-keygen -t rsa -C "2116815040@qq.com"`
3. 一路回车直到出现一个图案
4. 切换到.ssh文件夹 `cd ~/.ssh`
5. 查看生成的公钥
   1. `cat ~/.ssh/id_rsa.pub`
6. 复制公钥
7. github->setting
   1. 搜索ssh->SSH and GPG keys
   2. NEW SSH KEY 添加新的ssh公钥
   3. 输入命令`ssh -T git@github.com`测试配置是否成功