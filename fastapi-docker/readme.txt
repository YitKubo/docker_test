mkdir myapp && cd myapp
# → ファイル作成（app.py, Dockerfileなど）

docker compose up --build -d  # 起動
docker compose ps             # 状態確認
docker compose logs -f        # ログ監視
docker compose exec web bash  # 中に入る
docker compose restart        # 再起動
docker compose down           # 停止