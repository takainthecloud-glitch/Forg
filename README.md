# F_org — Organizational Friction Atelier

**English summary:** F_org is a single-file HTML self-assessment tool that quantifies the *organizational friction* that slows down digital and zero-trust transformation. Borrowing the metaphor of a coefficient of friction, it treats Silo (S) and Legacy (L) as the numerator and Crisis awareness (C) and Leadership will (L') as the denominator, computing **F_org = (S × L) / (C × L')** from 19 questions. Everything runs locally in the browser — no data is sent anywhere — and results can be printed to PDF or exported as JSON. The UI is in Japanese.

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

- **19 問の設問**（S:5 / L:5 / C:4 / L':5）を 4 章立てで回答。所要時間は約 10 分
- **F 値メーター** と業界平均との差分表示
- **組織アーキタイプ判定** — 分子・分母の組み合わせから 4 象限で組織の体質を分類（変革の臨界点型 / 孤立無援・理想家型 / 外圧依存・消去法型 / 安穏・思考停止型）
- **矛盾検出** — 回答間の不整合（例: 危機感は高いが予算が独立していない、IT 予算はあるのに動けない等）をパターンとして提示
- **推奨アクション** の優先度つき提示
- **IT 予算の売上高比率** を金額入力から自動算出し、公開ベンチマーク（ITR / Deloitte / Flexera）と比較
- **PDF 出力**（ブラウザの印刷機能を使用）と **OVERDUE 連携 (JSON)** — 診断結果を JSON で書き出し、投資対効果を試算する [OVERDUE](https://github.com/takainthecloud-glitch/OVERDUE) に渡せます
- **ライト / ダークテーマ**切替（paper / indigo）

### データの扱い

入力内容はブラウザ内で計算され、外部に送信されません。保存されるのはテーマ設定のみ（localStorage）で、診断の回答はページを閉じると消えます。必要な結果は PDF または JSON として手元に保存してください。

## 使い方

1. `forg_ztelier_v3_0_1.html`（または `index.html`）をダウンロードする
2. ブラウザでファイルを開く

ビルド不要・サーバー不要です。GitHub Pages を有効にした場合は `index.html` がそのまま表示されます。

> **ネットワークについて**: 画面描画に React / Babel を CDN（unpkg）から、フォントを Google Fonts から読み込みます。初回表示時はインターネット接続が必要です。完全オフラインで使う場合は、これらを同梱した形に改変してご利用ください。

### 動作環境

Chrome / Edge / Firefox / Safari の最新版。JavaScript を有効にしてください。

## バージョン

- アプリケーション: **v3.0.1**（HTML 内の `const APP_VER` が唯一の版数の出所）
- 設問セット（methodology）: v4.3
- 設計システム: Ztelier Edition

エクスポートする JSON のスキーマ版数はアプリ版数とは別軸で管理されています。

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
