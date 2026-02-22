# homepage

個人用ホームページを構築するためのリポジトリ
Python（Flask）をベースに、Linux（Rocky Linux 8）環境上で動作する Web アプリケーションとして開発

## 開発環境
- **OS**: bento/rocky Linux-8（Vagrant Box）
- **Virtualization**: VirtualBox "202508.03.0"
- **Language**: Python 3.14
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
    vagrant init bento/centos-8
    ```
3. Vagrantfile を編集する。
    ```ruby
    # -*- mode: ruby -*-
    # vi: set ft=ruby :

    Vagrant.configure("2") do |config|
      config.vm.box = "bento/rockylinux-8"
      config.vm.box_version = "202508.03.0"

      config.vm.hostname = "homepage"

      config.vm.network "private_network", ip: "192.168.56.30"
      config.vm.network "forwarded_port", guest: 5000, host: 5000

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
8. `requirements.txt` を作成する。
    ```text
    flask
    ```
9. ライブラリを一括インストールする。今回はFlaskのみ。
    ```console
    pip install -r requirements.txt
    ```
