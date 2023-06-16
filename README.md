# Exastro IT Automation in Docker Compose  
このdocker-composeは、Dockerを使用し Exastro IT Automation を起動して実行できるようにします。  
  - (based on [exastro-it-automation](https://github.com/exastro-suite/exastro-it-automation))  
  - (based on [exastro-platform](https://github.com/exastro-suite/exastro-platform))  
  

## 使用法  

Exastro IT Automation の実行には、  
exastro、exastro-platform、exastro-it-automation の3つのDocker環境を開始していきます。  

### exastro の docker-compose up  

はじめに、exastro ディレクトリのDocker 環境を開始します。  
Exastro IT Automationで利用するGitlabと、Exastro IT AutomationとExastro Platformで利用するDBが起動します。  

```shell
cd exastro
cp .env.sample .env  # 値を変更することなく起動が可能ですが、変更を行いたい場合は .envファイルを編集してください。

docker-compose up -d
```
  
Exastro IT Automation は、Gitlab をAPIで操作します。  
起動したGitlabに root ユーザーでログインを行い、 API実行用の Personal Access Token を取得してください。  
取得したTokenは、Exastro IT Automation の.envファイルに設定しますので控えておいてください。  

http://localhost:40080/  
※ .env を変更した場合は、変更した値でGitlabにアクセスしてください。  
※ Gitlabの起動には、10分程掛かる場合があります。  
  
rootユーザーの初期パスワードは、以下のコマンドで取得してください。  
```shell  
docker-compose exec gitlab cat /etc/gitlab/initial_root_password > initial_root_password | cat initial_root_password  
```
  
Personal Access Token に必要な権限は、以下を参照してください。  
https://ita-docs.exastro.org/2.1/ja/installation/helm_on_kubernetes.html#gitlab  
  
  
### exastro-platform の docker-compose up  
つぎに、exastro-platform ディレクトリのDocker 環境を開始します。
  
```shell
cd ../exastro-platform
cp .env.sample .env  
# 以下のコマンドの出力結果を .envファイルのENCRYPT_KEYに設定してください。
head -c 32 /dev/urandom | base64
# 他設定値の変更を行いたい場合は、.envファイルを編集してください。

docker-compose up -d
```


### exastro-it-automation の docker-compose up  
最後に、exastro-it-automation ディレクトリのDocker 環境を開始します。

```shell
cd ../exastro-it-automation
cp .env.sample .env  
# 以下のコマンドの出力結果を .envファイルのENCRYPT_KEYに設定してください。
head -c 32 /dev/urandom | base64
# GITLAB_ROOT_TOKENに、控えておいたGitlabのPersonal Access Tokenを設定してください。
# 他設定値の変更を行いたい場合は、.envファイルを編集してください。

docker-compose up -d
```

### Organization作成  

exastro-platform/toolsディレクトリに、Organization作成用のスクリプトがあります。  
このスクリプトを使用し、Organization作成が実行できます。  

※ スクリプトの詳細は、以下を参照してください。  
https://ita-docs.exastro.org/2.1/ja/manuals/platform_management/organization.html  

```shell
cd ../exastro-platform/tools
./create-organization.sh
```

### 各サイトURL

Keycloak管理コンソール  
http://localhost:38001/auth/  
  
Organization用サイト  
http://localhost:38000/{オーガナイゼーションID}/platform/  

Gitlab  
http://localhost:40080/  

※docker-composeで、各ポートの設定を変更した場合は、変更後のポートで各サイトにアクセスしてください。  

