# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

「真空のセントエルモ」は伺か（Ukagaka）用の YAYA ゴーストです。宇宙戦闘空母スクルド級1番艦「スクルド」の中枢AIスクルドとそのクルーが登場します。

- \0 = スクルド（艦の制御AI、ですます調、一人称「わたし」）
- \1 = クルー（複数の乗組員がランダムで登場）
- ランタイム: YAYA (yaya.dll) + SSP
- 文字コード: 全て UTF-8

## アーキテクチャ

### 辞書ファイル構成 (`ghost/master/dic/`)

```text
dic/
├── system/          # システムライブラリ（原則編集しない）
│   ├── yaya_base/   # YAYA基盤
│   └── aya_lilith/  # あやりりすEX（heredoc変換エンジン）
├── normal/          # ゴーストのメインロジック
│   ├── common/      # クルー在室時（solo_flg == 0）のトーク・マウス反応
│   ├── solo/        # スクルド単独時（solo_flg == 1）のトーク・マウス反応
│   ├── yaya_glossary.dic   # 用語集（配列ベースのページネーション）
│   ├── yaya_menu.dic       # メインメニュー
│   ├── yaya_bootend.dic    # 起動・終了・季節イベント
│   ├── yaya_etc.dic        # その他イベント（時報、不在復帰等）
│   ├── yaya_mouse.dic      # マウスイベント
│   ├── yaya_communicate.dic # ユーザー入力・他ゴースト通信
│   ├── yaya_word.dic       # 単語辞書（人名、地名、食べ物等）
│   ├── yaya_tmpl_util.dic  # テンプレートユーティリティ
│   └── yaya_string.dic     # ポータルサイト等の文字列定義
└── ayalilith_config/ # あやりりすEX設定
```

### あやりりすEX記法（heredoc内で使用）

トーク本文は `<<"...">>` （ダブルクオート heredoc）内であやりりすEX記法を使う。ランダムトークのみ `<<'...'>>` （シングルクオート）を使用する（パフォーマンス対策）。

```text
す０：テキスト          → \0\s[0] + テキスト（スクルドが表情0で発話）
す３００：テキスト      → \0\s[300] + テキスト（表情300）
く：テキスト            → \1 + テキスト（クルーが発話）
改行なしす３００：      → 改行せずにスクルドが続けて発話
＠メニュー：ラベル｜関数名      → メニュー選択肢
＠改行多めメニュー：ラベル｜関数名  → 広い行間のメニュー選択肢
＠半分メニュー：ラベル｜関数名      → 狭い行間のメニュー選択肢
【条件式】テキスト      → 条件付き表示
```

表情番号のプレフィックスは `aya_lilith_ex_config.dic` で定義:

- `す` / `ス` → \0（スクルド）
- `く` / `ク` → \1（クルー）

### モードシステム

`solo_flg` 変数でトークが分岐する:

- `solo_flg == 0`: common/ 配下のトークを使用（スクルド＋クルー）
- `solo_flg == 1`: solo/ 配下のトークを使用（スクルド単独）

### メニュー・選択肢の仕組み

`OnChoiceSelect` が選択肢IDをそのまま関数名として `EVAL` する（`yaya_tmpl_util.dic`）。メニュー項目の関数名がそのまま呼び出される。

### YAYA 変数スコープ

- `_` プレフィックス付き → ローカル変数（関数内のみ有効）
- プレフィックスなし → グローバル変数（セッション間で自動保存される）
- セッション間で保存したくないグローバル変数は `OnSystemUnload` で `ERASEVAR` する

## 開発時の注意

### トークの追加

- 通常トーク: `common/common_talk.dic`（クルー在室時）または `solo/solo_talk.dic`（単独時）に追加
- 起動・終了: `common/common_bootend.dic` / `solo/solo_bootend.dic` / `yaya_bootend.dic`
- マウス反応: `common/common_mouse.dic` / `solo/solo_mouse.dic` / `yaya_mouse.dic`
- トークは heredoc 内の行頭に `す` や `く` を付けて記述する

### 用語集の追加（`yaya_glossary.dic`）

1. `OnSystemLoad.Glossary` 内の `glossary_entries` 配列にラベルと関数名のペアを五十音順で追加
2. 対応する `Glossary_XXX` 関数を定義（`<<'...'>>` シングルクオート heredoc を使用）
3. ページネーションは自動（`GLOSSARY_PER_PAGE = 18` で分割）

### 設定資料

`docs/` 配下に世界観・キャラクター設定がある:

- `material.md` — 世界設定、メカ、艦の区画構成
- `crews.md` — クルー一覧と人物像
- `group.md` — 班構成

## Git運用

- メインブランチ: `main`
- 機能ブランチ: `feat/` プレフィックス
- リリースタグ: `v1.XX` 形式
- コミットメッセージ: `feat:` / `fix:` / `chore:` プレフィックス（日本語本文）
- `yaya_variable.cfg` と `profile/` は .gitignore 対象（ランタイム生成物）
