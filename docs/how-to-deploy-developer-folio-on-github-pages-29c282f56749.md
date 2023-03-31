# 如何在 GitHub 页面上部署开发者 Folio

> 原文：<https://medium.com/analytics-vidhya/how-to-deploy-developer-folio-on-github-pages-29c282f56749?source=collection_archive---------6----------------------->

DeveloperFolio 是由 Saad Pasta 为开发人员创建的投资组合网站模板。

![](img/88b9f891f697fa251b275a19216bbd0e.png)

费伦茨·阿尔马西在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

作为一名开发人员，但对 react、web 开发和部署一无所知。弄清楚如何部署它是相当具有挑战性的。并且没有足够的文档来正确地遵循这些步骤。

# 请遵循以下步骤:

1.  将[开发者账号](https://github.com/saadpasta/developerFolio/)转入你的 github 账户。
2.  现在，从您的 github 帐户将存储库克隆到您的本地环境中。
3.  现在根据你的要求编辑 portfolio.js，package.json 等文件。
4.  我们在“package.json”中的主要关注点。打开它，并将其更改为您的 github 用户名。

```
"homepage": "https://<GitHubUsername>.github.io",                         "name": "<GitHubUsername>.github.io",
```

5.在您的 github 帐户中创建一个新的存储库，如" <githubusername>.github.io "</githubusername>

6.现在，打开您执行“npm 安装”和“npm 启动”的终端。

7.输入命令进行查看

```
$ git remote -v
# View existing remotes
# origin  https://github.com/user/repo.git (fetch)
# origin  https://github.com/user/repo.git (push)
```

8.将您的**克隆的** developer folio 存储库源更改为您的<GitHubUsername>. github . io 源。

```
$ git remote rm origin
$ git remote add origin https://github.com/<user>/<user>.github.io.git
$ git config master.remote origin
$ git config master.merge refs/heads/master
# Change the 'origin' remote's URL
```

9.现在，再次检查原点。

```
$ git remote -v
```

10.设置上游以将推送发送到存储库。

```
git push --set-upstream origin master -f
```

11.现在，定期添加、提交、推送您所做的更改。

12.安装“gh-pages”

```
npm install --save gh-pages
```

13.最后是部署命令。

```
npm run deploy
```

恭喜你。您已经成功部署了您的投资组合。访问您的网站，与世界分享。

**参考文献:**

1.  [https://create-react-app.dev/docs/deployment/#github-pages](https://create-react-app.dev/docs/deployment/#github-pages)
2.  [https://stack overflow . com/questions/2432764/how-to-change-the-uri-URL-for-a-remote-git-repository](https://stackoverflow.com/questions/2432764/how-to-change-the-uri-url-for-a-remote-git-repository)
3.  [https://developerfolio.js.org/](https://developerfolio.js.org/)

# 结论:

你觉得这篇文章有用吗？给它鼓掌👏，分享给社区，有一些想法，还是我漏掉了什么？请在评论中与我分享📝。

# 连接

作者是一名数据科学家，热衷于构建有意义的影响导向型产品。他是纸牌专家。他是前谷歌开发者学生俱乐部(GDSC)负责人和 AWS 教育云大使。他喜欢与人交往。如果你喜欢他的作品，跟他打个招呼。

[](https://mrasimzahid.github.io/) [## @MrAsimZahid |研究科学家

### Kaggle 专家|前谷歌开发者 Studnet 俱乐部负责人& AWS 教育大使

mrasimzahid.github.io](https://mrasimzahid.github.io/)