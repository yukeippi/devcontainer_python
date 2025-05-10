# Python 開発環境 (devcontainer)

このリポジトリは、Visual Studio Codeの開発コンテナ（devcontainer）機能を使用したPython開発環境を提供します。

## devcontainer設定

このプロジェクトでは、以下のdevcontainer設定を使用しています。

### `.devcontainer/devcontainer.json`

```json
{
  "name": "Python",
  "build": {
    "dockerfile": "Dockerfile"
  },
  "runArgs": [
    "--name", "python-devcontainer"
  ],
  "features": {
    "ghcr.io/devcontainers/features/common-utils:2": {
      "installZsh": true,
      "installOhMyZsh": true,
      "installOhMyZshConfig": true,
      "configureZshAsDefaultShell": true
    }
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-python.python"
      ]
    }
  },
  "remoteUser": "vscode"
}
```

#### 設定の説明

- `name`: コンテナの名前を「Python」に設定
- `build`: Dockerfileを使用してコンテナをビルド
- `runArgs`: コンテナ実行時の引数（コンテナ名を「python-devcontainer」に設定）
- `features`: 追加機能
  - `common-utils`: Zshとoh-my-zshをインストールし、デフォルトシェルとして設定
- `customizations`: VSCode拡張機能の設定
  - `ms-python.python`: Python拡張機能をインストール
- `remoteUser`: コンテナ内で使用するユーザーを「vscode」に設定

### `.devcontainer/Dockerfile`

```dockerfile
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
```

#### Dockerfileの説明

- ベースイメージ: `python:3.11-slim`（軽量なPython 3.11環境）
- インストールされるパッケージ:
  - `git`: バージョン管理
  - `build-essential`: コンパイルツール（C/C++ライブラリのビルドに必要）
  - `zsh`: Z Shell（高機能なシェル）
  - `curl`: URLを使用してデータを転送するためのツール
- `vscode`ユーザーの作成: コンテナ内で特権のないユーザーとして操作するため
- コンテナサイズを最小限に抑えるため、キャッシュファイルを削除
- コメントアウトされた設定例:
  - 作業ディレクトリの設定
  - Pythonパッケージのインストール方法

## 使用方法

1. Visual Studio Codeをインストール
2. [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)拡張機能をインストール
3. このリポジトリをクローン
4. VSCodeでリポジトリを開く
5. 「Reopen in Container」を選択（右下のポップアップまたはコマンドパレットから）

コンテナ内では、Python 3.11環境が利用可能で、Zshがデフォルトシェルとして設定されています。
