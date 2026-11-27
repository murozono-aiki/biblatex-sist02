# カスタムフィールド・文字種判定・対応エントリタイプ詳細

[`sist02.dbx`](../sist02.dbx) で定義されているカスタムフィールド、言語・文字種判定ルール、`biblatex-sist02` がサポートする全エントリタイプと `.bib` 記述例をまとめています。

[← README に戻る](../README.md)

---

## 1. `sist02.dbx` カスタムフィールド一覧

| フィールド名 | 型 | 対象 SIST 02 節 | 用途と記述例 |
|---|---|---|---|
| `medium` | literal | 4.4.3 媒体表示 | `CD-ROM`, `DVD`, `online`, `マイクロフィッシュ` 等。出力時は自動で丸括弧 `(online)` 等で囲まれるため、値に二重括弧を付ける必要はありません。 |
| `availablesource` | literal | 4.4.4 入手先 | `CiNii`, `SpringerLink`, `東京大学学術機関リポジトリ`, `GitHub` 等。和文では `入手先, ...`、欧文では `available from ...` が自動前置されます。 |
| `accesslabel` | literal | 4.4.5 入手日付 | `参照`, `入手`, `accessed`, `cited` 等。エントリ個別に `urldate` のラベルを指定したい場合に使用し、パッケージオプション `urldatelabel` より優先されます。 |
| `origlangdisplay` | literal | 4.4.1 言語の表示 | `ロシア語`, `Japanese`, `ru` 等。`language` フィールドの自動出力文字列を上書きしたい場合にのみ使用します。 |
| `authorstyle` | literal | 4.1.1 著者名 | `japanese` または `western`。エントリ内のすべての人名表記を一括で上書きします。 |
| `editionliteral` | literal | 4.3.1 版表示 | `true` を指定すると、`edition` が数字のみの場合でも `1` の省略や `第○版` / `ed.` 補完を行わず、`edition` の値をそのまま出力します（例: 年号風の版名 `2025` や固有版名 `1`）。 |
| `updatedate` | date (ISO) | 4.3.4(4), 5.10–5.13 | `2026-11-26` 等（範囲日付も可）。ウェブサイト、メーリングリスト、データベース、ソフトウェアの更新日付を記録します。 |
| `commit` | literal | 5.13 ソースコード | `a1b2c3d` 等。`@software` エントリにて `Version 1.0 (commit a1b2c3d)` または `commit a1b2c3d` の形式で出力します。 |

> [!NOTE]
> `url` フィールドは `sist02.bbx` により `\url{#1}`（`url.sty` 標準の等幅 URL 表記）として描画されます。  
> [BibLoom](https://bibloom.riyoddoo.com/) では `edition` 欄に数字のみが入力されたときにのみ、入力欄の直下へ「数字のまま版名として出力する（`editionliteral`）」チェックボックスが表示されます。

---

## 2. SIST 02 完全記述で重要な標準 bibLaTeX フィールド

| フィールド名 | 用途 | 出力仕様 |
|---|---|---|
| `issuetitle` | 5.1.2 特集標題 | `特集標題: 論文名.` の形式で出力 |
| `pubstate` | 5.9 刊行状態 | `submitted`, `accepted`, `inpress`, `forthcoming`, `prepublished` を和文（`投稿中`, `受理済`, `印刷中`, `近刊`, `早期公開`）または欧文表記へ自動変換 |
| `holder` | 5.7 特許出願人・特許権者 | 発明者 `author` と組み合わせて出願人・権者を出力 |
| `eventdate` | 5.5 会議開催期間 | `2006-01-10/2006-01-12` のように同月・同年範囲を `2006-01-10/12` 等へ自動短縮 |
| `eid` | 5.1.1 電子雑誌の記事番号 | `p. ...` の代わりに記事番号 `e374` 等を出力 |
| `pagetotal` / `volumes` | 4.3.12 総ページ数 / 総冊数 | 和文 `573p.`, `12冊` / 欧文 `573p.`, `12 vols.` 形式で出力 |
| `translator` / `editor` | 訳者・編者 | 訳者名＋`訳` / `trans.`、編者名＋`編` / `ed.` を付与 |

---

## 3. 言語判定と `langid` / `language` / `origlangdisplay` の使い分け

### 3.1 エントリ全体の記述言語判定

1. `langid` フィールドが明示されている場合（`japanese` / `ja` → 和文、それ以外 → 欧文）はそれを最優先します。
2. `langid` が未指定の場合、`title` → `booktitle` → `journaltitle` → `maintitle` の順にひらがな・カタカナ・漢字の有無から自動判定します。すべて空の場合は `language` を参照します。

### 3.2 3 フィールドの役割分担

1. **`langid`（参照文献リストの記述言語・書式制御）**: 句読点、役割語（`編` / `ed.`、`訳` / `trans.`）、入手日付ラベル（`参照` / `accessed`）を和文・欧文のどちらで出力するかを決定します。通常はタイトルからの自動判定で十分ですが、ローマ字翻字した和文文献などを欧文形式で出したい場合にも明示します。
2. **`language` / `origlanguage`（原資料の言語メタデータ・翻訳時注記）**: 原資料そのものが何語で書かれているかを保持します。`sist02.bbx` は**記述言語（`langid` 等）と `language` が異なる場合**（例: 欧文リストに和文文献をローマ字で載せる `langid = {english}, language = {Japanese}`）にのみ末尾へ自動で `(Japanese)` を出力し、同じ言語であれば冗長な表示を行いません。
3. **`origlangdisplay`（言語表示文字列の任意上書き）**: `language = {russian}` のような標準キーを保ちつつ、画面出力を `(ロシア語)` や `(ru)` にしたい場合など、表示テキストを明示的に変えたいときに使用します。

---

## 4. 著者名の自動文字種判定と明示上書き

### 4.1 制作者ごとの自動文字種判定（Per-name Unicode Detection）

和文著者・カタカナ外国人名・欧文著者が 1 つの文献に混在していても、制作者一人ひとりの文字列から自動判定を行います。

| 条件 | 出力形式 | 複数名区切り | 例 |
|---|---|---|---|
| 組織名・単一名（`{...}` で囲む、または名がない） | `団体名`（コンマなし） | 和文内 `, ` / 混在・欧文 `; ` | `{DeepMind Research}` |
| 漢字または平仮名を含む | `姓名`（スペースなし） | 和文同士は `, ` | `森, 康夫` → `森康夫` |
| ラテン文字のみ、または**カタカナのみ** | `姓, 名` | 前後いずれかが欧文・カタカナなら `; ` | `ケネディ, ジョン F.` / `van Breemen, Nico` |

### 4.2 Biber アノテーション (`<role>+an`) と `authorstyle`

カタカナのみの日本人名（例: `タナカ, タロウ` → `タナカタロウ`）や、漢字氏名でも外国人名扱いにしたい場合は、Biber データアノテーション `<role>+an`（`author+an`, `editor+an`, `translator+an` 等）で個別に上書きできます。

```bibtex
@article{annot2026,
  author    = {タナカ, タロウ and 佐藤, 花子 and {DeepMind Research}},
  author+an = {1=japanese; 2=western},
  title     = {著者名個別アノテーション検証論文},
  journal   = {情報処理学会論文誌},
  year      = {2026},
  volume    = {67},
  number    = {3},
  eid       = {e104},
  doi       = {10.1234/ipsj.2026.03}
}
```

- 指定可能な値: `japanese`（スペースなし連結）、`western` / `foreign` / `katakana`（`姓, 名` 形式）、`literal`（単一・団体名）
- エントリ内の全人名を一括で切り替える場合は `authorstyle = {japanese}` または `authorstyle = {western}` を指定します。

---

## 5. 対応エントリタイプ一覧（全 36 種）

| SIST 02 分類 | 推奨タイプ | エイリアス・自動切替仕様 |
|---|---|---|
| 5.1 雑誌論文（通常・特集記事） | `@article` | `review`, `suppperiodical`（`issuetitle` 指定で特集形式） |
| 5.2.1 図書 1 冊 | `@book` | `mvbook`, `booklet`, `collection`, `mvcollection`, `reference`, `mvreference`, `periodical`, `commentary` |
| 5.2.2 / 5.2.3 図書の 1 章・論文集の 1 論文 | `@incollection` | `inbook`, `bookinbook`, `suppbook`, `suppcollection`, `inreference` |
| 5.3.1 レポート 1 冊 / 5.3.2 1 論文 | `@report` | `techreport`, `manual`（`booktitle` の有無で全体/1論文を自動切替） |
| 5.4 学位論文 | `@thesis` | `phdthesis`, `mastersthesis`（`type = {phdthesis}` / `{mathesis}` 対応） |
| 5.5.1 会議報告全体 | `@proceedings` | `mvproceedings` |
| 5.5.2 / 5.5.3 会議論文・予稿 | `@inproceedings` | `conference`（`journaltitle` 指定時は雑誌掲載形式へ自動切替） |
| 5.6 プレプリント | `@unpublished` | `number` または `eprint` を出力 |
| 5.7 特許文献 | `@patent` | `holder`（出願人）+ `author`（発明者）対応 |
| 5.8 規格文書 | `@standard` | `number:year. title. publisher.` |
| 5.9 投稿中・早期公開論文 | `@article` / `@unpublished` | `pubstate` で状態表記を自動出力 |
| 5.10 ウェブサイト・ブログ | `@online` | `electronic`, `www`（著者不明時は `editor` にも対応） |
| 5.11 メーリングリスト・掲示板 | `@letter` | 発信者 `author`, 件名 `title`, ML 名 `organization`, `updatedate` |
| 5.12 データベース | `@dataset` | `booktitle`/`maintitle` の有無で全体/一部を自動切替 |
| 5.13 プログラム・ソースコード | `@software` | `booktitle` 指定時はリポジトリ内ファイル参照、`commit` 対応 |
| 法令・判例 | `@legislation` | `jurisdiction`, `legal` |
| 映像・その他 | `@video`, `@misc` | `movie`, `audio`, `artwork`, `image`, `music`, `performance`, `set` |

---

## 6. `.bib` 記述例（カスタムフィールド活用）

各資料種別の網羅的な記述例は [`references.bib`](../references.bib) と [`main.tex`](../main.tex) もあわせてご参照ください。

```bibtex
@book{lit_edition2025,
  author         = {山田, 太郎},
  title          = {固有版番号テスト用図書},
  edition        = {2025},
  editionliteral = {true},
  location       = {東京},
  publisher      = {検証出版},
  year           = {2025},
  pagetotal      = {250},
  isbn           = {978-4-00-000001-0}
}

@online{smine2005,
  author          = {{国立情報学研究所}},
  title           = {研究データ公開ポータル},
  organization    = {国立情報学研究所},
  medium          = {online},
  availablesource = {NII Research Portal},
  url             = {https://example.nii.ac.jp/},
  updatedate      = {2026-04-01},
  urldate         = {2026-05-20},
  accesslabel     = {参照}
}

@letter{mckiernan2005,
  author       = {McKiernan, Gerry},
  title        = {Google Scholar is a joke?},
  organization = {Web4Lib},
  date         = {2005-03-08},
  updatedate   = {2005-03-09},
  medium       = {online},
  url          = {http://lists.webjunction.org/wjlists/web4lib/},
  urldate      = {2005-08-29}
}

@software{bibloom_src2026,
  author          = {Tanaka, Ichiro},
  title           = {generator.ts},
  booktitle       = {Sample Reference Manager},
  version         = {1.0.0},
  commit          = {a1b2c3d},
  year            = {2026},
  medium          = {online},
  availablesource = {GitHub},
  url             = {https://github.com/example/sample-reference-manager/blob/main/src/generator.ts},
  urldate         = {2026-11-26}
}
```
