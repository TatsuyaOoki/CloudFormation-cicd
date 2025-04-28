# CloudFormationのCI/CD確認用リポジトリ

CodePipelineを使用したCloudFormationのCI環境

## 使用方法

1. cloudformation tempalte格納用S3バケットをコンソールから作成する
2. cicd/pipeline.ymlを使用して、CI/CD関連リソースのスタックを作成する
3. template配下のYAMLファイルを編集し、GitHubにpushする。
4. CodePipelineが起動し、該当のS3バケットへテンプレートが格納される。
5. AWSコンソールからCloudFormationを開き、変更セットを作成し実行する。

## 備考

- 現状、templateがネスト構造の場合は、ネストされた子スタックに対して変更セットが作成されない。</br>
ネストされた子スタックに対して、変更セットを有効にするには以下のURLを参考に設定を実施するとよさそう</br>
[CI/CD配下でネストされたスタックの子スタックに対して、変更セットを有効にするテクニック](https://blog.usize-tech.com/aws-cfn-cicd-nestedstack-changeset/)

- 上記の制約があるため、S3へのCFnテンプレート格納までを自動化

- CodePipelineで変更セットを作成するにはS3のオブジェクトURLを直接指定はできず、`BuildOutput::template/main.yml`の形で指定する必要がある。\
その際は、変更セットの作成自体は可能だが、実行時にエラーとなる\
`S3 error: The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint. For more information check http://docs.aws.amazon.com/AmazonS3/latest/API/ErrorResponses.html`

- テストにはtemplateの構文検証とcfn-guardを使用した設定値確認を実施
