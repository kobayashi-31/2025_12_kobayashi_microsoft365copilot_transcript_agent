# オーケストレーターエージェント - 複数エージェント連携

このプロジェクトは、**Connected Agents（接続エージェント）機能**を使用して、複数の専門エージェント（ワーカーエージェント）を呼び出して応答するオーケストレーターエージェントです。

## 機能概要

- 複数のワーカーエージェントを呼び出して情報を取得・統合
- ユーザーの質問を分析し、適切なエージェントにルーティング
- 各エージェントからの応答を統合して包括的な回答を提供

## 前提条件

> **必要な環境**
>
> - [Node.js](https://nodejs.org/) バージョン: 18, 20, 22
> - [Microsoft 365 開発用アカウント](https://docs.microsoft.com/microsoftteams/platform/toolkit/accounts)
> - [Microsoft 365 Agents Toolkit VS Code 拡張機能](https://aka.ms/teams-toolkit) バージョン 5.0.0 以上
> - [Microsoft 365 Copilot ライセンス](https://learn.microsoft.com/microsoft-365-copilot/extensibility/prerequisites#prerequisites)
> - **接続するワーカーエージェント（事前にデプロイ済みである必要があります）**

## セットアップ手順

### 1. ワーカーエージェントのTitle IDを取得

接続したいエージェントのTitle IDを取得します。Title IDは以下の形式です：
```
T_xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Title IDの取得方法：
- Microsoft 365 Agents Toolkitの **Provision** コマンド実行時の出力
- [デベロッパーモード](https://learn.microsoft.com/microsoft-365-copilot/extensibility/debugging-agents-copilot-studio)でエージェントのメタデータを確認

### 2. 環境変数を設定

`env/.env.local` または `env/.env.dev` ファイルを編集し、ワーカーエージェントのTitle IDを設定します：

```env
WORKER_AGENT_1_TITLE_ID=T_12345678-1234-1234-1234-123456789abc
WORKER_AGENT_2_TITLE_ID=T_87654321-4321-4321-4321-cba987654321
```

### 3. エージェント数の調整

接続するエージェント数を変更する場合は、`appPackage/declarativeAgent.json` の `worker_agents` 配列を編集してください：

```json
"worker_agents": [
    { "id": "${{WORKER_AGENT_1_TITLE_ID}}" },
    { "id": "${{WORKER_AGENT_2_TITLE_ID}}" },
    { "id": "${{WORKER_AGENT_3_TITLE_ID}}" }
]
```

### 4. プロビジョニングと実行

![image](https://github.com/user-attachments/assets/51a221bb-a2c6-4dbf-8009-d2aa20a1638f)

1. VS Codeの左サイドバーで Microsoft 365 Agents Toolkit アイコンを選択
2. Account セクションで Microsoft 365 アカウントにサインイン
3. `Preview Local in Copilot (Edge)` または `Preview Local in Copilot (Chrome)` を選択して起動
4. Copilot アプリからオーケストレーターエージェントを選択
5. 質問を入力すると、接続されたワーカーエージェントを活用して回答します

## 重要な注意事項

⚠️ **ワーカーエージェントのインストール**: 各ワーカーエージェントは、ユーザーに事前にインストールされている必要があります。

⚠️ **通信形式**: エージェント間の通信はテキストのみです。ファイルバイナリや画像は送信できません。

⚠️ **プレビュー機能**: Connected Agents機能は現在プレビュー段階です。

## プロジェクト構成

| フォルダ       | 内容                                                                                 |
| ------------ | ------------------------------------------------------------------------------------ |
| `.vscode`    | デバッグ用VSCodeファイル                                                               |
| `appPackage` | アプリマニフェスト、エージェントマニフェストのテンプレート                                    |
| `env`        | 環境変数ファイル                                                                        |

| ファイル                              | 内容                                                                     |
| ------------------------------------ | ------------------------------------------------------------------------ |
| `appPackage/declarativeAgent.json`   | エージェントの動作設定（worker_agentsを含む）                                |
| `appPackage/instruction.txt`         | エージェントへの指示（プロンプト）                                           |
| `appPackage/manifest.json`           | アプリケーションマニフェスト                                                 |
| `m365agents.yml`                     | Microsoft 365 Agents Toolkit プロジェクト設定                               |

## 参考リンク

- [Connect to other agents from a declarative agent](https://learn.microsoft.com/microsoft-365-copilot/extensibility/declarative-agent-connected-agent)
- [Declarative agent schema v1.6](https://learn.microsoft.com/microsoft-365-copilot/extensibility/declarative-agent-manifest-1.6)
- [Microsoft 365 Agents Toolkit ガイド](https://github.com/OfficeDev/TeamsFx/wiki/Teams-Toolkit-Visual-Studio-Code-v5-Guide#overview)
- [Add API plugins](https://learn.microsoft.com/microsoft-365-copilot/extensibility/build-declarative-agents?tabs=ttk&tutorial-step=7) for agent to interact with REST APIs.

## Addition information and references

- [Declarative agents for Microsoft 365](https://aka.ms/teams-toolkit-declarative-agent)
