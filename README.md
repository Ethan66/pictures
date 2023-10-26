# pictures

### 介绍
作品：个人图床

### 仓库名

#### github地址
https://github.com/Ethan66/pictures

#### gitee仅做展示(Pages)
https://gitee.com/ethan6/images

### 操作

#### gitee创建项目和部署Pages
#### github创建项目
#### github action配置
在你的 GitHub 项目 .github/workflows/ 文件夹下创建一个 .yml 文件，如 sync.yml，内容如下：

```
name: Sync

on:
  push:
    branches: [master]
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Build Gitee Pages
        uses: yanglbme/gitee-pages-action@main
        with:
          # 注意替换为你的 Gitee 用户名
          gitee-username: ethan6
          # 注意在 Settings->Secrets 配置 GITEE_PASSWORD
          gitee-password: ${{ secrets.GITEE_PASSWORD }}
          # 注意替换为你的 Gitee 仓库，仓库名严格区分大小写，请准确填写，否则会出错
          gitee-repo: ethan6/images
          # 要部署的分支，默认是 master，若是其他分支，则需要指定（指定的分支必须存在）
          branch: master
```

#### github配置密码
> 在 GitHub 项目的「Settings -> Secrets」路径下配置好命名为 GITEE_PASSWORD 的密钥。GITEE_PASSWORD 存放 Gitee 帐号的密码。
![](https://ethan6.gitee.io/images/blog/github-secret-action-config.png)

#### github项目和gitee项目同步
```
npm init -y
npm install husky --save-dev
npx husky install

"scripts": {
    "prepare": "husky install",
    "push2": "sh pre-push-script.sh"
}

// 在根目录下新建文件pre-push-script.sh。
内容：
git push -f https://gitee.com/ethan6/images.git master

// 在.husky目录下新建文件pre-push: SKIP_PUSH是偶数或者SKIP_PUSH不存在才能npm run push2
内容：
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

if [[ -z "$SKIP_PUSH" || $(($SKIP_PUSH % 2)) -eq 0 ]]; then
  export SKIP_PUSH=$((SKIP_PUSH + 1))
  npm run push2
else
  export SKIP_PUSH=$((SKIP_PUSH + 1))
fi
```

### 参考文献
[可能是中国最好的 TS 入门到进阶系统教程--冴羽](https://github.com/mqyqingfeng/Blog/issues/238)

[Gitee Pages Action](https://gitee.com/yanglbme/gitee-pages-action/tree/main)
