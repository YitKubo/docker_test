# 🐳 Docker・Poetry・Pythonプロジェクト管理 まとめ

---

## 1. Dockerの基本

- **イメージ**：アプリや環境の設計図＋必要なファイル一式（サイズあり）
- **コンテナ**：イメージから作られた実行中の実体（動的）

### よく使うコマンド

| コマンド例 | 説明 |
|---|---|
| `docker images` | イメージ一覧表示 |
| `docker ps -a` | すべてのコンテナ一覧 |
| `docker compose ps` | Composeサービス一覧 |
| `docker run ...` | イメージから直接コンテナ起動 |
| `docker compose up --build -d` | ビルド＆バックグラウンド起動 |
| `docker compose logs -f` | リアルタイムログ表示 |
| `docker compose exec app bash` | コンテナ内に入る |
| `docker compose restart` | 再起動（設定変更なし） |
| `docker compose stop` / `start` | 一時停止・再開 |
| `docker compose down` | コンテナとネットワーク削除 |
| `docker compose down --volumes` | ボリュームも含めて完全削除 |

---

## 2. Dockerfileとdocker-compose.ymlの違い

- **Dockerfile**：イメージ（設計図）を作るためのファイル
- **docker-compose.yml**：複数コンテナの起動・連携・管理をまとめるファイル

---

## 3. Python依存管理

- **requirements.txt**：シンプルな依存リスト。見やすいが機能は限定的
- **pyproject.toml（Poetry）**：厳密な依存管理・プロジェクト情報も記述できる「清書」的な存在
- **セット運用も可**：Poetryで管理し、`poetry export`でrequirements.txtを生成する運用も一般的

---

## 4. Dockerでできること（あなたのプロジェクト例）

- 開発環境の再現・自動化
- 依存管理の自動化（Poetryやpip）
- FastAPIアプリの即時起動・テスト
- 本番環境へのデプロイ
- 複数サービスの一括管理
- CI/CDとの連携

---

## 5. トラブルシューティング

- **ファイル同期がうまくいかない場合**  
  - 個別ファイルマウント（`- ./poetry.lock:/update_poetry/poetry.lock` など）が有効な場合もある
  - ホスト側に空ファイルを事前作成・権限確認
  - ディレクトリ全体マウントと個別ファイルマウントの併用は避ける

---

## 6. 便利な追加コマンド

| コマンド例 | 説明 |
|---|---|
| `docker image prune` | 未使用イメージ削除 |
| `docker system prune` | 未使用リソース一括削除 |
| `docker stats` | リソース使用状況確認 |
| `docker compose pause` / `unpause` | 一時停止・再開 |
| `docker compose up --scale app=3 -d` | サービスを複数起動 |

---

**このまとめを参考に、Docker・Poetry・Pythonプロジェクトを効率よく管理してください！**
