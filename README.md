# 知识杂货铺

## 1、git操作
``` shell
git remote add  origin git@github.com:wuxy910810/ColaBaba_hardwork.git
git add .
git commit -m "new commit"
# 本地仓库推送到远程仓库 git pull [remote] [branch]
git pull origin master
```

## 2、github.com网址访问不了？怎么解决？

- step1：hosts文件修改，ping通。
``` shell
140.82.112.3 github.com
66.249.89.104 github.global.ssl.fastly.net
203.208.39.99 assets-cdn.github.com 
```

- step2: 若还是不行。

  高级 -> 打开代理设置 -> 取消代理服务器的设置

- step3: 还是不行，没招了。