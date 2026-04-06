# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

実験・学習用のリポジトリ。各サブディレクトリが独立したフロントエンド制作物（単一HTMLファイル）を格納する。
ビルドツール・パッケージマネージャー・テストフレームワークは使用しない。

**公開URL（GitHub Pages）:**
`https://hirof-des.github.io/claude_test-/<ディレクトリ名>/<ファイル名>.html`

## Projects

### `01/push-notification.html`
3Dパンダが登場するプッシュ通知許可画面。Three.js (r128, CDN) のみを外部依存として使用。
アーキテクチャの詳細は `01/CLAUDE.md` を参照。

### `02/ux-research-simulator.html`
ターゲットユーザーを設定するとダミーインタビューデータとUXインサイトを生成するモックツール。
外部通信なし（Google Fonts読み込みのみ）・APIキー不要・サーバー不要で動作するスタンドアロン版。

**データフロー:**
Step 1（フォーム入力）→ Step 2（モックインタビュー生成）→ Step 3（インサイト抽出）

**モックデータ生成ロジック（`02/ux-research-simulator.html` 内 `<script>`）:**
- `QA_POOL` — 6つの質問テンプレートそれぞれに5〜9種類の回答バリエーションを持つ配列
- `generateInterviews(fd)` — フォームデータ `fd` を受け取り、名前・プロフィール・Q&Aをランダム合成して返す
- `generateInsights()` — `PAIN_POINTS` / `NEEDS` / `QUOTES` / `HMW` プールから各4〜3件をランダム抽出
- `animateProgress()` — Promiseベースの疑似ローディングアニメーション（実際の非同期処理なし）

## Running

HTMLファイルはそのままブラウザで開くだけで動作する。

```bash
open 01/push-notification.html
open 02/ux-research-simulator.html
```

## デザインシステム（`02/` で採用・新規作成時に踏襲）

| 用途 | 値 |
|---|---|
| Primary | `#1a1a1a` |
| Surface | `#f8f7f4` |
| Accent Pain | `#D85A30` |
| Accent Need | `#378ADD` |
| Accent Quote | `#639922` |
| Accent HMW | `#BA7517` |
| フォント | Noto Sans JP（Google Fonts）|
| カード | `border: 0.5px solid rgba(0,0,0,0.1)`, `border-radius: 12px` |
| タグ選択時 | `background: #1a1a1a; color: #fff` |

## Git / GitHub Pages

- `main` ブランチへ push すると GitHub Pages が自動更新される（数分で反映）
- 新しい制作物は番号付きサブディレクトリ（`03/` など）に配置する
