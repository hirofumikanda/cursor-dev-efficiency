---
marp: true
theme: default
paginate: true
style: |
  section {
    font-size: 28px;
  }
  h2 {
    color: #0066cc;
  }
  table {
    font-size: 22px;
  }
---

# Cursorを使った開発の効率化

---

## 目次

1. **VSCodeと共通**の機能
   - ショートカット
   - Emmet
   - スニペット補完
   - リファクタリング

2. **Cursor特有**の機能
   - 外部リソースで補強する方法
   - 指示方法（AGENTS.md / ルール / スキル）
   - ワークフローの自動化

---

# VSCodeと共通の機能

---

## キーバインドのカスタマイズ

### VSCodeに使用感を合わせるため、以下のとおり設定変更

**Cursor Settings > Keyboard Shortcuts > 右クリック**で変更

参考: [note.com/yudai_sugiyama](https://note.com/yudai_sugiyama/n/nbbf2af7db241)

| 変更内容 | 効果 |
|---------|------|
| `Ctrl + Shift + K`：aipopup.action.model.generate を削除 | 1行削除が効くようになる |
| `Ctrl + M, Ctrl + W` → `Ctrl + K, Ctrl + W` に変更 | 全タブ閉じるが効くようになる |
| `Ctrl + L`：aichat.newchainaction を削除 | ターミナルクリアが効くようになる |

---

## ショートカット（基本操作）

| ショートカット | 機能 |
|---------------|------|
| `Ctrl + Shift + K` | 1行削除 |
| `Ctrl + K`, `Ctrl + W` | 全タブ閉じる |
| `Alt + 矢印` | 1行移動 |
| `Alt + Shift + 矢印` | 1行コピー |
| `Ctrl + Shift + P` | コマンドパレット開く |

---

## ショートカット（編集・ナビゲーション）

| ショートカット | 機能 |
|---------------|------|
| `Ctrl + D` | 選択中の文字列と同じ文字列を選択 |
| `Ctrl + Alt + 矢印` | 下行同じ個所をカーソル複製 |
| `Ctrl + P` | クイックオープン |
| `F12` | 定義に移動 |
| `Shift + F12` | 参照に移動 |

---

## Emmet

HTML・CSSの高速コーディングを実現する省略記法

| 記法 | 意味 |
|------|------|
| `!` | HTML5の基本構造 |
| `>` | 子要素 |
| `+` | 兄弟要素 |
| `*` | 繰り返し |
| HTMLタグ名 | タグのテンプレ |
| CSSセレクタ | CSS記法でHTML生成 |

📖 [docs.emmet.io/cheat-sheet](https://docs.emmet.io/cheat-sheet/)

---

## スニペット補完

- **`if`** など、言語ごとのスニペットが利用可能
- **拡張機能**を活用：`@category:"snippets"` で検索
- **独自スニペット**の定義
  - ファイル > ユーザ設定 > スニペットの構成

---

## リファクタリング

- **シンボルの名前変更**：変数・関数名を一括変更
  - 右クリック > 名前の変更、または `F2`

---

# Cursor特有の機能

---

## 他ツールやドキュメントの利用

### 外部リソースを参照してAIの知識を補強する方法

| 方法 | 特徴 |
|------|------|
| **Docs** | 公式ドキュメントに加えてサードパーティのドキュメントも参照可能 |
| **MCP** | MCPサーバの利用 |

---

## Docs（ドキュメント参照）

- チャット欄に`@docs` symbolでドキュメントをシステムプロンプトに追加可能
- 公式ドキュメント以外に**サードパーティ製のドキュメント**を参照可能
  - **Cursor Settings ＞ Indexing & docs > Code: Add Docs**又はチャット欄で **@docs > Add new doc**で追加
  - 例：Google スタイルガイド（使用言語ごとのベストプラクティス）
    - https://google.github.io/styleguide/
---

## MCP

- MCPサーバの利用追加機能。グローバル設定も可能
- **Cursor Settings ＞ Tools & MCP > New MCP Server**で追加可能（グルーバル配置で作成される）
- [MCPディレクトリ](https://cursor.com/ja/docs/context/mcp/directory)の**Add to Cursor**ボタンを押下することでも追加可能
- プロジェクトの`.cursor/`に配置、またはユーザの`~/.cursor/`（グローバル適用）に配置
  - ファイル名：`mcp.json`

---

## AWSが提供しているMCP

- [awslabs/mcp](https://github.com/awslabs/mcp)
  - 例：AWS Knowledges MCP
    ```json
    {
      "mcpServers": {
        "aws-knowledge-mcp-server": {
          "url": "https://knowledge-mcp.global.api.aws"
        }
      }
    }
    ```
---

## CSVやGeoJSONを分析できるMCP

- [mcp-server-motherduck](https://github.com/motherduckdb/mcp-server-motherduck)
  - DuckDBを利用してSQLクエリを実行できる
    ```json
    {
      "mcpServers": {
        "DuckDB (in-memory, r/w)": {
          "command": "uvx",
          "args": ["mcp-server-motherduck", "--db-path", ":memory:", "--read-write", "--allow-switch-databases"]
        }
      }
    }
    ```
---

## AIへの指示方法

### プロジェクト内又はプロジェクト横断で適用したい共通ルールを設定

| 方法 | 特徴 |
|------|------|
| **AGENTS.md** | 常に適用・最も簡単・すべて自動適用 |
| **ルール** | frontmatterで条件付き適用可能 |
| **スキル** | 適用要否をエージェント自動判断・グローバル配置可能 |

- 参考：https://zenn.dev/redamoon/articles/article38-cursor-skills-rules-commands

---

## AGENTS.md

- **常に適用**される。全て自動適用
- 最も簡単な指示方法
- プロジェクトルートに配置
  - ファイル名：`AGENTS.md`
- 利用例：プロジェクト全体の基本方針など

---

## ルール（Rules）

- **frontmatter**でメタデータ設定・条件付き適用
- **Cursor Settings > Rules, Skills, Subagents > Rules: +New**又はチャット欄で`/create-rule` コマンドで作成
- プロジェクトの`.cursor/rules/`に配置
  - ファイル名：`*.mdc`
- 利用例：特定ディレクトリだけに適用するルールなど

---

## スキル（Skills）

- グローバル配置可能
- **frontmatter**のメタデータから適用要否をエージェントが**自動判断**
- **Cursor Settings > Rules, Skills, Subagents > Skills: +New**又はチャット欄で`/create-skill` コマンドでプロジェクトに作成可能
- プロジェクトの`.cursor/skills/`に配置、またはユーザの`~/.cursor/skills/`（グローバル適用）に配置
  - ファイル名：`SKILL.md`
- Anthropicが標準仕様提供
  - 仕様：https://agentskills.io/specification
  - そのためオープンソースのskillもある
    - Vercel Agent Skills：https://github.com/vercel-labs/agent-skills/tree/main
- 利用例：業界標準のベストプラクティスなど

---

## ワークフロー自動化

### カスタムコマンドを使って定型の処理内容を設定

- よく使う操作をコマンド化して効率化
- **Cursor Settings > Rules, Skills, Subagents > Commands: +New**又はチャット欄に **/create > Create Command**で独自のワークフローを作成可能
- プロジェクトの`.cursor/commands/`に配置
  - ファイル名：`*.md`

---

# まとめ

- **VSCode共通機能**：ショートカット・Emmet・スニペット・リファクタリング機能
- **Cursor特有機能**：Docs/MCP・AGENTS.md/ルール/スキル・カスタムコマンド
- 両方を組み合わせて開発効率を向上
