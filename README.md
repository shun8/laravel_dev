# laravel_dev
Windows環境にLaravel開発環境を作っていくメモ

この下のblogディレクトリに作成

## 参考
下記リンク先のドキュメントを見ながら
https://readouble.com/laravel/7.x/ja/installation.html

Vagrantのコマンド
https://qiita.com/oreo3@github/items/4054a4120ccc249676d9

## Laravel Homestead
簡単に使える開発環境っぽいので使う
https://readouble.com/laravel/7.x/ja/homestead.html

解説してくれてる記事
https://qiita.com/ricoirico/items/9745160bcf9983fa30ad

### 実施手順メモ
Git for Windowsはインストール済み

Vagrantインストール
https://www.vagrantup.com/downloads

Windows HomeはHyper-V使えない
Virtual Box使う
https://www.virtualbox.org/wiki/Downloads

```PowerShell
vagrant box add laravel/homestead
# 2) virtualboxを選択
```

時間かかった

```PowerShell
git clone https://github.com/laravel/homestead.git C:\Users\<ユーザ名>\Homestead
# ~ではなくフルパス指定する必要あり(普通にやったら~って名前のディレクトリ作成された)
cd ~/Homestead
git checkout release
./init.bat
```

Homestead.yamlを編集

```yaml:Homestead.yaml
# 個人の環境に合わせて変更？
# SSH鍵を設定したり
# プロジェクトのフォルダをmapしたり
# IPアドレスホストマシンのセグメントとかぶらないように変更
ip: "192.168.11.10"

folders:
    - map: <プロジェクトのディレクトリ>
      to: /home/vagrant/code

sites:
    - map: homestead.test
      to: /home/vagrant/code/blog/public
```

```PowerShell
vagrant up
```

起動できた
自宅NWなのでファイアウォール(Windows Defender)は許可した
許可しない場合の不具合あるかは不明

```PowerShell
vagrant ssh
```

接続できた

A5-SQLから接続できた

ServerName=localhost, Port=54320, Database=postgres
user:homestead
pass:secret

ドキュメントの指示通りに`homestead.bat`作ってパス通して、`homestead up`, `homestead ssh`使えるようになった

Laravelプロジェクト作成

```bash
composer create-project --prefer-dist laravel/laravel blog
```

hosts編集した

```hosts
<ip> homestead.test
```

`Homestead.yaml`反映するのに`--provision`オプション必要っぽい

```PowerShell
homestead reload --provision
```

#### 発生した問題メモ

```PowerShell
vagrant up
#エラー
The specified host network collides with a non-hostonly network!
```

ホストマシンと同じセグメントにいるとダメ。IP変更する必要あり
https://medium.com/crunchtimer/%E5%B8%B0%E7%9C%81%E3%81%97%E3%81%9F%E3%82%89homestead%E3%81%8C%E8%B5%B7%E5%8B%95%E3%81%97%E3%81%AA%E3%81%8F%E3%81%AA%E3%81%A3%E3%81%9F%E8%A9%B1-2ac3333fa82b





