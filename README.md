# test_wordpress

# Composerを使ってWordPressを立ち上げてみる

## この説明で紹介すること

* Gitを使ってバージョン管理します。
* Editorconfigを使ってエディタの設定を共有します。
* Composerを使ってWordPressをインストールします。
* PHPのビルトインサーバを使って簡単な動作確認をします。
* GitHubで公開し、ソースコードを共有します。

※今回の内容に、LAMP環境の構築手順は含みません。
Composerを使ってWordPressを設置する手順のみ紹介します。

## Git
https://git-scm.com/

超有名なバージョン管理ツールです。
詳しくは「サルでもわかるGit入門」を見てください。

## GitHub
https://github.com/

GitHubは、いろんなプロジェクトのためにGitのリポジトリをホスティングするサービスです。
公開プロジェクトなら無料で利用できます。

## ATOM
https://atom.io/

GitHub製の高機能テキストエディタです。オープンソースです。
WindowsでもMacでも使えます。
豊富なプラグインで自分好みにカスタマイズできます。

## Editorconfig
http://editorconfig.org/

プロジェクトフォルダ内に作成するファイルの、
文字コード・改行コード・インデントなどをチーム内で統一できます。
※ただし対応するエディタとプラグインの有効化が必要です。

## composer
https://getcomposer.org/

PHPのパッケージ管理ツールです。依存関係も自動で解消してくれます。
当然、利用環境にはPHPがインストールされていることが条件となります。



# 構築手順

GitHubでリポジトリ作成。

projectフォルダを作成

    $ cd ~/
    $ mkdir test_wordpress
    $ cd test_wordpress

今回はAtomを使います。

    メインメニュー > File > Add Project Folder

初期化して初回プッシュ

    $ touch README.md
    $ echo # test_wordpress >> README.md
    $ git init
    $ git add README.md
    $ git commit -m "first commit"
    $ git remote add origin https://github.com/OsamuKubomoto/test_wordpress.git
    $ git push -u origin master

developブランチを作成

    $ git branch develop
    $ git checkout develop

無視リスト作成

    $ touch .gitignore

Git更新

    $ git add .
    $ git commit -m ".gitignoreを追加"

editorconfigを追加

    $ vi .editorconfig
※Windowsエクスプローラで隠しファイルを作る場合は最後に.を入れればOK

    root = true

    [*]
    charset = utf-8
    end_of_line = lf
    indent_style = space
    indent_size = 4
    trim_trailing_whitespace = true
    insert_final_newline = true

    [*.css]
    indent_style = space
    indent_size = 2

    [*.scss]
    indent_style = space
    indent_size = 2

Git更新

    $ git add .
    $ git commit -m "editorconfigを追加"

composerファイルをダウンロード

    $ curl -sS https://getcomposer.org/installer | php

.gitignoreを編集

    composer.phar
    ※上記1行を追加して、composer.pharをGitで無視させます。

Git更新

    $ git add .
    $ git commit -m "composer.pharを無視"

初期化

    $ php composer.phar init
    ※composer.jsonと.gitignoreが生成されます。

Git更新

    $ git add .
    $ git commit -m "composerを初期化"

WordPressをインストール

    $ php composer.phar require johnpbloch/wordpress
    ※wordpressパッケージがダウンロードされます。同時に依存パッケージもvendor内に配置されます。

wordpressはGit上でパッケージ管理するため、Composerの管理対象から外す。

    $ git checkout composer.json
    $ rm composer.lock

Git更新

    $ git add .
    $ git commit -m "wordpressを追加"

.gitignoreを編集

    /wordpress/*.log
    /wordpress/.htaccess
    /wordpress/sitemap.xml
    /wordpress/sitemap.xml.gz
    /wordpress/wp-config.php
    /wordpress/wp-content/advanced-cache.php
    /wordpress/wp-content/backup-db/
    /wordpress/wp-content/backups/
    /wordpress/wp-content/blogs.dir/
    /wordpress/wp-content/cache/
    /wordpress/wp-content/upgrade/
    /wordpress/wp-content/uploads/
    /wordpress/wp-content/wp-cache-config.php
    /wordpress/wp-content/plugins/hello.php

    /wordpress//readme.html
    /license.txt

Git更新

    $ git add .
    $ git commit -m "wordpressフォルダ内の無視リストを追加"

## phpビルトインサーバを起動して動作確認

    $ cd wordpress
    $ php -S localhost:8080
    ※Ctrl+Cで停止
    $ cd ..

masterブランチに戻る

    $ git checkout master
    $ git merge develop

GitHubにPushする

    $ git push

## ゼロからデプロイする

一度ローカル環境のプロジェクトを削除してgit cloneする

    $ git clone https://github.com/OsamuKubomoto/test_wordpress.git

## phpビルトインサーバを起動して動作確認

    $ cd wordpress
    $ php -S localhost:8080
    ※Ctrl+Cで停止

以上、お疲れさまでした。
