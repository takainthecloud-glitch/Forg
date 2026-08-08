# F_org — Organizational Friction Atelier

**English summary:** F_org is a single-file HTML self-assessment tool that quantifies the *organizational friction* that slows down digital and zero-trust transformation. Borrowing the metaphor of a coefficient of friction, it treats Silo (S) and Legacy (L) as the numerator and Crisis awareness (C) and Leadership will (L') as the denominator, computing **F_org = (S × L) / (C × L')** from 23 questions. Everything runs locally in the browser — no data is sent anywhere — and results can be printed to PDF or exported as JSON. The UI is in Japanese.

---

## 概要

F_org（フォーグ）は、**組織変革が進まない理由を「摩擦」として定量化する**単一 HTML の診断ツールです。

セキュリティやゼロトラストの取り組みが止まる原因は、多くの場合、製品でも予算でもなく組織側にあります。F_org はその摩擦を物理学の摩擦係数になぞらえ、4 つの変数の比で表します。

```
F_org = (S × L) / (C × L')
```

| 変数 | 名称 | 役割 | 測るもの |
|---|---|---|---|
| **S** | Silo / 縦割り | 分子（摩擦） | 予算の分離、意思決定の遅さ、業務部門の可視性、有事の横断体制 |
| **L** | Legacy / レガシー | 分子（摩擦） | IT 予算の硬直度、アプリ配置、NW 依存度、リプレース周期 |
| **C** | Crisis / 危機感 | 分母（推進力） | インシデント認知、セキュリティの位置づけ、IT 予算の売上高比率、予算の独立性 |
| **L'** | Leadership / 変革意志 | 分母（推進力） | 責任の所在、ゴール定義、変革の起点、経営計画との連動、失敗の許容度 |

比であるため、分母を大きくしても分子を小さくしても F は下がります。

### F 値の読み方

| バンド | 状態 |
|---|---|
| F < 1.0 | 変革の加速帯 |
| F = 1.0 – 2.0 | 変革の停滞帯 |
| F > 2.0 | 変革の膠着帯 |

### 主な機能

- **23 問の設問**（S:6 / L:6 / C:5 / L':6）を 4 章立てで回答。所要時間は約 12 分
  - v3.1.0 で AI統制4問（シャドーAI可視性・AI参照可能なデータ基盤・AIリスク認識・組織的推進）を追加
- **F 値メーター** と業界平均との差分表示
- **組織アーキタイプ判定** — 分子・分母の組み合わせから 4 象限で組織の体質を分類（変革の臨界点型 / 孤立無援・理想家型 / 外圧依存・消去法型 / 安穏・思考停止型）
- **矛盾検出** — 回答間の不整合（例: 危機感は高いが予算が独立していない、IT 予算はあるのに動けない等）をパターンとして提示
- **推奨アクション** の優先度つき提示
- **IT 予算の売上高比率** を金額入力から自動算出し、公開ベンチマーク（ITR / Deloitte / Flexera / 総務省 情報通信白書 2026）と比較
- **PDF 出力**（ブラウザの印刷機能を使用）と **OVERDUE 連携 (JSON)** — 診断結果を JSON で書き出し、投資対効果を試算する [OVERDUE](https://github.com/takainthecloud-glitch/OVERDUE) に渡せます
- **ライト / ダークテーマ**切替（paper / indigo）

### データの扱い

入力内容はブラウザ内で計算され、外部に送信されません。保存されるのはテーマ設定のみ（localStorage）で、診断の回答はページを閉じると消えます。必要な結果は PDF または JSON として手元に保存してください。

## 使い方

1. `forg_ztelier_v3_2_1.html`（または `index.html`）をダウンロードする
2. ブラウザでファイルを開く

ビルド不要・サーバー不要です。GitHub Pages を有効にした場合は `index.html` がそのまま表示されます。

> **ネットワークについて**: 画面描画に React / Babel を CDN（unpkg）から、フォントを Google Fonts から読み込みます。初回表示時はインターネット接続が必要です。完全オフラインで使う場合は、これらを同梱した形に改変してご利用ください。

### 動作環境

Chrome / Edge / Firefox / Safari の最新版。JavaScript を有効にしてください。

## バージョン

- アプリケーション: **v3.2.1**（HTML 内の `const APP_VER` が唯一の版数の出所）
- 設問セット（methodology）: v4.4
- 設計システム: Ztelier Edition — Ztelier UI Kit v1.0

エクスポートする JSON のスキーマ版数はアプリ版数とは別軸で管理されています。

> v4.4（23問）の F 値は v4.3（19問）と直接比較できません。

### 更新履歴

- **v3.2.1**（2026-08-08）
  - **診断ロジックの修正** — 全ての軸が業界平均以上に良好な場合でも、その中で相対的に最も低い軸を「最弱軸」として断定してしまう問題を修正しました。判定を4状態（明確な弱点あり / 相対的に低いだけ / 全軸良好 / 判定不能）に分け、軸の差が **±0.25 以内**（判定デッドバンド）のときは順位づけを避けて「有意差なし」として扱います。あわせて未測定の軸があるときは最弱軸の断定自体を行わないゲートを追加しました。**該当する回答パターンでは、v3.2.0 までと最弱軸の表示が変わります。**
  - `damage_estimate` に算定根拠のフィールドを追加。この数値が**売上高ベースの概算**であることを出力側からも判別できるようにしました（追加のみ。既存フィールドは不変）。
  - forg-v4 契約は additive を維持しており、**v3.2.0 以前の出力もそのまま読み込めます**。
- **v3.2.0**（2026-08-08 · 公開前に v3.2.1 へ統合したため単独のファイルはありません）
  - UI を **Ztelier UI Kit v1.0** に一本化。旧 Common Kit（v1.1.0 / v2.0 パッチ）由来の重複トークンと未使用のコンポーネント定義を整理し、間隔・角丸・モーションを単一のスケールに揃えました。
  - **フォントサイズの下限を画面・印刷とも 11px に統一**。ブラウザ既定の `sub` / `smaller` 指定で 11px を割っていた箇所（`F_org` の下付き表記など）に明示サイズを与えて是正しました。
  - **コントラストを WCAG AA まで引き上げ** — 補助テキスト色 `--ink-3` を AA 確定値に変更。あわせて危険・警告色に本文用の濃色（`--danger-2` / `--warn-2`）を用意し、小さい文字の上でも AA を満たすようにしました。
  - **フォーカスリングを全操作要素で明示**（`:focus-visible` に 2px アウトライン）。キーボード操作時の現在位置が常に見える状態になります。
  - 印刷 CSS を修正し、PDF 出力時の大見出しの過大サイズと不要な改ページを解消しました。
  - 装飾（影・グラデーション背景・色付き縦バー）は罫線と面に置き換えています。**設問・スコア計算・アーキタイプ判定・エクスポート JSON の形式に変更はありません**（v3.1.1 の出力とそのまま比較できます）。
- **v3.1.1**（2026-08-04）
  - 矛盾ペナルティを累積適用化 — S×L' と C×L' の両矛盾が同時成立するケースで、ペナルティが `×0.85` のみ（後勝ち）から `×0.85×0.9`（累積）に変更されました。該当ケースの F 値は **v3.1.0 比で約 1.18 倍に上振れ**し、変革バンド（加速／停滞／膠着）の判定が変わることがあります。
    - **v3.1.0 以前に出力した forg-v4 JSON とは、両矛盾成立ケースに限り直接比較できません。**
  - 「vs industry avg」の業界平均値に「19問基準・参考値」の注記を追加（`INDUSTRY_AVG` の数値・export JSON のスキーマは変更なし）
  - JSON エクスポートの `completed` を「23問すべて回答済み」ベースの判定に修正
  - Hero セクションの所要時間表記を 10分 → 12分 に修正（実測値に整合）
  - エクスポート JSON の `tool_version` をアプリ版数（`APP_VER`）から自動導出するよう修正（固定文字列のドリフトを解消）

## ライセンス

Apache License 2.0 — 詳細は [LICENSE](./LICENSE) を参照してください。

```
Copyright 2026 takainthecloud-glitch

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0
```

## 免責

本ツールは組織状態の把握を支援する情報提供を目的としています。算出される F 値・アーキタイプ・推奨アクションは公開ベンチマークと回答に基づく目安であり、特定の投資判断や結果を保証するものではありません。
