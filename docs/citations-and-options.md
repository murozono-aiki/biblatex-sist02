# パッケージオプションと文中での引用コマンドガイド

[`sist02.bbx`](../sist02.bbx) および [`sist02.cbx`](../sist02.cbx) が提供するパッケージオプションと、本文中で使用できる引用コマンド（`biblatex` 標準コマンドおよび `sist02` 固有拡張）の詳細リファレンスです。

[← README に戻る](../README.md)

---

## 1. パッケージオプション

### 1.1 `superscript=true | false`（既定値: `true`）

本文中の `\cite`・`\parencite`・`\cites`・`\parencites`・`\autocite` が生成する参照番号を上付き文字にするか、本文と同じベースラインに配置するかを文書全体で切り替えます。

**上付き角括弧にする場合（既定値）**:

```latex
\usepackage[backend=biber,style=sist02]{biblatex}
% 明示する場合: \usepackage[backend=biber,style=sist02,superscript=true]{biblatex}
```

- 文中の `\cite{nishi2003}` は `^[1]` のように角括弧ごと上付きで出力されます（直前の不要な空白を除き、参照記号直前での改行を禁止します）。

**上付きにせず通常位置で出力する場合**:

```latex
\usepackage[backend=biber,style=sist02,superscript=false]{biblatex}
```

- 文中の `\cite{nishi2003}` は `[1]` のように本文と同じベースライン上に出力されます。

### 1.2 `urldatelabel=accessed | cited`（既定値: `accessed`）

`urldate` 出力時のラベルの既定値を切り替えます。

- `urldatelabel=accessed`（既定）: 和文文献では `参照`、欧文文献では `accessed` を出力
- `urldatelabel=cited`: 和文文献では `入手`、欧文文献では `cited` を出力

> [!NOTE]
> エントリ側でカスタムフィールド `accesslabel` が指定されている場合は、パッケージオプションよりもエントリ側の `accesslabel` が優先されます。

### 1.3 `maxbibnames=<N>` / `minbibnames=<N>`（既定値: `999`）

SIST 02 完全記述の原則に従い、既定では `maxnames=999, minnames=999` としてすべての著者を省略せずに列記します。著者数が多い文献を省略表示したい場合は以下のように上書きできます。

```latex
\usepackage[backend=biber,style=sist02,maxbibnames=2,minbibnames=2]{biblatex}
```

- 省略記号は和文文献で `ほか`、欧文文献で `et al.` が自動選択されます。

### 1.4 自動的に有効化される `biblatex` 標準オプション

`sist02.bbx` および `sist02.cbx` により、ロード時に以下のオプションが既定で設定されます。

| 設定項目 | 既定値 | 目的 |
|---|---|---|
| `giveninits` | `false` | 欧文著者名の名をイニシャル化せず完全表記 |
| `useprefix` | `true` | `van`, `de` 等の前置詞を姓の前に維持 |
| `maxnames` / `minnames` | `999` | 原則すべての著者名を列記 |
| `sorting` | `none` | 本文中での引用順に文献番号を割り当て |
| `sortcites` | `true` | `\cite{b,a}` のような複数引用を文献番号順に自動整列 |
| `date` / `urldate` / `eventdate` / `updatedate` | `iso` | 日付を ISO 8601 拡張形式（`YYYY-MM-DD`）で出力 |
| `seconds` | `true` | 時刻を含むタイムスタンプを秒まで表現可能 |
| `isbn` / `doi` / `eprint` | `true` | ISBN・DOI・eprint 識別子を出力 |
| `autocite` | `inline` | 基底スタイル `numeric-comp` に従い `\autocite` を `\parencite` へ委譲 |
| `\bibrangedash` | `\textendash` | 連続参照番号の圧縮区切り（例: `[7–10]`） |
| `\multicitedelim` | `\addcomma\space` | 非連続参照番号の区切り（例: `[1, 4]`） |

---

## 2. 文中での引用の使い方

### 2.1 上付き・通常位置の混在や個別切替

パッケージオプション `superscript` で文書全体の既定を決めた上で、特定の箇所だけ逆の形式を使いたい場合は `\supercite` と `\inlinecite` を使い分けます。

```latex
% superscript=true（既定）のとき
通常は上付きで参照します\cite{nishi2003}。
文中で括弧を通常の高さにしたい場合は \inlinecite{nishi2003,fan2006} を使います。

% superscript=false のとき
通常はベースライン上で参照します \cite{nishi2003}。
脚注番号風に上付きにしたい箇所では\supercite{shomei2003}を使います。
```

### 2.2 複数文献の圧縮・前置詞／後置詞（prenote / postnote）

`sist02.cbx` は `numeric-comp` を継承しているため、複数キーの指定時に自動で昇順ソートと連続番号の圧縮が行われます。

```latex
% 連続する番号 7, 8, 9, 10 を参照 -> [7–10]
図書\cite{shomei2003,miller2006,vanbreemen1998,lit_edition2025}

% 非連続の番号 1, 4 を参照 -> [1, 4]
関連研究 \inlinecite{nishi2003,fan2006}

% 参照箇所（ページ番号等）を明記する場合
第3章を参照\cite[p. 42]{shomei2003}
詳細を比較\cite[詳しくは][第2節]{nishi2003}
```

---

## 3. 引用コマンド一覧（`biblatex` 標準＋`sist02` 拡張）

`biblatex` 標準コマンドは、原則として先頭大文字版（`\Cite`, `\Textcite`, `\Autocite` 等）で文頭大文字化、末尾に `s` を付けた複数形（`\cites`, `\parencites`, `\textcites`, `\autocites` 等）でキーごとに個別の prenote / postnote を渡せます。

| コマンド | 複数形・先頭大文字版 | `biblatex` 標準での挙動 | `sist02` スタイルでの出力・特徴 |
|---|---|---|---|
| `\cite[pre][post]{key}` | `\cites`, `\Cite`, `\Cites` | 基本引用コマンド。多くの数値スタイルでは `[1]`、著者名-年式では `Author 2020` を出力 | `superscript=true` 時は上付き `[1]`（改行抑制・圧縮対応）、`superscript=false` 時は通常位置 `[1]` |
| `\parencite[pre][post]{key}` | `\parencites`, `\Parencite` | 括弧付き引用（著者名-年式では `(Author 2020)`、数値スタイルでは `[1]`） | `\cite` と同じ（`superscript` オプションに連動） |
| `\supercite[pre][post]{key}` | `\supercites` | 上付き数字引用（標準 `numeric-comp` では角括弧なしの `1`、prenote/postnote 非対応） | `sist02` で角括弧付き上付き `[1]` として再定義済み。`superscript` 設定に関わらず常に上付き `[1]`（prenote/postnote・圧縮対応） |
| `\inlinecite[pre][post]{key}` | `\inlinecites` | *(標準にはない `sist02` 固有拡張)* | `superscript` 設定に関わらず常に本文と同じベースライン上の `[1]` を出力 |
| `\textcite[pre][post]{key}` | `\textcites`, `\Textcite` | 文中組み込み引用（「著者名 [1]」／「著者名 (2020)」） | 例: `森; ケネディ; 島田 et al. [2]`（主語・目的語として文に組み込む用途） |
| `\autocite[pre][post]{key}` | `\autocites`, `\Autocite` | `autocite=` オプション（`plain`/`inline`/`footnote`/`superscript`）に応じて引用形式を自動切替 | 既定の `autocite=inline` では `\parencite` に委譲され、`superscript` 設定に連動 |
| `\footcite[pre][post]{key}` | `\footcites`, `\Footcite` | 脚注（`\footnote`）内に引用を出力 | 脚注内に `[1]` を出力 |
| `\footcitetext[pre][post]{key}` | `\footcitetexts` | `\footnotetext` で脚注本文のみを生成（図表・ミニページなど向け） | 脚注テキストとして `[1]` を出力 |
| `\smartcite[pre][post]{key}` | `\smartcites`, `\Smartcite` | 本文中では脚注化、すでに脚注内なら括弧引用を出力 | 本文では脚注 `[1]`、脚注内では `[1]` |
| `\citeauthor{key}` | `\Citeauthor` | 著者・編集者名（`labelname`）のみを出力 | 例: `西; 石原` / `Fan; Gabaldon` |
| `\citetitle{key}` | `\Citetitle` | タイトル（`labeltitle`）のみを出力 | 対象文献の題名を出力 |
| `\citeyear{key}` / `\citedate{key}` | — | 出版年（`year`）／日付全体（`date`）のみを出力 | `2003` / `2026-05-21` |
| `\citeurl{key}` | — | `url` フィールドを URL フォーマットで出力 | `\url{https://...}` 形式 |
| `\fullcite{key}` / `\footfullcite{key}` | — | 本文中または脚注内に完全な書誌記述をその場で展開 | SIST 02 形式の書誌記述一式を出力 |
| `\notecite[pre][post]{key}` | `\Notecite`, `\pnotecite`, `\fnotecite` | 引用ラベル自体は出さず prenote / postnote のみを出力（参考文献一覧には登録） | 標準と同じ |
| `\nocite{key}` / `\nocite{*}` | — | 本文中に何も出力せず、参考文献一覧（`\printbibliography`）へ登録するのみ（`*` で `.bib` 内全件） | 参考文献一覧のみに出力 |

---

## 関連ドキュメント

- [カスタムフィールド・言語／著者名判定・対応エントリタイプ一覧](./fields-and-entries.md)
- [BibLoom（SIST 02 対応 Web 文献管理アプリ）](https://bibloom.riyoddoo.com/)
