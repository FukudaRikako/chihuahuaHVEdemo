# 4匹のチワワ健康管理デモ

これは、Hypervelocity Engineering を非エンジニアにも説明しやすくするためのデモです。

テーマは「4匹のチワワの健康管理アプリを作りたい」です。専門用語ではなく、次の流れで見せます。

```text
家族の困りごと
  ↓
AI に渡すメモ
  ↓
AI が整理したアプリの要件
  ↓
画面・機能・データの設計案
```

## 30 秒で説明すると

4匹のチワワの体重、食事、薬、通院、気になる症状がバラバラに記録されていて、家族で共有しづらい。

そこで、AI に「いま困っていること」と「作りたいアプリのイメージ」を渡すと、健康管理アプリに必要な機能や画面を順番に整理してくれる、というデモです。

## 登場する4匹

| 名前 | どんな子？ | 気にしたいこと |
|---|---|---|
| まめ | 8歳。甘えん坊で食いしん坊 | 体重、歯、足の違和感 |
| こむぎ | 5歳。怖がり | 食欲ムラ、おなかの調子 |
| ちょこ | 11歳。シニアで穏やか | 心臓、運動量、通院メモ |
| ルナ | 2歳。元気いっぱい | 低血糖、ワクチン予定 |

## デモで見せるもの

| 見せる順番 | 開くもの | ひとことで言うと |
|---:|---|---|
| 1 | `original-docs\chihuahua-health-context.md` | 飼い主さんのメモ |
| 2 | `qa\chihuahua-health-seed-answers.md` | AI に先に教えておくルール |
| 3 | HVE の実行コマンド | AI に整理をお願いするボタンの代わり |
| 4 | `docs\` | AI が作った「アプリに必要なこと」 |
| 5 | `knowledge\` | 次の作業でも使い回せる整理済みメモ |

## セットアップ

リポジトリルートで実行します。

```powershell
.\demos\chihuahua-health\setup-demo.ps1
```

これで、デモ用の入力ファイルが次の場所にコピーされます。

- `original-docs\chihuahua-health-context.md`
- `qa\chihuahua-health-seed-answers.md`
- `docs\business-requirement.md`

既存ファイルを上書きしたい場合だけ `-Force` を付けます。

```powershell
.\demos\chihuahua-health\setup-demo.ps1 -Force
```

## まずは安全に「予告編」だけ見せる

最初は `--dry-run` を付けます。これは実際に AI に作業させず、「この順番で進めます」という流れだけ確認するモードです。

```powershell
python -m hve orchestrate `
  --workflow ard `
  --company-name "チワワファミリー" `
  --target-business "4匹のチワワ健康管理" `
  --survey-base-date 2026-06-23 `
  --survey-period-years 1 `
  --target-region "日本" `
  --analysis-purpose "家族で共有できるペット健康管理アプリの要件整理" `
  --dry-run
```

この画面で言うこと:

> いきなりアプリを作るのではなく、まず AI が「何を整理するか」「どの順番で考えるか」を確認しています。

## 本番デモで見せる

Copilot SDK と GitHub 認証が使える環境では、`--dry-run` を外して実行します。

```powershell
python -m hve orchestrate `
  --workflow ard `
  --company-name "チワワファミリー" `
  --target-business "4匹のチワワ健康管理" `
  --survey-base-date 2026-06-23 `
  --survey-period-years 1 `
  --target-region "日本" `
  --analysis-purpose "家族で共有できるペット健康管理アプリの要件整理"
```

実行後は、次のようなものを見せると伝わりやすいです。

| ファイル | 非エンジニア向けの説明 |
|---|---|
| `docs\business-requirement.md` | どんなアプリが必要かを整理した紙 |
| `docs\catalog\use-case-catalog.md` | 「体重を記録する」「薬を忘れない」など、やりたいこと一覧 |
| `docs\company-business-requirement.md` | 4匹と家族の状況を整理した背景メモ |
| `knowledge\D*.md` | 次の設計や実装でも使う、AI 用の整理済みノート |

## 10 分デモ台本

1. 「今日は難しい業務システムではなく、4匹のチワワの健康管理アプリを例にします」
2. 「まず、飼い主さんのメモを見ます。体重、薬、通院、気になる症状がバラバラです」
3. 「次に、AI に先に教えておくルールを見ます。診断はしない、家族だけで見る、通院前にまとめたい、などです」
4. 「ここで HVE を実行します。AI がいきなりコードを書くのではなく、まず要件を整理します」
5. 「出てきた docs を見ると、アプリに必要な機能が整理されています」
6. 「つまり HVE は、ふわっとした相談を、開発に使える材料へ変換する流れを作るものです」

## 専門用語を使うなら最後に

| 用語 | やさしい言い方 |
|---|---|
| ARD | 何を作るべきか整理する工程 |
| AKM | メモをあとで再利用できる知識にする工程 |
| AAS | アプリの構成を考える工程 |
| dry-run | 本番前の予告編 |
| workflow | AI にお願いする作業の順番 |

