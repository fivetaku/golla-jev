[English](README.md) | [한국어](README.ko.md) | [中文](README.zh.md) | 日本語 | [Español](README.es.md)

# golla-jev（골라제브）

<p align="center">
  <img src="assets/golla-jev-hero.png" alt="golla-jev" width="320">
</p>

> 韓国の市場では商人が「골라 골라〜（選んで選んで〜）」と呼び込みます。オフィスでも人は一日中選んでいます。この領収書はどの費目か、このメールは返信が要るか、この条項は危ないか。golla-jev はその**選ぶ瞬間**を見つけ、Jev に任せるワークフローを設計します。

Jev（TypeSafe System One）は文章を書かず、決められた選択肢から選ぶモデルです。確率で答え、1回あたり約0.2秒です。golla-jev は業務の説明を受け取り、選ぶ仕事は Jev に、計算・日付・照合はコードに、文章はテンプレートに、写真や音声の読み取りはマルチモーダルモデルに振り分けた設計案を出します。

## クイックスタート

```
/plugin marketplace add https://github.com/fivetaku/gptaku_plugins.git
/plugin install golla-jev
```

Claude Code 2.1.275+: `/plugin install golla-jev --marketplace fivetaku/gptaku_plugins`

Claude Code を再起動して、次のように話しかけます。

- `/golla-jev チームの領収書精算を自動化したい`
- 「golla-jev、うちの会社で Jev を使えるところある？」— 範囲が広いと候補を3〜5個先に挙げます（アイデアモード）
- 「この業務の Jev ワークフローを組んで」— 業務を1つ最後まで設計します（設計モード）

## 何が出てくるか

- 一目でわかる流れ：入力 → 書き起こし → Jev の質問 N 個 → スクリプト → 成果物
- 段階ごとの担当表（Jev・コード・LLM・人）
- そのまま送れる Jev 質問の下書き JSON（state の例つき）
- 成果文書のプレビュー（決裁様式・日誌・台帳）。欄ごとに [J] Jev の値、[C] コード計算、[원] 原文、[사람] 人の担当を表示
- 似た公開事例と数値、間違えやすい点と防ぎ方
- テスト計画：サンプル数、正解の固定、dev/holdout による閾値の決め方

## 設計案の例

golla-jev が実際に出した設計案を3つ、そのまま載せています。業務説明を1段落入れると何が出てくるか、まず見てください（本文は韓国語です）。

- [領収書 → 支出決議書](examples/01-expense-report.md) — 設計モード
- [共用受信箱 → カンバンボード・週次報告](examples/02-mail-board.md) — 設計モード
- [インフルエンサー候補の選定](examples/03-influencer-screening.md) — アイデアモードから設計モードへ

## 同梱のベンチマークテンプレート

`skills/golla-jev/bench/` に韓国語のオフィス業務5種（領収書の費目・メールボード・議事録・満足度・契約条項）108問のベンチマークが入っています。問題は実際の顧客データではなく手書きの例です。作者が 2026-11-13 に jev-1.14.0 で実行したところ、韓国語の質問で 96.3%（208/216）、英語の質問で 93.5% でした。dev で選んだ閾値 0.70 を holdout に適用すると 97%（57/59）を自動処理し、自動処理分の誤答は0でした。1か月前の jev-1.13.0 では同じ手順で閾値が 0.85 でした。モデルが変わったら閾値も選び直す必要があります（`bench/REPORT.md`）。

```
cp -r <プラグインのパス>/skills/golla-jev/bench ./my-bench && cd my-bench
python3 build_items.py          # WF 辞書を自分の業務に書き換えたあと
python3 run.py                  # TYPESAFE_API_KEY 環境変数が必要（値をコマンドラインに直接書かない）
python3 analyze.py && python3 sweep.py
```

## 動作要件

- Claude Code
- 質問の下書きを実際に動かすには TypeSafe API キー（有料、https://typesafe.ai）が必要です。設計案だけならキーは不要です。
- 推奨：公式 `typesafe:typesafe-ai` プラグイン。golla-jev はモデル ID・価格・上限のように変わる事実を、記憶ではなくそのスキルと docs.typesafe.ai で確認します。
- ベンチマーク実行：Python 3.9+、任意で `certifi`

## しないこと

- 「Jev が合う／合わない」で終わりません。合わない部分は別の担当に回して流れを完成させます。
- 他人の閾値をそのまま勧めません。閾値は自分の業務サンプルで決めます。
- Jev に文章作成・計算・日付の数え上げを任せません。

## ライセンス

MIT。TypeSafe・Jev とは無関係の個人プロジェクトです。[DISCLAIMER](DISCLAIMER.md)
