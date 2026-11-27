# biblatex-sist02

**`biblatex-sist02`** は、科学技術振興機構（JST）が定める科学技術情報流通技術基準 **SIST 02-2007（参照文献の書き方）** の**完全記述（Full Description）** に厳密に準拠した bibLaTeX スタイルパッケージです。

和文・欧文が混在する文献リストの句読点・著者名表記（和文の姓・名連結、外国人名の `姓, 名`、混在時の `; ` 区切り、Biber アノテーションによる明示指定）、全 13 分類の資料種別、およびソフトウェア・ソースコード・データベース等の電子的情報源に対応しています。

---

## BibLoom との連携（Web 文献管理アプリ）

本スタイルと 1 対 1 で連携する多言語文献管理 PWA **[BibLoom](https://bibloom.riyoddoo.com/)** を利用すると、`.bib` ファイルの手編集なしで SIST 02 完全記述の書誌管理が行えます。

- **Web アプリ**: https://bibloom.riyoddoo.com/
- **GitHub リポジトリ**: https://github.com/murozono-aiki/bibloom

### BibLoom で利用できる連携機能

- **`sist02.dbx` カスタムフィールドのネイティブ編集**: `medium`, `availablesource`, `accesslabel`, `origlangdisplay`, `updatedate`, `commit` や標準拡張フィールド（`issuetitle`, `pubstate`, `holder`）を専用 UI から入力・編集可能
- **個人ごとの著者表記切替 (`author+an`)**: 制作者ごとに「自動判定 / 和文 / 欧文・カタカナ」を選択でき、`.bib` エクスポート時に `author+an` 等を自動出力（双方向インポート対応）
- **数字のみの固有版名 (`editionliteral`)**: 版表示に数字のみが入力された場合に入力欄直下へチェックボックスを表示し、必要な場合のみ `editionliteral = {true}` を出力
- **多言語メタデータ管理 (`langid` / `language` / `origlangdisplay`)**: 日英ロケールごとの書誌保持と自動言語注記に対応
- **SIST 02 参照文献テキスト生成**: TeX コンパイルを行わずに `[1] ...` 形式の参照文献リストをコピー・テキスト出力可能

---

## 動作環境とファイル構成

### 必須環境

Unicode 文字種判定（`expl3` 正規表現）および Biber データアノテーション（`author+an` 等）を使用するため、**LuaLaTeX**（または **XeLaTeX**）と **Biber** バックエンドが必須です（`bibtex` バックエンドは非対応です）。

### パッケージ構成

| ファイル | 役割 |
|---|---|
| [`sist02.dbx`](./sist02.dbx) | SIST 02 専用カスタムフィールド 8 種を bibLaTeX / Biber のデータモデルへ登録 |
| [`sist02.bbx`](./sist02.bbx) | 文献一覧スタイル（文字種自動判定、著者区切り、URL `\url{#1}`、全 36 種エントリドライバ） |
| [`sist02.cbx`](./sist02.cbx) | 本文中の引用スタイル（上付き `[1]` / 通常位置 `[1]` 切替、`numeric-comp` + `sortcites=true` ベース） |
| [`main.tex`](./main.tex) | 全資料種別・各種引用コマンドの動作確認用サンプル文書 |
| [`references.bib`](./references.bib) | 全資料種別を網羅したサンプル書誌データベース |
| [`docs/citations-and-options.md`](./docs/citations-and-options.md) | パッケージオプション・文中での引用コマンド詳細ガイド |
| [`docs/fields-and-entries.md`](./docs/fields-and-entries.md) | カスタムフィールド・言語／著者名判定・対応エントリタイプ一覧 |

---

## 導入方法とクイックスタート

### 1. スタイルファイルの配置

[`sist02.bbx`](./sist02.bbx)、[`sist02.cbx`](./sist02.cbx)、[`sist02.dbx`](./sist02.dbx) の 3 ファイルを、文書と同じディレクトリ（または `TEXMFHOME/tex/latex/biblatex-sist02/` 配下）に配置します。

### 2. LaTeX 文書の記述例（LuaLaTeX + `jlreq`）

```latex
\documentclass[paper=a4]{jlreq}
\usepackage{url}
\usepackage{hyperref}
\usepackage[backend=biber,style=sist02]{biblatex}
\addbibresource{references.bib}

\begin{document}
本文中で上付き引用を行う例\cite{nishi2003}や、
複数文献をまとめて引用する例\cite{shomei2003,miller2006,vanbreemen1998}。

通常位置で引用したい場合は \inlinecite{fan2006} や、
パッケージオプションで \verb|superscript=false| を指定します。

\printbibliography[title=参考文献]
\end{document}
```

### 3. コンパイル手順

```bash
lualatex main.tex
biber main
lualatex main.tex
lualatex main.tex
```

---

## パッケージオプションと文中引用の概要

詳しくは [パッケージオプション・引用コマンド詳細ガイド](./docs/citations-and-options.md) を参照してください。

### 主なパッケージオプション

| オプション | 既定値 | 概要 |
|---|---|---|
| `superscript=true \| false` | `true` | `\cite` / `\parencite` / `\autocite` 等を上付き角括弧 `[1]`（`true`）にするか、本文と同じベースライン上の `[1]`（`false`）にするかを切り替え |
| `urldatelabel=accessed \| cited` | `accessed` | 入手日付ラベルの既定値切替（`accessed`: 参照 / accessed、`cited`: 入手 / cited）。エントリ個別の `accesslabel` が優先 |
| `maxbibnames=<N>` / `minbibnames=<N>` | `999` | 全著者列記から `ほか` / `et al.` 省略へ切り替える際のしきい値（例: `maxbibnames=1,minbibnames=1`） |

### 主な文中引用コマンド

- `\cite{key}` / `\parencite{key}` / `\autocite{key}`: `superscript` オプションに従い上付き `[1]` または通常位置 `[1]` を出力（連続番号は `[1–3]` へ自動圧縮）
- `\supercite{key}`: オプションに関わらず常に上付き角括弧 `[1]` を出力（`prenote` / `postnote` 対応）
- `\inlinecite{key}` *(sist02 拡張)*: オプションに関わらず常にベースライン上の `[1]` を出力
- `\textcite{key}`: `著者名 [1]` の形で文中に組み込み出力
- その他 `\footcite`, `\smartcite`, `\citeauthor`, `\citetitle`, `\citeyear`, `\citeurl`, `\fullcite`, `\nocite` など bibLaTeX 標準コマンドに対応

---

## カスタムフィールド・文字種判定・エントリタイプ概要

詳しくは [カスタムフィールド・言語判定・対応エントリタイプ詳細](./docs/fields-and-entries.md) を参照してください。

- **独自カスタムフィールド (`sist02.dbx`)**: `medium`（媒体表示）、`availablesource`（入手先）、`accesslabel`（入手日付ラベル）、`origlangdisplay`（言語表示上書き）、`authorstyle`（エントリ一括著者表記）、`editionliteral`（数字のみ版名の補完抑止）、`updatedate`（更新日付 ISO）、`commit`（ソースコードのコミットハッシュ）
- **自動判定と個別上書き**: `title` 等からエントリの和文／欧文を判定し、さらに制作者一人ひとりの文字列（漢字・平仮名 vs ラテン文字・カタカナのみ）から表記と区切り文字（`, ` / `; `）を自動選択。カタカナのみの日本人名などは `author+an = {1=japanese}` で個別に指定可能
- **対応エントリタイプ**: `@article`, `@book`, `@incollection`, `@report`, `@thesis`, `@proceedings`, `@inproceedings`, `@unpublished`, `@patent`, `@standard`, `@online`, `@letter`, `@dataset`, `@software`, `@legislation`, `@video`, `@misc` など全 36 種（エイリアス含む）に対応

---

## ライセンス

[MIT License](./LICENSE)
