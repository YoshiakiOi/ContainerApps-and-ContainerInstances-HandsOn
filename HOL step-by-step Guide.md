# Azure Container Apps and Container Instances ハンズオン

<br />

### Contents

- [開発環境の準備](#開発環境の準備)

  - [Task 1: 開発環境へのリポジトリのクローン](#task-1-開発環境へのリポジトリのクローン)

- [Exercise 1: Docker イメージの構築と実行](#exercise-1-docker-イメージの構築と実行)

  - [Task 1: ローカルでのアプリケーションの実行](#task-1-ローカルでのアプリケーションの実行)

  - [Task 2: 公開用ビルドのファイル セットの発行](#task-2-公開用ビルドのファイル-セットの発行)

  - [Task 3: Docker ファイルの作成](#task-3-docker-ファイルの作成)

  - [Task 4: Docker イメージの構築](#task-4-docker-イメージの構築)

  - [Task 5: イメージからコンテナーを起動](#task-5-イメージからコンテナーを起動)

- [Exercise 2: Azure Container Registry の作成とイメージのプッシュ](#exercise-2-azure-container-registry-の作成とイメージのプッシュ)

  - [Task 1: Azure Container Registry の作成](#task-1-azure-container-registry-の作成)

  - [Task 2: イメージをレジストリへプッシュ](#task-2-イメージをレジストリへプッシュ)

- [Exercise 3: Azure Container Apps の作成とイメージの展開](#exercise-3-azure-container-apps-の作成とイメージの展開)

  - [Task 1: コンテナー アプリの作成](#task-1-コンテナー-アプリの作成)

  - [Task 2: コンテナー レジストリからのイメージ取得](#task-2-コンテナー-レジストリからのイメージ取得)

- [Exercise 4: Azure Container Apps の設定](#exercise-4-azure-container-apps-の設定)

  - [Task 1: リビジョン モードの設定](#task-1-リビジョン-モードの設定)

  - [Task 2: アプリケーションの更新](#task-2-アプリケーションの更新)

  - [Task 3: 新しいリビジョンの展開](#task-3-新しいリビジョンの展開)

  - [Task 4: コンテナー アプリのスケーリング](#task-4-コンテナー-アプリのスケーリング)

- [Exercise 5: Azure Container Instances の作成と実行](#exercise-5-azure-container-instances-の作成と実行)

  - [Task 1: バッチ実行用コンテナーイメージの作成とプッシュ](#task-1-バッチ実行用コンテナーイメージの作成とプッシュ)

  - [Task 2: Azure Container Instances の作成と実行](#task-2-azure-container-instances-の作成と実行)

<br />

## 開発環境の準備

<img src="images/development-environment.png" />

### Task 1: 開発環境へのリポジトリのクローン

- [Azure ポータル](#https://portal.azure.com)へアクセス

- 事前展開済みの仮想マシンの管理ブレードへ移動し、「**接続**」-「**Bastion**」を選択

  <img src="images/connect-vm-01.png" />

- ユーザー名、パスワードを指定し、仮想マシンへ接続

  <img src="images/connect-vm-02.png" />

- 新しいタブで仮想マシンへの接続を行い、デスクトップ画面が表示

- Visual Studio Code を起動 (デスクトップ上の準備されたショートカットをダブルクリック)

- "**Terminal**" - "**New Terminal**" を選択し、ターミナルを表示

  <img src="images/git-config-01.png" />

- Web ブラウザで このリポジトリの "**Code**" をクリック

  表示されるツール チップよりリポジトリの URL をコピー

  <img src="images/git-clone-01.png" />

- Visual Studio Code のサイドバーから Explorer を選択し "**Clone Repository**" をクリック

  <img src="images/git-clone-02.png" />

- リポジトリの URL の入力を求められるためコピーした URL を貼り付け Enter キーを押下

  <img src="images/git-clone-03.png" />

- 複製先となるローカル ディレクトリ (Documents) を選択

  GitHub の認証情報が求められる場合は、アカウント名、パスワードを入力し認証を実施

- 複製されたリポジトリを開くかどうかのメッセージが表示されるので "**Open**" をクリック

- Explorer に複製したリポジトリのディレクトリ、ファイルが表示

  ```
  git remote -v
  ```

<br />

## Exercise 1: Docker イメージの構築と実行

### Task 1: ローカルでのアプリケーションの実行

<details>
  <summary>C#</summary>

- Visual Studio Code で "**Terminal**" - "**New Terminal**" を選択

- ASP.NET Core プロジェクトのディレクトリへ移動

  ```
  cd src/CS/Web
  ```

- アプリケーションを実行

  ```
  dotnet run
  ```

- ターミナルに表示されるアプリケーションの URL を Ctrl キーを押しながらクリック

  <img src="images/dotnet-run-01.png" />

- Web ブラウザが起動し、アプリケーションのトップ画面が表示

  <img src="images/dotnet-run-02.png" />

- ターミナルで Ctrl + C を押下し、アプリケーションの実行を終了

</details>

<details>
  <summary>Java</summary>

- Visual Studio Code で "**Terminal**" - "**New Terminal**" を選択

- SpringBoot プロジェクトのディレクトリへ移動

  ```
  cd src/Java/Web
  ```

- アプリケーションを実行

  ```
  ./mvnw spring-boot:run
  ```

  - ※Spring-Boot のプラグインがインストールしてある場合はつぎの手順でも実行可能。

    1. Visual Studio Code で src/Java/Web/src/main/com/microsoft/cloudworkshop/CloudworkshopApplication.java を**ダブルクリック**で開く。しばらく待つと下図のように main メソッドの上部に「Run|Debug」が表示される

    <img src="images/java-run-01.png" />

    2. main メソッドの上に表示された Run をクリック。

- 起動したらブラウザで http://localhost:5001 にアクセス

- アプリケーションのトップ画面が表示

  <img src="images/java-run-02.png" />

- ターミナルで Ctrl + C を押下し、アプリケーションの実行を終了

</details>

<br />

### Task 2: 公開用ビルドのファイル セットの発行

<details>
  <summary>C#</summary>

- ターミナルでコマンドを実行し、展開のためのファイル セットをディレクトリへ発行

  ```
  dotnet publish -c Release -o ./bin/Publish
  ```

- bin フォルダ内に Publish フォルダが生成され、アプリケーションとその依存関係が発行されていることを確認

  <img src="images/dotnet-publish.png" />

</details>

<details>
  <summary>Java</summary>

- Visual Studio Code で "**Terminal**" - "**New Terminal**" を選択

- SpringBoot プロジェクトのディレクトリへ移動

  ```
  cd src/Java/Web
  ```

- パッケージ作成

  ```
  ./mvnw package
  ```

- target フォルダ内に jar ファイルが生成されていることを確認

  <img src="images/java-publish.png" />

</details>

<br />

### Task 3: Docker ファイルの作成

<details>
  <summary>C#</summary>

- Visual Studio Code の Explorer で "**.docker**" - "**CS**" を展開し "**dockerfile** を選択

  <img src="images/update-dockerfile-01.png" />

- エディタ画面で編集

  9 行目に先の手順で発行したファイル セットをコピーする操作を追加

  ```
  COPY ./src/CS/Web/bin/Publish .
  ```

- 「**File**」メニューの「**Save**」を選択し、ファイルを保存

</details>

<details>
  <summary>Java</summary>

- Visual Studio Code の Explorer で "**.docker**" - "**Java**" を展開し "**dockerfile** を選択

  <img src="images/update-dockerfile-java-01.png" />

- エディタ画面で編集

  8 行目に先の手順で発行した jar ファイルをコピーする操作を追加

  ```
  COPY src/Java/Web/target/*.jar /opt/app/app.jar
  ```

- 「**File**」メニューの「**Save**」を選択し、ファイルを保存

</details>

<br />

### Task 4: Docker イメージの構築

- デスクトップ上の "**Ubuntu**" ショートカットをダブルクリック

- 操作用のプロンプトが起動

- WSL で Windows 側のマウントされたディレクトリへ移動

  ```
  cd /mnt/c/Users/AzureUser/Documents/ContainerApps-and-ContainerInstances-HandsOn
  ```

- docker build コマンドを実行しイメージを構築

  <details>
    <summary>C#</summary>

  ```
  docker build -t app:v1 -f .docker/CS/dockerfile .
  ```

  ※ コマンドのオプション

  - **-t**: 名前とタグを **名前:タグ** の形式で指定

  - **-f**: dockerfile のパスを指定

    <img src="images/docker-build-01.png" />

  - docker images コマンドを実行し、構築されたイメージが表示されることを確認

  ```
  docker images
  ```

    <img src="images/docker-build-02.png" />

  </details>

  <details>
    <summary>Java</summary>
    
    ```
    docker build -t app:v1 -f .docker/Java/dockerfile .
    ```

  ※ コマンドのオプション

  - **-t**: 名前とタグを **名前:タグ** の形式で指定

  - **-f**: dockerfile のパスを指定

    <img src="images/docker-build-java-01.png" />

  - docker images コマンドを実行し、構築されたイメージが表示されることを確認

  ```
  docker images
  ```

    <img src="images/docker-build-java-02.png" />

  </details>

<br />

### Task 5: イメージからコンテナーを起動

<details>
  <summary>C#</summary>

- docker run コマンドを実行し、作成したイメージからコンテナーを起動

  ```
  docker run -p 8080:80 app:v1
  ```

  ※ コマンドのオプション

  - **-p**: ポート マッピング（コンテナーの 80 番ポートを 8080 番ポートへマッピング）

  <img src="images/cs-docker-run-01.png" />

- Web ブラウザを起動し http://localhost:8080 へアクセス

  <img src="images/cs-docker-run-02.png" />

- 操作用のプロンプトで Ctrl + C キーを押下し、アプリケーションを終了

</details>

<details>
  <summary>Java</summary>

- docker run コマンドを実行し、作成したイメージからコンテナーを起動

  ```
  docker run -p 8080:80 app:v1
  ```

  ※ コマンドのオプション

  - **-p**: ポート マッピング（コンテナーの 80 番ポートを 8080 番ポートへマッピング）

  <img src="images/java-docker-run-01.png" />

- Web ブラウザを起動し http://localhost:8080 へアクセス

  <img src="images/cs-docker-run-02.png" />

- 操作用のプロンプトで Ctrl + C キーを押下し、アプリケーションを終了
</details>

- コンテナー一覧を表示するコマンドを実行

  ```
  docker ps -a
  ```

  ※ コマンドのオプション

  - **-a**: 起動中・停止中を含め、すべてのコンテナを表示（Java の場合は COMMAND の内容が異なる）

  <img src="images/docker-run-03.png" />

- 再度、イメージからコンテナーを起動

  ```
  docker run -d --rm -p 8080:80 app:v1
  ```

  ※コマンドのオプション

  - **-d**: デタッチド モードでコンテナを起動

  - **--rm**: コンテナ終了時にコンテナを削除

  - **-p**: ポート マッピング（コンテナの 80 番ポートを 8080 番ポートへマッピング）

  <img src="images/docker-run-04.png" />

- 起動中のコンテナーを確認

  ```
  docker ps
  ```

  <img src="images/docker-run-05.png" />

  ※ CONTAINER ID を確認

- Web ブラウザを起動し http://localhost:8080 へアクセス

- コンテナーを停止 (docker ps コマンドで確認した CONTAINER ID を指定)

  ```
  docker stop <CONTAINER ID>
  ```

- コンテナー一覧を表示するコマンドを実行

  ```
  docker ps -a
  ```

  ※ 2 度目に実行したコンテナーが表示されないことを確認

- 停止中のコンテナーを削除 (1 度目に実行したコンテナを削除)

  ```
  docker rm <CONTAINER ID> -f
  ```

  <img src="images/docker-run-06.png" />

- コンテナー一覧を表示するコマンドを実行

  ```
  docker ps -a
  ```

  ※ コンテナーが表示されないことを確認

<br />

## Exercise 2: Azure Container Registry の作成とイメージのプッシュ

<img src="images/exercise-2.png" />

### Task 1: Azure Container Registry の作成

- Web ブラウザで [Azure ポータル](#https://portal.azure.com)へアクセス

- ポータルのトップ画面で「**+ リソースの作成**」をクリック

  <img src="images/create-azure-resources.png" />

- 左側のメニューで「**コンテナー**」を選択し、「**Container Registry**」の「**作成**」をクリック

  <img src="images/create-azure-container-registry-01.png" />

- コンテナー レジストリの作成

  - 「**基本**」

    - **プロジェクト詳細**

      - **サブスクリプション**: ワークショップで使用するサブスクリプション

      - **リソース グループ**: ワークショップで使用するリソース グループ

    - **インスタンスの詳細**

      - **レジストリ名**: 任意 (英大文字小文字、数字のみで 5 から 50 文字)

      - **場所**: 任意

      - **SKU**: Standard

      <img src="images/create-azure-container-registry-02.png" />

  - 「**ネットワーク**」

    - **接続の構成**: パブリック アクセス (すべてのネットワーク)

      <img src="images/create-azure-container-registry-03.png" />

      ※ Premium SKU 選択時のみ構成可

  - 「**暗号化**」

    - **カスタマー マネージド キー**: 無効

      <img src="images/create-azure-container-registry-04.png" />

      ※ Premium SKU 選択時のみ構成可

- 「**確認および作成**」をクリックし、指定した内容を確認後、「**作成**」をクリック

  <img src="images/create-azure-container-registry-05.png" />

<br />

### Task 2: イメージをレジストリへプッシュ

- Azure ポータルで作成したコンテナー レジストリの管理ブレードへアクセス

- 左側のメニューから「**アクセス キー**」を選択

- 「**管理者ユーザー**」を有効化

  <img src="images/azure-container-registry-key.png" />

- 操作用のプロンプトでレジストリへログイン

  ```
  docker login yourregistry.azurecr.io
  ```

  ※ yourreregistry.azurecr.io を作成したコンテナー レジストリのログイン サーバーに変更

  ※ コンテナー レジストリのログイン サーバー名は管理ブレードのアクセス キーから確認可

- Username, Password を指定し、ログインを実行

  ※ コンテナー レジストリの管理ブレードのアクセス キーから取得できるユーザー名とパスワードを使用

- レジストリへの完全修飾パスを使用して、イメージのエイリアスを作成

  ```
  docker tag app:v1 yourregistry.azurecr.io/app:v1
  ```

  ※ yourreregistry.azurecr.io を作成したコンテナー レジストリのログイン サーバーに変更

- エイリアスを付与したイメージが表示されることを確認

  ```
  docker images
  ```

  <img src="images/docker-push-01.png" />

- docker push を使用してレジストリへプッシュ

  ```
  docker push yourregistry.azurecr.io/app:v1
  ```

  ※ yourreregistry.azurecr.io を作成したコンテナー レジストリのログイン サーバーに変更

  <img src="images/docker-push-02.png" />

- Azure ポータルで作成したコンテナー レジストリの管理ブレードへアクセス

- 左側のメニューから「**リポジトリ**」を選択

- リポジトリ内のイメージを確認

  <img src="images/docker-push-02.png" />

<br />

## Exercise 3: Azure Container Apps の作成とイメージの展開

<img src="images/exercise-3.png" />

### Task 1: コンテナー アプリの作成

- Web ブラウザで [Azure ポータル](#https://portal.azure.com)へアクセス

- ポータルのトップ画面で「**+ リソースの作成**」をクリック

  <img src="images/create-azure-resources.png" />

- 左側のメニューで「**コンテナー**」を選択し、「**コンテナー アプリ**」の「**作成**」をクリック

  <img src="images/create-container-apps-01.png" />

- コンテナー アプリの作成

  - **Container Apps 環境** の地域を選択し「**新規作成**」をクリック
<!-- 
    <img src="images/create-container-apps-02.png" />

    ※ 地域は使用するリソース グループ内に展開済みの仮想ネットワークと同じものを選択

    - Container Apps 環境の作成

      - 「**基本**」

        - **環境名**: managedEnvironment-xxx (xxx は任意、既定の名前で OK)

        - **ゾーン冗長**: 無効

        <img src="images/create-container-apps-env-01.png" />

      - 「**監視**」

        - **Log Analytics ワークスペース**: (新規)xxx (xxx は任意、既定の名前で OK)

        <img src="images/create-container-apps-env-02.png" />

        ※ 名前を変更したい場合は、新規作成をクリックしてワークスペース名を入力

      - 「**ネットワーク**」

        - **自分の仮想ネットワークを使用する**: はい

        - **仮想ネットワーク**: ワークショップで使用中のリソース グループに展開済みのものを選択

        - **インフラストラクチャ サブネット**: 新規作成

          - **サブネット名**: Subnet-2

          - **仮想ネットワークのアドレス ブロック**: 既定

          - **サブネット アドレス ブロック**: /23 で指定

            <img src="images/create-container-apps-env-subnet.png" />

        - **仮想 IP**: 外部

        <img src="images/create-container-apps-env-03.png" />

    - 「**Create**」をクリックし Container Apps 環境を作成 -->

  - 「**基本**」

    - **プロジェクトの詳細**

      - **サブスクリプション**: ワークショップで使用するサブスクリプション

      - **リソース グループ**: ワークショップで使用するリソース グループ

      - **コンテナー アプリ名**: 任意 (英小文字、数字、ー (ハイフン) の組み合わせで 32 文字以下)

    - **Container Apps 環境**

      me-1 が選択できることを確認

      <img src="images/create-container-apps-08.png" />

  - 「**アプリ設定**」

    - **クイックスタート イメージを使用する**: チェック

    - **クイックスタート イメージ**: Simple hello world container

      <img src="images/create-container-apps-04.png" />

      ※ 既定の設定で OK

  - 「**確認と作成**」をクリック

- 事前評価で問題がなければ、指定した内容を確認し「**作成**」をクリック

  <img src="images/create-container-apps-04.png" />

- リソースの展開完了後、コンテナー アプリの管理ブレードへアクセス

- 「**アプリケーション URL**」をクリック

  <img src="images/create-container-apps-05.png" />

- Web ブラウザが起動し、アプリケーションの画面を表示

  <img src="images/create-container-apps-06.png" />

<br />

### Task 2: コンテナー レジストリからのイメージ取得

- コンテナー アプリの管理ブレードの左側のメニューから「**リビジョン管理**」を選択

- 「**＋ 新しいリビジョンを作成**」をクリック

  <img src="images/pull-image-from-acr-01.png" />

- 新しいリビジョンの作成とデプロイ

  - 「**コンテナー**」

    - 既存のコンテナー イメージを選択し「**削除**」をクリック

      <img src="images/pull-image-from-acr-02.png" />

    - 「**＋ 追加**」をクリック

      <img src="images/pull-image-from-acr-03.png" />

    - コンテナーの追加

      - **コンテナーの詳細**

        - **名前**: mcw-hol-container (任意)

        - **イメージのソース**: Azure Container Registry

        - **認証**: 管理者資格情報

        - **レジストリ**: Exercise 2 で作成したコンテナー レジストリ

        - **イメージ**: app

        - **イメージ タグ**: v1

        - **OS の種類**: Linux

        - **コマンドのオーバーライド**: 空白 (指定なし)

      - コンテナー リソースの割り当て

        - **CPU コア**: 0.5

        - **メモリ**: 1 Gi

        <img src="images/pull-image-from-acr-04.png" />

        ※ 正常性プローブは既定の設定のままで OK

    - 「**追加**」をクリック

  - 「**スケーリング**」

    - **レプリカの最小数または最大数**: 0 - 10 (既定)

      <img src="images/pull-image-from-acr-05.png" />

- 「**作成**」をクリック

- 「新しいリビジョンが正常にデプロイされました」のメッセージを確認

- コンテナー アプリの管理ブレードの「**概要**」タブの「**アプリケーション URL**」をクリック

  <img src="images/create-container-apps-06.png" />

- Web ブラウザが起動し、アプリケーションの画面を表示

- コンテナー レジストリへプッシュしたアプリケーションが展開されていることを確認

  <img src="images/cs-pull-image-from-acr.png" />

<br />

## Exercise 4: Azure Container Apps の設定

<img src="images/exercise-4.png" />

### Task 1: リビジョン モードの設定

- コンテナー アプリの管理ブレードの左側のメニューから「**リビジョン管理**」を選択

- 「**リビジョン モードの選択**」をクリック

  <img src="images/revision-mode-01.png" />

- 「**複数: 同時に複数のリビジョンをアクティブにする**」を選択し、「**適用**」をクリック

  <img src="images/revision-mode-02.png" />

- 「リビジョンが正常に更新されました」のメッセージを確認

<br />

### Task 2: アプリケーションの更新

- Visual Studio Code を起動

<details>
  <summary>C#</summary>

- Explorer で ASP.NET Core アプリケーションのディレクトリ（「**src**」-「**CS**」-「**Web**」）を展開

- 「**View**」-「**Home**」を展開し、「**Index.cshtml**」を選択

- **Index.cshtml** の 10 行目、11 行目をエディタで編集

  ```
      <img src="~/images/yellow_small.gif" />
      <p>Version 2</p>
  ```

  <img src="images/update-application-02.png" />

- 「**File**」メニューの「**Save**」を選択し、ファイルを保存

- ローカルでアプリケーションの変更を確認するため、ターミナルからコマンドを実行

  ```
  dotnet run
  ```

  ※ カレント ディレクトリが **Web** であることを確認後にコマンドを実行

  <img src="images/update-application-03.png" />

- ターミナルでコマンドを実行し、展開のためのファイル セットをディレクトリへ発行

  ```
  dotnet publish -c Release -o ./bin/Publish
  ```

  <img src="images/update-application-04.png" />

</details>

<details>
  <summary>Java</summary>

- Explorer で Spring Boot アプリケーションのディレクトリ（「**src**」-「**Java**」-「**Web**」）を展開

- 「**src**」-「**main**」-「**resources**」-「**templates**」-「**home**」を展開し、「**Index.html**」を選択

- **Index.html** の 13 行目、14 行目をエディタで編集

  ```
      <img src="images/yellow_small.gif" />
      <p>Version 2</p>
  ```

  <img src="images/update-application-java-02.png" />

- 「**File**」メニューの「**Save**」を選択し、ファイルを保存

- ローカルでアプリケーションの変更を確認するため、VSCode のターミナルからコマンドを実行

  ```
  ./mvnw spring-boot:run
  ```

  ※ カレント ディレクトリが **src\Java\Web** であることを確認後にコマンドを実行

  <img src="images/update-application-java-03.png" />

- ブラウザで変更を確認したら **CTRL+C** でローカル実行を停止

- VSCode のターミナルでコマンドを実行し、展開のためのファイル セットをディレクトリへ発行

  ```
  ./mvnw package
  ```

</details>

- docker 操作用 Linux プロンプトへ移動

  ※ 起動していない場合は、デスクトップ上の Ubuntu ショートカットをダブルクリックして起動

  ※ 起動後、マウントされたディレクトリへ移動

  ```
  cd /mnt/c/Users/AzureUser/Documents/ContainerApps-and-ContainerInstances-HandsOn
  ```

- イメージを構築

  ※ yourreregistry.azurecr.io を作成したコンテナー レジストリのログイン サーバーに変更

  <details>
    <summary>C#</summary>

  ```
  docker build -t yourregistry.azurecr.io/app:v2 -f .docker/CS/dockerfile .
  ```

  </details>

  <details>
    <summary>Java</summary>

  ```
  docker build -t yourregistry.azurecr.io/app:v2 -f .docker/Java/dockerfile .
  ```

  </details>

  <img src="images/update-application-05.png" />

- イメージの確認

  ```
  docker images
  ```

  <img src="images/update-application-06.png" />

- 動作確認

  ※ yourreregistry.azurecr.io を作成したコンテナー レジストリのログイン サーバーに変更

  ```
  docker run --rm -p 8080:80 yourregistry.azurecr.io/app:v2
  ```

<details>
  <summary>C#</summary>

  <img src="images/update-application-07.png" />

</details>

<details>
  <summary>Java</summary>

  <img src="images/update-application-java-07.png" />

</details>

- Web ブラウザを起動し http://localhost:8080 へアクセス

  <img src="images/update-application-08.png" />

- 操作用のプロンプトで Ctrl + C キーを押下し、アプリケーションを終了

<br />

- docker push を使用してレジストリへプッシュ

  ※ yourreregistry.azurecr.io を作成したコンテナー レジストリのログイン サーバーに変更

  ```
  docker push yourregistry.azurecr.io/app:v2
  ```

  <img src="images/update-application-09.png" />

- Azure ポータルで作成したコンテナー レジストリの管理ブレードへアクセス

- 左側のメニューから「**リポジトリ**」を選択

- リポジトリ内のイメージを確認

  <img src="images/update-application-10.png" />

<br />

### Task 3: 新しいリビジョンの展開

- コンテナー アプリの管理ブレードの左側のメニューから「**リビジョン管理**」を選択

- 「**＋ 新しいリビジョンを作成**」をクリック

  <img src="images/new-revision-01.png" />

- 新しいリビジョンの作成とデプロイの「**コンテナー**」タブでコンテナー イメージの名前をクリック

  <img src="images/new-revision-02.png" />

- 「**コンテナーの編集**」で「**イメージ タグ**」を「**v2**」に変更

  <img src="images/new-revision-03.png" />

- 「**保存**」をクリック

- 「**タグ**」が「**v2**」に変更されていることを確認し、「**作成**」をクリック

  <img src="images/new-revision-04.png" />

- デプロイ完了後、複数リビジョンがアクティブ状態であることを確認

  <img src="images/new-revision-05.png" />

  ※ トラフィックは、100% を元のリビジョンに割り当て

- コンテナー アプリの管理ブレードの「**概要**」タブの「**アプリケーション URL**」をクリック

  <img src="images/create-container-apps-06.png" />

- Web ブラウザが起動し、アプリケーションの画面を表示

  - v1 イメージのアプリケーションが表示されることを確認

    <img src="images/cs-pull-image-from-acr.png" />

- コンテナー アプリの管理ブレードの左側のメニューから「**リビジョン管理**」を選択

- 新しく展開したリビジョンに 100% のトラフィックを割り当てるよう変更し、「**保存**」をクリック

  <img src="images/new-revision-06.png" />

- コンテナー アプリの管理ブレードの「**概要**」タブの「**アプリケーション URL**」をクリック

  <img src="images/create-container-apps-06.png" />

- Web ブラウザが起動し、アプリケーションの画面を表示

  - v2 イメージのアプリケーションに変更されたことを確認

    <img src="images/new-revision-cs.png" />

<br />

### Task 4: コンテナー アプリのスケーリング

- コンテナー アプリの管理ブレードの左側のメニューで「**スケーリング**」を選択

- 既定で最小 0、最大 10 のスケール設定が行われていることを確認

  <img src="images/scaling-01.png" />

- 左側のメニューで「**概要**」を選択し「**アプリケーション URL**」をコピー

  <img src="images/load-testing-01.png" />

- リソース グループから展開済みの Load Testing を選択し、管理ブレードへアクセス

- 左側のメニューから「**テスト**」を選択し、「**＋ 作成**」-「**クイック テストを作成する**」をクリック

  <img src="images/load-testing-02.png" />

- テストの詳細で URL とロード パラメーターを指定し「**テストの実行**」をクリック

  - **テスト URL**: コンテナー アプリのアプリケーション URL

  - **仮想ユーザーの数**: 500

  - **テスト期間 (秒)**: 120

  - **増加時間 (秒)**: 20

    <img src="images/load-testing-03.png" />

- テスト スクリプトの生成とテスト エンジンの準備のためしばらく待機

  <img src="images/load-testing-04.png" />

- テストを実行

  <img src="images/load-testing-05.png" />

- テストの完了後、コンテナー アプリの管理ブレードへ移動

- 左側のメニューから「**メトリック**」を選択

- 「**メトリック**」のリストから「**Replica Count**」を選択

  <img src="images/metrics-01.png" />

- 画面右上の時間の範囲・粒度、時間帯の種類を設定

  - **時間の範囲**: 過去 30 分

  - **時間の粒度**: 5 分

  - **公開する時間帯の種類**: ローカル

    <img src="images/metrics-02.png" />

- 設定した内容でグラフが表示

  <img src="images/metrics-03.png" />

  ※ 負荷に応じてスケール アウトが行われていることを確認

<br />

## Exercise 5: Azure Container Instances の作成と実行

### Task 1: バッチ実行用コンテナーイメージの作成とプッシュ

- Visual Studio Code の Explorer で "**.docker**" - "**ACI**" を展開し "**dockerfile** を選択

  - dockerfile の中身を確認

  <img src="images/aci-01.png" />

- デスクトップ上の "**Ubuntu**" ショートカットをダブルクリック

- 操作用のプロンプトが起動

- WSL で Windows 側のマウントされたディレクトリへ移動

  ```
  cd /mnt/c/Users/AzureUser/Documents/ContainerApps-and-ContainerInstances-HandsOn
  ```

- docker build コマンドを実行しイメージを構築

  ```
  docker build -t yourregistry.azurecr.io/batch:v1 -f .docker/ACI/dockerfile .
  ```

- docker push を使用してレジストリへプッシュ

  ```
  docker push yourregistry.azurecr.io/batch:v1
  ```

- Azure ポータルでコンテナー レジストリの管理ブレードへアクセス

- 左側のメニューから「**リポジトリ**」を選択

- リポジトリ内のイメージを確認

  <img src="images/aci-02.png" />

<br />

### Task 2: Azure Container Instances の作成と実行

- Azure Portal でコンテナーインスタンスを検索しクリック

- コンテナーインスタンスの画面で作成をクリック

- コンテナー名は任意 (batch-container など)、地域はリソースグループのリージョンと同じものを選択、SKU は Standard、イメージのソースは自身の Azure Container Registry を選択し、batch:v1 を選択。サイズは 1 vcpu、1.5 GiB メモリ、0 gpu のまま。

<img src="images/aci-03.png" />
<img src="images/aci-04.png" />

- ネットワークタブは既定値のまま進む

<img src="images/aci-05.png" />

- 詳細タブは再起動ポリシーが「失敗時」になっていることを確認し、「確認および作成」をクリック

<img src="images/aci-06.png" />

- その後「作成」をクリックし、Azure Container Instances (コンテナーグループ)を作成する

<img src="images/aci-07.png" />

- 作成されたコンテナーを開き、概要タブで「開始」をクリック

<img src="images/aci-08.png" />

- しばらく経過した後、「設定」のなかの「コンテナー」を選択。ログをクリックし「Test」と表示されていることを確認

<img src="images/aci-09.png" />

<br />

### Task 2 (補足 - Task 2がうまくいかない場合のみ): Azure CLI を利用した、Azure Container Instances の作成と実行

- Azure CLI を起動

- Azure CLI を最初にクリックすると、ストレージの作成が必要になるので、「詳細設定」をクリックし、ストレージアカウントを新規で作成（英数字のみかつ全世界でユニークな名前を入力）し、ファイル共有をひとつ作成する。

- Azure CLI で以下のコマンドを実行する

  ```
  az container create --resource-group <リソースグループ名> --name <任意のコンテナー名> --image yourregistry.azurecr.io/batch:v1 --ports 80 --registry-password password --registry-username <ログインユーザー名>  --location <リージョン名>
  ```

<img src="images/aci-10.png" />

<br />
