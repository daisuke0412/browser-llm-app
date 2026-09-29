# Browser LLM App

[WebLLM](https://webllm.mlc.ai/) をブラウザ上でお試しで動かすためのアプリです。React / TypeScript / Vite で作成しています。

**公開アプリ:** https://browser-llm-app-pearl.vercel.app/

## できること

- モデルを選択して読み込み、システムプロンプトとメッセージを入力して推論を実行する。
- 任意の JSON Schema を指定して、構造化出力を試す。
- 生成結果や実行ログを確認する。

WebLLM の推論はブラウザ内で実行されるため、API キーは不要です。WebGPU 対応のブラウザ・GPU 環境が必要です。初回はモデルのダウンロードが発生し、モデルによって通信量やメモリ使用量が大きくなります。ダウンロード済みのモデルはキャッシュされます。

## ローカルで起動

Node.js と pnpm を用意し、このプロジェクトのディレクトリで実行してください。

```powershell
pnpm install
pnpm dev
```

ターミナルに表示される URL をブラウザで開いてください。

## ディレクトリ構成

```text
browser-llm-app/
├── src/                   # ブラウザアプリ本体
│   ├── main.tsx           # アプリの起動
│   ├── App.tsx            # ルートコンポーネント
│   ├── components/        # 入力・出力・ログなどの画面コンポーネント
│   ├── hooks/             # モデルの読み込みと推論実行の処理
│   ├── workers/           # WebLLM を動かす Web Worker
│   └── utils/             # 非同期処理などの共通ユーティリティ
├── scripts/               # Claude / OpenAI API を使った検証用の Python スクリプト
│   ├── call_claude.py     # Claude にサンプルのプロンプトを送信し、応答と処理時間を表示
│   ├── call_openai.py     # OpenAI にサンプルのプロンプトを送信し、応答を表示
│   └── evaluate.py        # WebLLM の出力を Claude / OpenAI で採点し、未採点の項目を更新
├── logs/
│   └── result_io.json     # 評価対象と結果を保存
└── tests/                 # アプリのテスト
```

## Claude / OpenAI スクリプトの実行

Python 環境を用意し、必要な SDK をインストールしてください。以下はプロジェクトのディレクトリで実行する例です。

```powershell
python -m pip install anthropic openai
```

**環境設定ファイルによる API キーの読み込みはありません。** CLI などで、実行するターミナルの環境変数に設定してください。

| 実行対象 | 必要な環境変数 |
| --- | --- |
| Claude | `ANTHROPIC_API_KEY` |
| OpenAI | `OPENAI_API_KEY` |
| 両方の API による評価 | 上記の両方 |
