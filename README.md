# 4匹のチワワ健康管理 HVE デモ

このリポジトリは、Hypervelocity Engineering（HVE）と GitHub Copilot を使って、雑なメモからアプリ開発に必要な要件・機能一覧を整理するサンプルです。

## このデモで見せたいこと

```text
飼い主さんの雑なメモ
  ↓
AI が困りごとを整理
  ↓
必要な機能を洗い出し
  ↓
アプリ開発のたたき台を作成
```

題材は「4匹のチワワの健康管理アプリ」です。完成アプリではなく、アプリを作る前のモヤモヤを、開発チームに渡せる材料へ変換するデモです。

## 入っているもの

| フォルダ / ファイル | 内容 |
|---|---|
| `index.html` | 4匹のプロフィール一覧PoC |
| `original-docs/chihuahua-health-context.md` | 4匹のチワワの健康管理に関する雑なメモ |
| `qa/chihuahua-health-seed-answers.md` | AI に先に伝えておくルールや補足 |
| `docs/business-requirement.md` | AI が整理したアプリ要件 |
| `docs/recommended-kpi-okr.md` | 成功指標のたたき台 |
| `docs/usecase/` | 体重記録、投薬、通院メモなどのユースケース詳細 |
| `demo-guide.md` | 非エンジニア向けの見せ方・台本 |

## 30秒で説明するなら

4匹のチワワの体重、食事、薬、通院、気になる症状がバラバラに記録されていて、家族で共有しづらい。

その雑なメモを HVE + GitHub Copilot に渡すと、健康管理アプリに必要な機能や画面、ユースケースのたたき台に整理してくれる、というサンプルです。

## 見る順番

1. `index.html`
2. `original-docs/chihuahua-health-context.md`
3. `qa/chihuahua-health-seed-answers.md`
4. `docs/business-requirement.md`
5. `docs/usecase/`
6. `demo-guide.md`

## PoCを開く

`index.html` をブラウザーで開くと、4匹のプロフィール一覧PoCを見られます。

このPoCで確認できること:

- 4匹のプロフィールを一覧で見られる
- 名前・性格・健康上の注意で検索できる
- シニア、食事、通院、予定管理で絞り込める
- 1匹を選ぶと、今日見ることと家族メモを確認できる

## 注意

このリポジトリはデモ成果物のサンプルです。HVE 本体や Copilot 実行環境は含めていません。
