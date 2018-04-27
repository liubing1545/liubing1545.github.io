---
title: 利用AppVeyor做GithubIO的CI
date: 2018-04-24 11:45:11
tags: [AppVeyor, github, hexo, CI, 持续集成, 持续部署]
---
## 建立两个仓库
 我基于hexo搭建的github.io，在更换电脑时无法随意的提交blog。（因每个机器都需要部署node+hexo的环境）    
 于是，我将我的github.io建立两个仓库。
 1. master仓库（hexo generate后生成的html等文件）
 2. hexo仓库（原始文件，包含md文档及主题等）
 
 并将hexo仓库设置为这个repo的主仓库。

## AppVeyor
[appveyor](https://www.appveyor.com/)是支持windows OS做CI的持续集成工具，可以使用GitHub账号登陆。绑定我的github.io项目。

#### 添加appveyor.yml到Source Repo
```yml
clone_depth: 5

environment:
  access_token:
    secure: [Your Github Access Token]

install:
  - ps: Install-Product node 6 #因为我的hexo是3.71的版本，因此需要安装node 6
  - node --version
  - npm --version
  - npm install
  - npm install hexo-cli -g


build_script:
  - hexo generate

artifacts:
  - path: public

on_success:
  - git config --global credential.helper store
  - ps: Add-Content "$env:USERPROFILE\.git-credentials" "https://$($env:access_token):x-oauth-basic@github.com`n"
  - git config --global user.email "%GIT_USER_EMAIL%"
  - git config --global user.name "%GIT_USER_NAME%"
  - git clone --depth 5 -q --branch=%TARGET_BRANCH% %STATIC_SITE_REPO% %TEMP%\static-site
  - cd %TEMP%\static-site
  - del * /f /q
  - for /d %%p IN (*) do rmdir "%%p" /s /q
  - SETLOCAL EnableDelayedExpansion & robocopy "%APPVEYOR_BUILD_FOLDER%\public" "%TEMP%\static-site" /e & IF !ERRORLEVEL! EQU 1 (exit 0) ELSE (IF !ERRORLEVEL! EQU 3 (exit 0) ELSE (exit 1))
  - git add -A
  - git commit -m "Update Static Site" && git push origin %TARGET_BRANCH% && appveyor AddMessage "Static Site Updated"
```
在GitHub生成好Access Token之后，你需要到[AppVeyor加密页面](https://ci.appveyor.com/tools/encrypt)把Access Token加密之后再替换[Your GitHub Access Token]。
![github-access-token](http://obksgg9lx.bkt.clouddn.com/github-access-token.png)
![github-webhook](http://obksgg9lx.bkt.clouddn.com/github-webhook.png)

#### 设置Appveyor的settings

![appveyor-setting1](http://obksgg9lx.bkt.clouddn.com/appveyor-setting1-.png)
在Appveyor Settings的Environment里设置以下四个变量。STATIC_SITE_REPO就是github Repo的地址，TARGET_BRANCH是Repo的目标branch（这里是master，相当于提交编译后的html等文件至master），GIT_USER_EMAIL和GIT_USER_NAME就是你GitHub账号的信息。
![appveyor-setting2](http://obksgg9lx.bkt.clouddn.com/appveyor-setting2-.png)
在Appveyor Settings的build里默认是MSBUILD，因为是appveyor.yml中配置的脚本来build的，因此这里改成SCRIPT。
![appveyor-setting3](http://obksgg9lx.bkt.clouddn.com/appveyor-setting3.png)


