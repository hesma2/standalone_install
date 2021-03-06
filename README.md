# standalone_install
スタンドアロン環境にインストールさせるためのyumリポジトリの生成スクリプト

## 動作イメージ
![demo](https://raw.githubusercontent.com/wiki/hesma2/standalone_install/images/standalone_install_demo.gif)

## ファイル構成
### `create_repo.sh`
1. ローカルPC上でCentOSコンテナを立ち上げる
1. CentOSコンテナ上で、パッケージをダウンロードする
1. ダウンロードファイルの置かれたディレクトリをリポジトリに設定する
1. リポジトリに設定したディレクトリをtar圧縮する
1. 圧縮ファイルをローカルPCにコピーする
1. CentOSコンテナを停止、削除する

### `local_install.sh`
1. 圧縮ファイルの解凍
1. リポジトリ設定ファイルの作成
1. パッケージのインストール

### `local_install.repo`
- リポジトリ設定ファイルのテンプレート

## 前提条件
- Dockerがインストールされていること
- ローカルPCがインターネットに接続されていること

## インストール手順
1. ローカルPC（インターネット接続有り）でリポジトリ用ファイルを生成する

    ```
        ./create_repo.sh [インストールするパッケージ名(複数可)]
    ```

    例：

    > `./create_repo.sh git`

2. リポジトリ用ファイルをスタンドアロン環境に転送する

    ```
        scp -i [鍵ファイルの相対パス] ./standalone_install.* [ユーザー名]@[IP/DNS]:[送り先の絶対パス]
    ```

    例：

    > `scp -i ~/.ssh/key/ec2.pem ./standalone_install.* centos@192.168.0.1:/home/centos/`

3. スタンドアロン環境にアクセスし、インストールを実行する

    ```
        cd [ファイルを送ったディレクトリ]
        ./standalone_install.sh [インストールするパッケージ名(複数可)]
    ```

    例:

    > `./standalone_install.sh git`