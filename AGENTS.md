# AGENTS.md

このドキュメントは、このプロジェクトを操作・理解する AI エージェントのためのコンテキスト情報を提供します。

## プロジェクト概要
Mem0 (OpenMemory) の Docker Compose スタックです。AI エージェントに永続的な記憶能力を提供するためのインフラストラクチャとして機能します。

## インフラストラクチャ構成
- **Orchestration**: Docker Compose (`docker-compose.yml`)
- **Services**:
    - `openmemory-mcp`: FastAPI ベースの MCP サーバー (Port: 8765)
    - `openmemory-ui`: Next.js ベースの管理 UI (Port: 3000)
    - `mem0_store`: Qdrant ベクトルデータベース (Port: 6333)

## 運用コマンド
- **起動**: `docker compose up -d`
- **停止**: `docker compose down`
- **ログ確認**: `docker compose logs -f`
- **リビルド**: `docker compose up -d --build` (設定変更時)

## MCP 接続情報
このサーバーは SSE (Server-Sent Events) ベースの MCP エンドポイントを提供します。

**接続設定例 (mcp_config.json):**
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
*注意: URL パスの `den` はユーザー ID です。`.env` の `USER` 変数と一致させる必要があります。*

## 参照情報源 (References)

本プロジェクトの構築にあたり、以下の情報を参照しました。

| 情報源 | URL | 用途・参照内容 |
| :--- | :--- | :--- |
| **Mem0 GitHub Repository** | `https://github.com/mem0ai/mem0` | **主要ソース**: `openmemory` ディレクトリ内の `docker-compose.yml`, `api`, `ui` のコードベースを取得するために使用。 |
| **Mem0 Documentation** | `https://docs.mem0.ai/` | **仕様確認**: Mem0 のアーキテクチャ、MCP サーバーとしての機能、環境変数の仕様確認に使用。 |
| **Docker Hub** | `https://hub.docker.com/u/mem0` | **イメージ確認**: `mem0/openmemory-mcp` などの公式イメージの存在確認。 |

## ディレクトリ構造の注意点
- `api/` および `ui/` ディレクトリは、公式リポジトリ (`mem0ai/mem0`) の `openmemory` ディレクトリからコピーされたものです。
- 独自のカスタマイズを行う場合を除き、これらのディレクトリ内のコードを直接変更することは推奨されません。設定変更は主に `.env` または `docker-compose.yml` で行います。
