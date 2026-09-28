<!-- devkit:ghost-template -->
<!-- 上の行は「まだ書き終えていない」しるしです。書き終えたら、上の行とこの行を消してください。 -->

# GHOST.md

このゴーストに固有の情報です。AI エージェントは、作業の前に `AGENTS.md` とあわせて読みます。
作者が自由に書き換えるファイルで、開発キットの更新（`tools/update-devkit.ps1`）で上書きされることはありません。

## このゴーストについて

「真空のセントエルモ」は伺か（Ukagaka）用の YAYA ゴーストです。宇宙戦闘空母スクルド級1番艦「スクルド」の中枢AIスクルドとそのクルーが登場します。

- 名前（`ghost/master/descript.txt` の `name`）:
- キャラクター（`sakura.name` / `kero.name`）:
- 作者:
- 配布先・ネットワーク更新の URL:
- 元にしたテンプレートやゴースト:

## ライセンス

- 辞書:
- シェル: （作者と利用条件。改変した画像の配布や商用利用ができるか）

## 辞書の構成

- 読み込む辞書（`ghost/master/yaya.txt` の `dic` / `dicdir`）: `dic/normal`、`dic/ayalilith_config`（システム辞書 `dic/system` は `system_config.txt` の `dicdir` で最初に読み込む）
- 緊急モード（`yaya_emerg.txt`）:
- システム辞書（`ghost/master/dic/system/` など）: `ghost/master/dic/system/`（submodule ではなく普通のファイル）。上流の yaya-dic から、`_loading_order.txt` の `dicdir, aya_lilith` を有効にしている

`ghost/master/dic/` の構成:

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

## イベントと辞書ファイルの対応

パスは `ghost/master/dic/normal/` からの相対パス。

| ファイル | 主な中身 |
|---|---|
| `common/common_talk.dic` / `solo/solo_talk.dic` | 通常トーク（クルー在室時 / 単独時） |
| `common/common_bootend.dic` / `solo/solo_bootend.dic` / `yaya_bootend.dic` | 起動・終了 |
| `common/common_mouse.dic` / `solo/solo_mouse.dic` / `yaya_mouse.dic` | マウス反応 |
| `yaya_glossary.dic` | 用語集 |
| `yaya_menu.dic` | メインメニュー |
| `yaya_etc.dic` | その他イベント（時報、不在復帰等） |
| `yaya_communicate.dic` | ユーザー入力・他ゴースト通信 |

新しいイベントに反応させるときに書く場所:

## キャラクターとサーフェス

| スコープ | キャラクター | 人物像（一人称、口調、性格） | 使えるサーフェス |
|---|---|---|---|
| `\0` | スクルド | 艦の制御AI、ですます調、一人称「わたし」 | |
| `\1` | クルー | 複数の乗組員がランダムで登場 | |

- 当たり判定（`surfaces.txt` の `collision`）:
- トークで使わないサーフェス（アニメーション用の部品など）:

## トークの書き方

トーク本文は `<<"...">>` （ダブルクオート heredoc）内であやりりすEX記法を使う。ランダムトークのみ `<<'...'>>` （シングルクオート）を使用する（パフォーマンス対策）。トークは heredoc 内の行頭に `す` や `く` を付けて記述する。

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

## 独自のルール

### モードシステム

`solo_flg` 変数でトークが分岐する:

- `solo_flg == 0`: common/ 配下のトークを使用（スクルド＋クルー）
- `solo_flg == 1`: solo/ 配下のトークを使用（スクルド単独）

### メニュー・選択肢の仕組み

`OnChoiceSelect` が選択肢IDをそのまま関数名として `EVAL` する（`yaya_tmpl_util.dic`）。メニュー項目の関数名がそのまま呼び出される。

### グローバル変数

セッション間で保存したくないグローバル変数は `OnSystemUnload` で `ERASEVAR` する。

### 用語集の追加（`yaya_glossary.dic`）

1. `OnSystemLoad.Glossary` 内の `glossary_entries` 配列にラベルと関数名のペアを五十音順で追加
2. 対応する `Glossary_XXX` 関数を定義（`<<'...'>>` シングルクオート heredoc を使用）
3. ページネーションは自動（`GLOSSARY_PER_PAGE = 18` で分割）

### 設定資料

`docs/` 配下に世界観・キャラクター設定がある:

- `material.md` — 世界設定、メカ、艦の区画構成
- `crews.md` — クルー一覧と人物像
- `group.md` — 班構成

### Git運用

- メインブランチ: `main`
- 機能ブランチ: `feat/` プレフィックス
- リリースタグ: `v1.XX` 形式
- コミットメッセージ: `feat:` / `fix:` / `chore:` プレフィックス（日本語本文）
- `yaya_variable.cfg` と `profile/` は .gitignore 対象（ランタイム生成物）
