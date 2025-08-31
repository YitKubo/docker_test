# 🧠 GPU対応Docker + VSCode + Git開発環境まとめ

---

## ✅ 1. Gitリポジトリをクローン

```bash
git clone https://github.com/yourname/your-repo.git
cd your-repo
```

---

## ✅ 2. Dockerfileを作成（GPU + Python + PyTorch）

```Dockerfile
FROM nvidia/cuda:12.2.2-cudnn8-devel-ubuntu22.04

RUN apt-get update && apt-get install -y \
    python3.10 python3.10-dev python3-pip git curl && \
    rm -rf /var/lib/apt/lists/*

RUN python3.10 -m pip install --upgrade pip
RUN python3.10 -m pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

WORKDIR /workspace
CMD ["bash"]
```

---

## ✅ 3. Dockerイメージをビルド

```bash
docker build -t my-gpu-dev-env .
```

### コマンド解説

- `docker build` : Dockerイメージをビルド（作成）
- `-t my-gpu-dev-env` : 作成するイメージに `my-gpu-dev-env` という名前（タグ）を付ける
- `.` : 現在のディレクトリ（Dockerfileがある場所）をビルドコンテキストとして指定

---

## ✅ 4. コンテナを起動（Git管理コードをマウント）

```bash
# Docker Composeでコンテナを起動（イメージは docker-compose.yml で指定済み）
docker compose up -d

# コンテナに入る
docker compose exec gpu-dev bash
```

### コマンド解説

- `docker compose up -d` : `docker-compose.yml`の設定でコンテナをバックグラウンド起動
- `docker compose exec gpu-dev bash` : 起動中の`gpu-dev`サービスでbashを実行
- GPU設定、マウント、作業ディレクトリはすべて`docker-compose.yml`で管理済み

---

## ✅ 5. VSCodeからアタッチして編集(起動中のコンテナに入る!!) 

### 🎯 方法1: 自動化（推奨） - Dev Container使用

1. VSCodeで`docker_gpu`フォルダを開く
2. 右下に表示される「**Reopen in Container**」をクリック
3. 自動でコンテナ起動 + 接続完了！

> 📁 `.devcontainer/devcontainer.json` で全て自動化済み

### 🔧 方法2: 手動 - 既存のコンテナにアタッチ

1. 左下「><」クリック → `Attach to Running Container...`
2. `my-dev-container` を選択
3. プロジェクト内のファイルが `/workspace` に見える

#### 🚨 トラブルシューティング

**VSCode Serverのセットアップで止まる場合：**

```bash
# 1. コンテナを再起動
docker compose down
docker compose up -d

# 2. VSCode Serverキャッシュをクリア
rm -rf ~/.vscode-server
docker compose exec gpu-dev rm -rf /root/.vscode-server

# 3. VSCodeを再起動してから再接続
```

---

## ✅ 6. Pythonスクリプト例（`main.py`）

```python
import torch

def main():
    print("GPU available:", torch.cuda.is_available())
    print("Device:", torch.cuda.get_device_name(0))

if __name__ == "__main__":
    main()
```

---

## ✅ 7. `.vscode/launch.json` 作成（デバッグ設定）

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: GPU Debug",
      "type": "python",
      "request": "launch",
      "program": "${workspaceFolder}/main.py",
      "console": "integratedTerminal",
      "python": "/usr/bin/python3.10"
    }
  ]
}
```

---

## ✅ 8. Git操作もそのまま可能！

```bash
git status
git add .
git commit -m "Update"
git push origin main
```

---

## ✅ 認証まわりの注意点

| 方法 | 内容 |
|------|------|
| HTTPS | 初回 push 時にユーザー名 + PAT を入力（2FA必須） |
| SSH  | `-v ~/.ssh:/root/.ssh` でマウント |
| GitHub CLI | `gh auth login` でトークン認証（GUIあり） |

---

## ✅ おすすめ `.gitignore` の設定

```gitignore
.vscode/settings.json
.vscode/tasks.json
```

→ `launch.json` だけ共有するのがチームでは定番

---

## ✅ さらに発展させるには？

- `devcontainer.json` を使ってVSCode起動と同時に環境構築も自動化
- `requirements.txt` や `pyproject.toml` で依存管理
- `JupyterLab` や `TensorBoard` の追加
- 自動テストやCI/CD導入も視野に