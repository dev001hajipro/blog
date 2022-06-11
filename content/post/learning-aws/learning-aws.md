---
title: "Amplifyのチュートリアルをやってみます"
date: 2022-04-18T21:16:57+09:00
draft: false
---

## AmplifyとApp Runnerの使い分け

- [【開催報告&動画公開】アプリ開発に集中するためのインフラ選定。運用工数を低減できるサービスとは -AWS Startup.fm](https://aws.amazon.com/jp/blogs/startup/event-report-startupfm-infrastructure-selection-2021/)

## チュートリアル、ハンズオン

- [AWS Amplify 入門ハンズオンを公開しました！ – AWS Hands-on for Beginners Update](https://aws.amazon.com/jp/blogs/news/aws-hands-on-for-beginners-19/)

- [React アプリケーションの構築
AWS Amplify を使用してシンプルなウェブアプリケーションを作成する](https://aws.amazon.com/jp/getting-started/hands-on/build-react-app-amplify-graphql/)

- [Amplify docs](https://docs.amplify.aws/)

## チュートリアル

[Amplify Docs - Getting started](https://docs.amplify.aws/start/getting-started/installation/q/integration/react/)

英語版のチュートリアルは、各フレームワークや言語が揃っている。とりあえず、Reactを試す。

### 準備

さいしょにAWSアカウントを作成して、Node.jsでAmplify Consoleのインストール。Amplifyが使うリージョンやユーザーの作成を行います。

```sh
amplify configure
```

- 東京リージョン:`ap-northeast-1`
- アクセス権限:`AdministratorAccess-Amplify`
  - 以前は`AdministratorAccess`を設定していたようです。
- AWS Profile:デフォルトでもよいけど`amplify-tutorial`

AWS Profileは`%USERPROFILE%\.aws`に2つファイルができます。AWSのドキュメントの[AWS CLI の名前付きプロファイル](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/cli-configure-profiles.html)からファイルの意味が分かるので後で調べましょう。

#### Reactプロジェクト作成

create-react-appでReactプロジェクトを作成。

```bash
npx create-react-app  react-amplified
cd react-amplified
npm start
```

`amplify init`でバックエンドの初期化。Amplifyの各種設定がほぼ自動で設定できるので便利です。

```bash
amplify init
```

### amplify init のプロジェクト名

デフォルトのままで問題ないです。

AWSやFirebaseなどを使ったことがあると分かりやすいですが、はじめてだとなぜ2つプロジェクト名が必要か疑問になると思います。

`amplify init`は実行すると、amplifyディレクトリー内に、各種設定ファイルを作成(IoC)し、*このプロジェクト名でAWS上にAmplifyプロジェクトを作成します。*

一度、ブラウザからAWS Amplifyコンソールをみてみたり、EC2インスタンスを作ってみたりするとすぐ意味が分かります。

```text
react-amplified/
  amplify/
    .config/
      project-config.json <-ここにAmplifyプロジェクト名
```

```bash
Enter a name for the project (ractamplified)
The following configuration will be applied:
```

最後に以下のメッセージが表示されてAmplifyの初期化が完了。

```bash
Your project has been successfully initialized and connected to the cloud!

Some next steps:
"amplify status" will show you what you've added already and if it's locally configured or deployed
"amplify add <category>" will allow you to add features like user login or a backend API
"amplify push" will build all your local backend resources and provision it in the cloud
"amplify console" to open the Amplify Console and view your project status
"amplify publish" will build all your local backend and frontend resources (if you have hosting category added) and provision it in the cloud

Pro tip:
Try "amplify add api" to create a backend API and then "amplify push" to deploy everything
```

## データベース接続をAPI作成

コマンド１つでAPI作ってくれました。便利!

```bash
> amplify add api
? Select from one of the below mentioned services: GraphQL
? Here is the GraphQL API that we will create. Select a setting to edit or continue Continue
? Choose a schema template: Single object with fields (e.g., “Todo” with ID, name, description)

⚠️  WARNING: your GraphQL API currently allows public create, read, update, and delete access to all models via an API Key. To configure PRODUCTION-READY authorization rules, review: https://docs.amplify.aws/cli/graphql/authorization-rules

✅ GraphQL schema compiled successfully.

Edit your schema at C:\react-amplified\amplify\backend\api\amplified\schema.graphql or place .graphql files in a directory at C:\react-amplified\amplify\backend\api\amplified\schema
```

このまま特に問題なくチュートリアルを完了できました。

## 認証の前や後に独自の処理を追加

Trigger機能があります。

[Amazon Cognito UserPoolでメールアドレスが事前登録されたユーザーにだけソーシャル・ログインを許可する仕組みを作る](https://zenn.dev/intercept6/articles/574c3eb5f79d463e25ae)

## Amplifyの簡単さ

Amplify CLIでFirebaseと同じように簡単にアプリが作れました。以前、[AWS の開始方法
基本的なウェブアプリケーションを構築する](https://aws.amazon.com/jp/getting-started/hands-on/build-web-app-s3-lambda-api-gateway-dynamodb/?e=gs2020&p=fullstack)を試した各種コンソールで設定をしないといけないのでAWSは面倒と感じたのですが、Amplifyがあれば新規アプリやプロトタイプを作るのがすぐできそう。

## 公開と公開したリソース削除

amplifyのサブコマンドを使えば、公開、削除、状態確認ができます。`amplify delete`では構築したAWS環境を削除できます。

```bash
amplify publish
amplify delete
amplify status
```

## その他チュートリアル

[AWS の開始方法 基本的なウェブアプリケーションを構築する](https://aws.amazon.com/jp/getting-started/hands-on/build-web-app-s3-lambda-api-gateway-dynamodb/)

このチュートリアルは、ブラウザ上でAWS Lambda, Gateway, DynamoDBなどを追加していきAmplifyベースのWebアプリを作成します。順々に作るので、個々の機能の意味を理解しながら作りやすいです。

しかし、コマンドライン版であるAmplify CLIでは、コマンド１つでそれらを自動生成してくれます。この手軽さが上記チュートリアルでは分からないのでAWS初心者は両方試してみると良いです。

### わからない用語のまとめ

- Amazon Cognito - 認証/認可
- AWS App Runner 2021/05
  - ECS/Fargateだとネットワーク、インフラ設定が大変。
  - Python3, Node.js12
- Amplify
  - GraphQLをAWS AppSyncですぐに使える。
- AWS SAM(Serveless Application Model) 2016年リリース
- AWS CloudFunction (2010) is tool for Infrastructure as Code(IaC)
- AWS CDK コードのインフラ化CloudFunctionとの違いはJSONではなくPythonなど言語で書ける。

## SAMまわりの教材

- AWS CloudFormationは2011年リリース
- CloudFormationでサーバーレスアプリを作るための拡張機能がSAM
- AWS SAM(Serverless Application Model)は2016年リリース

[AWS CloudFormation と AWS SAM を使用したサーバーレスアプリケーションの開発とデプロイ](https://aws.amazon.com/jp/blogs/news/build-deploy-serverless-app-using-aws-serverless-application-model-and-aws-cloudformation/)
