# biblatex-sist02

**SIST 02-2007（参照文献の書き方）** の完全記述に準拠した bibLaTeX スタイルパッケージです。和文・欧文が混在する文献リストや、電子資料・ソフトウェア等の記述に対応しています。

- **関連 Web アプリ**: [BibLoom](https://bibloom.riyoddoo.com/)（本スタイル対応の多言語文献管理アプリ。`.bib` ファイルの編集・エクスポートに対応）

---

## 使い方

### 1. ファイルの配置

[Releases](https://github.com/murozono-aiki/biblatex-sist02/releases) から **`sist02.bbx`**, **`sist02.cbx`**, **`sist02.dbx`** の 3 ファイルを取得し、LaTeX 文書と同じフォルダ（または `TEXMFHOME/tex/latex/biblatex-sist02/` 配下）に配置します。

> **要件**: バックエンドは **Biber**（`backend=biber`）が必須です（`bibtex` 非対応）。TeX エンジンは LuaLaTeX / XeLaTeX / upLaTeX / pLaTeX / pdfLaTeX で動作します（日本語文書には LuaLaTeX を推奨）。

### 2. LaTeX 文書の記述例

```latex
\documentclass[paper=a4]{jlreq}
\usepackage{url}
\usepackage{hyperref}
\usepackage[backend=biber,style=sist02]{biblatex}
\addbibresource{references.bib}

\begin{document}
本文中で引用する例\cite{nishi2003}、複数文献の例\cite{shomei2003,miller2006,vanbreemen1998}。
上付きにせず引用したい場合は \inlinecite{fan2006} を使います。

\printbibliography[title=参考文献]
\end{document}
```

### 3. コンパイル

```bash
lualatex main.tex
biber main
lualatex main.tex
lualatex main.tex
```

---

## 主なオプションと引用コマンド

| オプション | 既定値 | 用途 |
|---|---|---|
| `superscript=true \| false` | `true` | `\cite` 等を上付き `[1]` にするか、通常位置 `[1]` にするか切替 |
| `urldatelabel=accessed \| cited` | `accessed` | 入手日付ラベルの既定値（`参照` / `accessed` または `入手` / `cited`） |
| `maxbibnames=<N>` / `minbibnames=<N>` | `999` | 著者を `ほか` / `et al.` で省略する人数のしきい値 |

- `\cite{key}` / `\parencite{key}` / `\autocite{key}`: `superscript` 設定に従って `[1]` を出力（連続番号は `[1–3]` に圧縮）
- `\supercite{key}` / `\inlinecite{key}`: 設定に関わらず常に上付き `[1]` / 通常位置 `[1]` を出力
- `\textcite{key}`: `著者名 [1]` の形で文中に組み込み

---

## 詳細ドキュメント・サンプル

- [パッケージオプション・文中引用コマンド一覧](./docs/citations-and-options.md)
- [カスタムフィールド・著者名／言語判定・対応エントリタイプ](./docs/fields-and-entries.md)
- サンプル文書: [`main.tex`](./main.tex) / [`references.bib`](./references.bib)

---

## ライセンス

[MIT License](./LICENSE)
