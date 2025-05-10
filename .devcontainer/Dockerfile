# ベースイメージを指定
FROM python:3.11-slim

# 必要なパッケージをインストール
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    build-essential \
    zsh \
    curl \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# 必要に応じてユーザー追加や権限設定
# 例: VSCode公式devcontainerの慣例に合わせてvscodeユーザーを使う場合
RUN useradd -m vscode

# 作業ディレクトリを設定（省略可。devcontainer.jsonでworkspaceFolderを指定する場合は不要）
# WORKDIR /workspace

# 必要なPythonパッケージのインストール例
# COPY requirements.txt .
# RUN pip install --no-cache-dir -r requirements.txt
