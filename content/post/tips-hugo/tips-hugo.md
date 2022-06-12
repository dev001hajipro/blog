---
title: "Tips HUGO"
date: 2022-04-12T19:58:07+09:00
draft: false
---

## Hugo cheat sheet

```bash
hugo new posts/xxxx.md
#output width Draft and watch
hugo server -D 
#generate files
hugo --minify
```

出力先は config.toml の `publishDir = 'docs'`で設定

## Theme

- [ananke](https://github.com/theNewDynamic/gohugo-theme-ananke)
- [Stack](https://themes.gohugo.io/themes/hugo-theme-stack/)

## install Theme Stack

easy install from Route1

- [Hugo theme Stack](https://docs.stack.jimmycai.com/getting-started)

## WindowsのDockerでhugoを動かす

環境依存しないhugoコマンドが欲しかったのでDockerでklakegg/hugo:ext-ubuntuを使ってみました。
単純なklakegg/hugo-ubuntuを使うとテーマによってはhugo-extendが必要になるため面倒でした。

```bash
docker run --rm -it -p 1313:1313 -v $(pwd):/src klakegg/hugo:ext-ubuntu server
docker run --rm -it -v $(pwd):/src klakegg/hugo:ext-ubuntu build
```

## HugoとWindows10のWSL2 Ubuntu20-04

aptパッケージは古いのでsnapを使おうと検討しましたが、WSL2の場合snapを動かすのに特殊なスクリプト
が必要。またhugo modを使うのにはhugo.exeだけではなくgoも必要になるので、Windows10 Homeに
インストーラーでgolangをインストール、hugoをchocolateryでインストールするのが無難でした。

## Github Pagesに公開する場合はbaseurlを指定しないとCSSやJavaScriptが読み込まれない

hugoとStackテーマをインストールしてローカル環境で問題なかったのでGitHubにリリースしたら、
Github Pagesに公開する場合はbaseurlを指定しないとCSSやJavaScriptが読み込まれませんでした。

```yaml
baseurl: https://dev001hajipro.github.io/blog/
languageCode: ja
theme: hugo-theme-stack
paginate: 5
title: ブログ
publishDir: 'docs'
```

## Stackの細かい設定

<https://dev.classmethod.jp/articles/git-submodules-ignore-dirty/>
