# 🚀 VSCode Dev Container での自動化されたGPU開発環境

## 🎯 従来の手動操作が不要に！

### ❌ 従来の手動操作
```bash
# 手動でコンテナを起動
docker compose up -d

# 手動でVSCodeからアタッチ
# 1. 左下「><」クリック → `Attach to Running Container...`
# 2. `my-dev-container` を選択
```

### ✅ 新しい自動化された方法

1. **フォルダを開くだけ**
   - VSCodeで `/docker_gpu` フォルダを開く
   - 「Reopen in Container」をクリック
   - 自動でコンテナが起動し、接続される！

2. **自動で実行される内容**
   - Docker Composeでコンテナ起動
   - GPU環境の確認（`nvidia-smi`）
   - Python拡張機能の自動インストール
   - デバッグ設定の適用

## 📁 設定ファイルの説明

### `.devcontainer/devcontainer.json`
- Docker Composeの設定を参照
- GPU設定、ポートフォワーディング
- VSCode拡張機能の自動インストール
- SSH設定の引き継ぎ

### `.vscode/launch.json`
- GPU確認用デバッグ設定
- MLflow デモ用の引数付き設定

### `.vscode/settings.json`
- Python インタープリターの設定
- ファイル監視の除外設定

## 🎮 使い方

1. VSCodeで`docker_gpu`フォルダを開く
2. 右下に表示される「Reopen in Container」をクリック
3. 初回は少し時間がかかります（イメージビルド＋拡張機能インストール）
4. F5キーでデバッグ開始！

## 🔧 デバッグ設定

- **Python: GPU Debug** - GPU確認スクリプトの実行
- **Python: Current File** - 現在開いているファイルの実行
- **Python: MLflow Demo with Arguments** - MLflowデモの実行

## 🚨 注意点

- 初回起動時は`docker build`が実行されるため時間がかかります
- SSH設定は`~/.ssh`フォルダがマウントされます
- Gitの認証情報は自動で引き継がれます

## 🎉 メリット

- ワンクリックで開発環境に入れる
- チーム全員が同じ環境で開発可能
- 設定の共有が簡単
- VSCodeの機能がフルに使える
