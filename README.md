# Welcome to ansible_template.d

# What's ansible_template.d
ansibleの公式ドキュメントを元にしたディレクトリテンプレートです。
> https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html

# Getting Started
1. ディレクトリ構成をgit cloneしローカル環境に配置します。
```
$ git clone https://github.com/ishimotty/ansible_template.d.git
``` 

2. 新規にgitリポジトリとして管理するために既存のgit情報を削除します。
```
$ cd ansible_template.d
$ rm -rf .git
```

3. 新規に管理するgitリポジトリとしてgit initします。
```
$ git init
$ git add .
$ git commit -m "1st commit"
```

# Directory Layout
```
01_production             # プロダクション環境用インベントリーファイル
02_staging                # ステージング環境用インベントリーファイル
03_development            # 開発環境用インベントリーファイル

group_vars/
   group1.yml             # インベントリーファイルで定義したグループに変数を割り当てる
   group2.yml             # ファイル名 「{グループ名}.yml」
host_vars/
   hostname1.yml          # インベントリーファイルで定義したホストに変数を割り当てる
   hostname2.yml          # ファイル名 「{ホスト名}.yml」

library/                  # カスタムモジュールを保存するディレクトリ (optional)
module_utils/             # モジュールユーティリティを保存するディレクトリ (optional)
filter_plugins/           # フィルタープラグインを保存するディレクトリ (optional)

site.yml                  # マスターのプレイブック
tmp_servers.yml           # 「site.yml」から読み込まれるサーバーの役割ごとのプレイブック

roles/
    common/               # "role"ごとのディレクトリ
        tasks/            #
            main.yml      #  <-- 実際に実行する処理を記載するファイル
        handlers/         #
            main.yml      #  <-- "handlers" ファイル
                          #      "handlers" : Task上で状態が変更されていた時に1回だけ実行されるジョブの仕組み
        templates/        #  <-- "template" リソースで使用するためのファイル
            ntp.conf.j2   #  <------- テンプレートは「.j2」拡張子で定義される
        files/            #
            bar.txt       #  <-- "copy" リソースで使用するためのファイル
            foo.sh        #  <-- "script" リソースで使用するためのスクリプトファイル
        vars/             #
            main.yml      #  <-- この"role"に関連付けられている変数(group_varsで上書き不可)
        defaults/         #
            main.yml      #  <-- この"role"に関連付けられている変数(group_varsで上書き可)
        meta/             #
            main.yml      #  <-- "role"の依存関係を定義するファイル
        library/          # "role"ごとに組み込むカスタムモジュールを保存するディレクトリ
        module_utils/     # "role"ごとに組み込むモジュールユーティリティを保存するディレクトリ
        lookup_plugins/   # "role"ごとに組み込む"lookup plugin"を保存するディレクトリ

    template_role/        # "common" と同じディレクトリ構造を持ったテンプレート
                          # "role"を増やす場合は複製して使用。
```

# Examples
```
# 本番環境へのプレイブック実行
ansible-playbook -i 01_production site.yml

# ステージング環境へのプレイブック実行
ansible-playbook -i 02_staging site.yml

# 開発環境へのプレイブック実行
ansible-playbook -i 03_development
```
