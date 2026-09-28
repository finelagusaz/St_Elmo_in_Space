# GHOST.md

このゴーストに固有の情報です。AI エージェントは、作業の前に `AGENTS.md` とあわせて読みます。
作者が自由に書き換えるファイルで、開発キットの更新（`tools/update-devkit.ps1`）で上書きされることはありません。

## このゴーストについて

「真空のセントエルモ」は伺か（Ukagaka）用の YAYA ゴーストです。宇宙戦闘空母スクルド級1番艦「スクルド」の中枢AIスクルドとそのクルーが登場します。

- 名前（`ghost/master/descript.txt` の `name`）: 真空のセントエルモ
- キャラクター（`sakura.name` / `kero.name`）: スクルド / クルー
- 作者: Fine Lagusaz（https://blankrune.sakura.ne.jp/）
- 配布先: https://blankrune.sakura.ne.jp/
- ネットワーク更新の URL: http://blankrune.sakura.ne.jp/named/St_Elmo_in_Space/update/（`dic/normal/yaya_homeurl.dic` と `dic/emerg/yaya_homeurl.dic`）
- 元にしたテンプレートやゴースト: konnoyayame
- シェル: `shell/master`（Master）、`shell/master_large`（Master Large）

## ライセンス

作者以外の人（このゴーストを改造する人、ほかのゴーストに流用する人、その人が使うエージェント）が守る条件です。作者が依頼する編集は、シェルの画像を除いて制限しません。

- 辞書:
  - 開発キット、ベース辞書（konnoyayame、システム辞書）: それぞれのライセンスに従う（`yaya.dll` は `ghost/master/readme.txt`、文 は `ghost/master/readme-original.txt`）
  - 作者が作成した部分: トークとマウス反応を除いて、改変・流用してよい。トークとマウス反応（台詞）は、改変も流用もしない
- シェル: サーフェスはゆみるさんの作画（`shell/master/readme.txt`）。改変も、改変した画像の配布もできない。作者を含めて誰も画像を編集しない

## 辞書の構成

- 読み込む辞書（`ghost/master/yaya.txt` の `dic` / `dicdir`）: `dic/normal`、`dic/ayalilith_config`（システム辞書 `dic/system` は `system_config.txt` の `dicdir` で最初に読み込む）
- 緊急モード（`yaya_emerg.txt`）: `dic/emerg`（`yaya_emerg_dic.dic`、`yaya_homeurl.dic`）
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
│   ├── yaya_aitalk.dic     # ランダムトークの振り分け（solo_flg で common / solo を選ぶ）
│   ├── yaya_change.dic     # ゴースト切り替え時のトーク
│   ├── yaya_glossary.dic   # 用語集（配列ベースのページネーション）
│   ├── yaya_menu.dic       # メインメニュー
│   ├── yaya_bootend.dic    # 起動・終了・季節イベント
│   ├── yaya_etc.dic        # その他イベント（時報、不在復帰、インストール、ネットワーク更新等）
│   ├── yaya_mouse.dic      # マウスイベント
│   ├── yaya_communicate.dic # ユーザー入力・他ゴースト通信
│   ├── yaya_homeurl.dic    # ネットワーク更新の URL
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
| `yaya_change.dic` | ゴースト切り替え（`自分へ変更` / `〜から変更` など） |
| `yaya_glossary.dic` | 用語集 |
| `yaya_menu.dic` | メインメニュー |
| `yaya_etc.dic` | その他イベント（時報、不在復帰、`OnInstall*`、`OnUpdate*`、`OnVanish*` 等） |
| `yaya_communicate.dic` | ユーザー入力（`ユーザーコミュ〜`。「ふたりっきり」で `solo_flg = 1`、「またあとで」で `solo_flg = 0`）・他ゴースト通信 |

新しいイベントに反応させるときに書く場所: 上の表に当てはまらないものは `yaya_etc.dic`

## キャラクターとサーフェス

| スコープ | キャラクター | 人物像（一人称、口調、性格） | 使えるサーフェス |
|---|---|---|---|
| `\0` | スクルド | 艦の制御AI。ですます調、一人称「わたし」、二人称「あなた」、三人称は真面目なときは階級呼び・オフのときはさん付け。ユーザーは `%username` / `%usernameさん` と呼ぶ。性格は `docs/material.md` の「傾向」 | 下の表 |
| `\1` | クルー | 複数の乗組員がランダムで登場。台詞の頭に `<名前>` を付ける（`く：<ブラト> 〜`）。乗組員は `docs/crews.md` | `10` のみ（`surface10.png`、あやりりすEX の標準表情1） |

`\0` のサーフェス（`shell/master/surfacetable.txt`）。番号は「ポーズ（百の位）＋表情（下 2 桁）」で、どのポーズにも同じ 14 の表情がある。

| ポーズ | 番号 |
|---|---|
| 立 | 0〜9、11〜14 |
| 手重ね | 200〜209、211〜214 |
| 右手襟元 | 300〜309、311〜314 |
| 首傾げ | 400〜409、411〜414 |
| 手重ね首傾げ | 500〜509、511〜514 |
| 右手襟元首傾げ | 600〜609、611〜614 |

| 下 2 桁 | 表情 | 下 2 桁 | 表情 |
|---|---|---|---|
| 00 | 素 | 07 | 怒り |
| 01 | 照れ | 08 | 困り |
| 02 | 驚き | 09 | 照れ怒り |
| 03 | 不安 | 11 | ＞＜ |
| 04 | 落ち込み | 12 | 困り目そらし |
| 05 | 笑顔 | 13 | 照れ微笑 |
| 06 | 目閉じ | 14 | 照れ目そらし |

- `\0` には下 2 桁が 10 の番号（10、210 など）は無い。10 は `\1` のクルーのサーフェス。
- 当たり判定（`surfaces.txt` の `collision`）: 全サーフェス共通で `leg`（足）、`skirt`（スカート）、`bust`（胸）、`shoulder`（肩）、`lip`（唇）、`head`（頭）、`hand`（手）。手と肩の位置はポーズごとに違う
- トークで使わないサーフェス（アニメーション用の部品など）: `surface1000`〜`1015`（まばたき）、`surface1100`〜`1112`（口パク）

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

例（`common/common_talk.dic`）:

```text
す９：中尉、やめてください。
く：<中尉> いいじゃないか、少しぐらい。
す８：それは、少しではありません。
く：<中尉> 故郷の味なんだぜ？ このテラ盛り焼きそば。
す３：食事の時間にお食べください。
く：<中尉> おかんか、お前は。
```

あやりりすEX を使わずにさくらスクリプトで直接書くところ（`yaya_communicate.dic` など）では、読点の後に `\w4`、文末に `\w9` を置いている。

スクルドの台詞は `docs/material.md` の「\0 スクルド」→「傾向」に沿って書く。要点:

- 事実に基づいて話す。戦闘や作戦の話では冷静
- 感情はフラットで、表情に出さないようにしている。ただし、思うところはあり、たまに毒を吐く
- 艦内の全員と話すので、しょうもない話も山ほど持っているが、口は堅い

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
