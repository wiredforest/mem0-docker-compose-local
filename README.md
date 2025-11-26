# Mem0 Docker Stack

Mem0 (OpenMemory) を Docker Compose でローカル環境に構築するためのプロジェクトです。
このスタックは、AI エージェントのための長期記憶層 (Memory Layer) を提供し、MCP (Model Context Protocol) サーバーとして機能します。

## 概要

このプロジェクトは、以下のコンポーネントを Docker コンテナとして立ち上げます：

- **Mem0 API (MCP Server)**: メモリの保存・検索を行うコアサービス。MCP プロトコルで外部エージェントと通信します。
- **Mem0 UI**: メモリの内容を可視化・管理するための Web インターフェース。
- **Qdrant**: ベクトルデータベース。メモリの保存先として使用されます。

## 前提条件

- Docker
- Docker Compose
- OpenAI API Key

## セットアップ手順

1.  **環境変数の設定**:
    `.env.example` をコピーして `.env` を作成し、必要な値を設定してください。

    ```bash
    cp .env.example .env
    # .env を編集して OPENAI_API_KEY を設定
    ```

2.  **起動**:
    以下のコマンドでスタックを起動します。

    ```bash
    docker compose up -d
    ```

3.  **確認**:
    - **UI**: [http://localhost:3000](http://localhost:3000) にアクセス。
    - **API Docs**: [http://localhost:8765/docs](http://localhost:8765/docs) にアクセス。

## MCP 接続設定

このサーバーを MCP クライアント（Windsurf, Claude Desktop など）から利用する場合の接続設定例です。

```json
{
  "mcpServers": {
    "mem0": {
      "command": "npx",
      "args": [
        "-y",
        "supergateway",
        "--sse",
        "http://localhost:8765/mcp/windsurf/sse/den"
      ]
    }
  }
}
```

## ディレクトリ構造

- `api/`: Mem0 API サーバーのソースコード（Upstream からのコピー）
- `ui/`: Mem0 UI のソースコード（Upstream からのコピー）
- `docker-compose.yml`: コンテナ構成定義

## ⚖️ License
Apache 2.0 — see the [LICENSE](./LICENSE) file for details.

## Third-Party Code

The `upstream/` directory contains unmodified code from the [Mem0 project](https://github.com/mem0ai/mem0),
licensed under Apache License 2.0. See `upstream/LICENSE` for details.
