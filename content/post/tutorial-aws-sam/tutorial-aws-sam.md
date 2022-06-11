---
title: "AWS SAM(Serverless Application Model) のチュートリアルをやってみます。"
date: 2022-05-04T08:50:07+09:00
draft: true
---

[チュートリアル: Hello World アプリケーションのデプロイ](https://docs.aws.amazon.com/ja_jp/serverless-application-model/latest/developerguide/serverless-getting-started-hello-world.html
) を試してSAMがどのようなもの学習します。

AWS SAMは、`npm init`や`npx create-react-app my-app`のようにAWS用サーバーレスアプリの
プロジェクトを作成します。2010年にAWS CloudFormationがリリースされました。これは
Infrastructure as Code(IaC)で、JSONまたはYAMLでEC2、Lambda、VPCなどを管理できます。
SAMは、CloudFormationの拡張です。よってSAMは、手軽にサーバーレスアプリを作るときに使います。
CloudFormationは、AWS上の独自サービスと連携するなど細かい設定が必要な場合に使われます。

---

## sam initを実行

`sam init`を実行すると、どのプロジェクトテンプレートを使うか聞かれるので、
`AWS Quick Start Templates`の`Hello World Example`とランタイムをPythonにしました。
これで、すぐに開発が始められます。

```powershell
> sam init

You can preselect a particular runtime or package type when using the `sam init` experience.
Call `sam init --help` to learn more.

Which template source would you like to use?
        1 - AWS Quick Start Templates
        2 - Custom Template Location
Choice: 1

Choose an AWS Quick Start application template
        1 - Hello World Example
        2 - Multi-step workflow
        3 - Serverless API
        4 - Scheduled task
        5 - Standalone function
        6 - Data processing
        7 - Infrastructure event management
        8 - Machine Learning
Template: 1

 Use the most popular runtime and package type? (Python and zip) [y/N]: y

Project name [sam-app]:

Cloning from https://github.com/aws/aws-sam-cli-app-templates (process may take a moment)

    -----------------------
    Generating application:
    -----------------------
    Name: sam-app
    Runtime: python3.9
    Architectures: x86_64
    Dependency Manager: pip
    Application Template: hello-world
    Output Directory: .

    Next steps can be found in the README file at ./sam-app/README.md


    Commands you can use next
    =========================
    [*] Create pipeline: cd sam-app && sam pipeline init --bootstrap
    [*] Validate SAM template: sam validate
    [*] Test Function in the Cloud: sam sync --stack-name {stack-name} --watch


SAM CLI update available (1.48.0); (1.47.0 installed)
To download: https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html
```

---

## Deploy 配置

`sam deploy`で各種設定をすると数分でAWSにデプロイされます。この時にIAMのロールなどを作成して
くれるのでAPI Gateway, LamdaをAWSコンソールから設定するときより楽に開発ができます。
デプロイしたら、API Gateway endpoint URLが表示されるのでここからAPIが使えます。

```powershell
CloudFormation outputs from deployed stack
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Outputs
Key                 HelloWorldFunctionIamRole
Description         Implicit IAM Role created for Hello World function
Value               arn:aws:iam::nnnnnnnn:role/sam-app-HelloWorldFunctionRole-nxnxnxnxnxnx

Key                 HelloWorldApi
Description         API Gateway endpoint URL for Prod stage for Hello World function
Value               https://nxnxnxnxnxnx.execute-api.ap-northeast-1.amazonaws.com/Prod/hello/

Key                 HelloWorldFunction
Description         Hello World Lambda Function ARN
Value               arn:aws:lambda:ap-northeast-1:nnnnnnnn:function:sam-app-HelloWorldFunction-nxnxnxnxnxnx
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------  
```

---

## --body ""ではイベント生成エラー

ひとまずの対応として、`--body`を削除して、`--method "GET"`と文字列のGETを使うように修正。

```powershell
sam local generate-event apigateway aws-proxy --path "hello" --method "GET" > api-event.json
```

チュートリアルでは以下が書かれていましたが、`--body ""`が使えずapi-event.jsonファイル作成に
失敗します。

```powershell
> sam local generate-event apigateway aws-proxy --body "" --path "hello" --method GET > api-event.json
Usage: sam local generate-event apigateway aws-proxy [OPTIONS]
Try 'sam local generate-event apigateway aws-proxy -h' for help.

Error: Got unexpected extra argument (hello)
```

`sam local generate-event apigateway aws-proxy -h`でヘルプが表示されるのでいろいろ試し
ましょう。リソースは、APIコンソールのCloudFoundationのスタックから削除ができます。

---

## まとめ

今回AWS SAMのチュートリアルをやってみました。使ってみてCloudFormationのスタックとSAMの関連性
がAWSコンソールから分かりました。CloufFormationエディターで構成を図解できたのもよかったです。
また開発フローとしては`sam deploy`に数分かかるので、コードを少し修正してすぐAWS上で確認するのは
難しく、チュートリアルに書かれていたDockerでローカル開発が必須のようです。

[AWS Serverless Application Repository](https://aws.amazon.com/jp/serverless/serverlessrepo/)
では、アプリのテンプレートを作って公開できます。[serverless-cognito](https://serverlessrepo.aws.amazon.com/applications/arn:aws:serverlessrepo:us-east-1:517266833056:applications~serverless-cognito
)は、SAMでCognite、Lambda、API Gateway、DynamoDBを使っているので、このCloudFormationや
Webにある日本語のチュートリアルなどを試していくとSAMが分かってきそうです。
