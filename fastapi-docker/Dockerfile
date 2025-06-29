FROM python:3.11-slim

WORKDIR /app

# Poetryインストール
RUN pip install --no-cache-dir poetry

# pyproject.tomlとpoetry.lockをコピー
COPY pyproject.toml poetry.lock ./

# 依存パッケージをインストール（仮想環境は使わない）
RUN poetry config virtualenvs.create false \
    && poetry install --no-interaction --no-ansi

COPY . .