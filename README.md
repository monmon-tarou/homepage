# homepage

個人用ホームページを構築するためのリポジトリ
Python（Flask）をベースに、Linux（Rocky Linux 8）環境上で動作する Web アプリケーションとして開発

## 開発環境
- **OS**: bento/rocky Linux-8（Vagrant Box）
- **Virtualization**: VirtualBox "202508.03.0"
- **Language**: Python
- **Framework**: Flask
- **Package Manager**: pip

## 準備
- VScodeのインストール
- forkのインストール
- vagrantのインストール
- VirtualBoxのインストール
- pythonのインストール
- GitHubの登録、homepageリポジトリの作成
- `C:\Users\h-yos`に`dev`フォルダを作成
- homepageリポジトリをforkで`dev`に`clone`

## 実践
1. コマンドプロンプトで homepage ディレクトリに移動する。
2. VM の初期化。vagrant で扱うための VM の設定情報を記載する `Vagrantfile` が作成される。
    ```console
    vagrant init bento/rockylinux-8
    ```
3. Vagrantfile を編集する。
    ```ruby
    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    Vagrant.configure("2") do |config|
      config.vm.box = "bento/rockylinux-8"
      config.vm.box_version = "202508.03.0"

      config.vm.hostname = "homepage"

      config.vm.network "private_network", ip: "192.168.33.10"

      # 同期フォルダの設定
      config.vm.synced_folder "C:/Users/h-yos/dev/homepage", "/vagrant_data"

      # VirtualBox の設定
      config.vm.provider "virtualbox" do |vb|
        vb.name = "homepage"   # VirtualBox 上の VM 名
        vb.memory = 1024       # メモリ 1GB
        vb.cpus = 1            # CPU 1コア
      end
    end
    ```
4. VMを起動する。初up以外では`vagrant up {ID}`で指定して起動可能。
    ```console
    vagrant up
    ```
5. VMに割り当てられたIDを確認する。
    ```console
    vagrant global-status
    ```
6. Linux環境へ接続する。
    ```console
    vagrant ssh {ID}
    ```
7. フォルダが共有されているか確認する。マウントされているかどうか。
    ```console
    cd /vagrant_data
    ```
    ```console
    ll
    ```
8. pythonをインストールする。pipも同時にインストールされる。
    ```console
    sudo dnf install -y python3
    ```
9. `requirements.txt` を作成する。
    ```text
    flask
    ```
10. ライブラリを一括インストールする。今回はFlaskのみ。
    ```console
    pip3 install -r requirements.txt
    ```
11. Windows側のVScodeで編集するため、Windows側にも環境を入れる。
    ```console
    python -m pip install -r requirements.txt
    ```
12. firewalldを無効化する
    ```console
    sudo systemctl disable firewalld
    ```
    ```console
    vagrant reload
    ```
13. `app.py`を作成
    ```python
    from flask import Flask

    app = Flask(__name__)

    @app.route("/")
    def index():
        return "ホームページへようこそ"

    if __name__ == "__main__":
        app.run(host="192.168.33.10", port=5000)
    ```
14. `app.py`を実行する。
    ```console
    python3 app.py
    ```
15. http://192.168.33.10:5000/ にアクセスする。
