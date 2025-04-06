# CloudFormationのCI/CD確認用リポジトリ

CodePipelineを使用したCloudFormationのCI/CD環境

## 使用方法

1. cicd/pipeline.ymlを使用して、CI/CD関連リソースのスタックを作成する
2. 作成されたSNSに対してサブスクリプションを追加する。
3. 必要に応じてcicd/param.dev.jsonを編集する</br>
※jsonファイルはCloudFormationのパラメータの値を定義したもの
4. template配下のYAMLファイルを編集し、GitHubにpushする。
5. CodePipelineが起動し、変更セットの作成後、承認リクエストがSNS経由で届く
6. 承認を押下すると、変更セットが実行され、CloudFormationの内容をデプロイする

## 備考

- 現状、templateがネスト構造の場合は、ネストされた子スタックに対して変更セットが作成されない。</br>
ネストされた子スタックに対して、変更セットを有効にするには以下のURLを参考に設定を実施するとよさそう</br>
[CI/CD配下でネストされたスタックの子スタックに対して、変更セットを有効にするテクニック](https://blog.usize-tech.com/aws-cfn-cicd-nestedstack-changeset/)
- テストにはtemplateの構文検証とcfn-guardを使用した設定値確認を実施
