---
title: GCEでDiscordBotを稼働する方法
emoji: 📝
type: tech
topics:
  - GCE
  - DiscordBot
published: false
---

GCE VMインスタンスの作成は
https://qiita.com/ynack/items/03c6af3bb5c04e6ffb56
を参考に無料枠内で作成できます。

# セットアップ手順
package.json
```bash
{
  "name": "discord-bot2",
  "version": "1.0.0",
  "description": "Simple Discord Bot",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "discord.js": "^14.17.3",
    "dotenv": "^16.4.7"
  }
}

```


## 1. システムの準備と Git の導入
まずは権限を確認し、コードを取得するために Git をインストールします。

```bash
# パッケージリストの更新
sudo apt-get update

# Git のインストール
sudo apt-get install git -y

# インストール確認
git --version
```

## 2. GitHub 接続用の SSH 鍵作成
プライベートリポジトリを安全に扱うための鍵を生成

```bash
# SSH 鍵の生成（Ed25519 アルゴリズムを指定）
ssh-keygen -t ed25519 -C "github-test"

# 公開鍵の内容を表示（これをコピーして GitHub に登録）
cat ~/.ssh/id_ed25519.pub

# GitHub との接続テスト
ssh -T git@github.com
```

> 公開鍵を GitHub の [Settings → SSH and GPG keys](https://github.com/settings/keys)のAddNewSSHKeyから登録

## 3. コードの取得と Node.js のインストール
リポジトリをクローンし、実行エンジンである Node.js を導入

```bash
# リポジトリのクローン
git clone git@github.com:ユーザー名/リポジトリ名.git

# NodeSource から Node.js 20 系のセットアップ
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Node.js 本体のインストール
sudo apt-get install -y nodejs

# バージョン確認
node -v
npm -v
```

## 4. Bot の設定と環境構築
最小構成の OS に必要なツールを追加し、Bot の依存関係を解決

```bash
# 依存ライブラリのインストール
cd (リポジトリ名)
npm install

# エディタのインストール（.env 編集用）
sudo apt-get install -y nano

# 環境変数の作成
nano .env
```

> `.env` に Discord トークンや設定値を記述して保存

## 5. 24 時間稼働の設定（PM2）
SSH セッションを閉じても Bot が動作し続けるよう、PM2 で永続化

```bash
# PM2 のインストール
sudo npm install -g pm2

# Bot のバックグラウンド起動
pm2 start npm --name "my-bot" -- run start

```

## 6. リソースの生存確認
ディスク容量が足りているかを確認し、運用に支障がないかチェック

```bash
# ディスク使用量の確認
df -h /
```

---
