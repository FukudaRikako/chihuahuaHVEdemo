# 推奨 KPI / OKR 定義書

- 対象企業（家族単位）: チワワファミリー
- 分析対象事業・業務: 4匹のチワワ（まめ・こむぎ・ちょこ・ルナ）の健康管理
- 調査期間: 直近 1 年
- 対象地域: 日本国内
- 分析目的: 家族で共有できるペット健康管理アプリの要件整理に資する KPI / OKR / 計測データ・データ収集設計
- 一次情報: `docs/business-requirement.md`（Step 2 出力）
- 参考: `original-docs/chihuahua-health-context.md`, `qa/chihuahua-health-seed-answers.md`

> **凡例（信頼度区分）**:
> 1. **資料上確認できる事実**: `docs/business-requirement.md` に直接記載
> 2. **外部情報補足**: 一次情報に無いが業界一般・公開ベンチマーク等（出典明記必須）
> 3. **合理的仮説**: 一次情報の文脈から論理的に導出（前提・根拠を明示）
> 4. **追加確認必要論点**: 一次情報になく推定も不可。後続工程で追加調査が必要

> **本ファミリーの特殊性**: 本対象は営利事業体ではなく家族単位の業務であるため、KPI/OKR は「家計コストの最適化」「犬の健康寿命の最大化」「家族の心理的負担減」の3点に最適化している（`docs/business-requirement.md` §3 コスト・支出構造）。

### 実施前チェックリスト

- [x] 一次情報（`docs/business-requirement.md`）からの戦略的記述抽出
- [x] 戦略目標の構造化（ST-* ID 付与）
- [x] KPI 候補の SMART 検証
- [x] OKR Objective / Key Results 設計（3-5 Objective × 3-5 KR）
- [x] 計測データの定量・定性分類
- [x] データ収集の実装インターフェース仕様化
- [x] 後続工程（UC / APP / テスト）への引き継ぎ整理

---

## 1. Executive Summary

- **実施目的**: ARD Step 2 で確定した To-Be ビジョンと戦略推奨を、後続のユースケース設計（Step 4.1/4.2）およびアプリケーション選定（aas）が直接参照できる **測定可能な KPI / OKR** に落とし込む。
- **使用する主なインプット**: `docs/business-requirement.md` §1, §6, §7, §9（特に §9 主要 KPI 列挙）、§5 Key Issues（I1〜I8）。
- **戦略目標と KPI/OKR の全体像**:
  - 5 つの戦略目標（ST-01〜ST-05）に対し、9 個の KPI（KPI-01〜KPI-09）、4 個の OKR（OKR-01〜OKR-04）、14 個の計測データ（DAT-01〜DAT-14）を定義。
  - MVP（短期 0-6ヶ月）で KPI-01〜KPI-04 を計測可能化し、中期で KPI-05〜KPI-07、長期で KPI-08〜KPI-09 を整備する。
- **主要数値目標サマリ**（一次情報根拠）:
  - 通院前サマリ生成所要時間: **30分 → 5分（約 83% 削減）**（資料上確認できる事実: `docs/business-requirement.md` §1, §6, §9）
  - 投薬・予防薬の記録漏れ件数: **ゼロ運用化**（資料上確認できる事実: 同上）
  - 家族間「やった／やってない」確認往復回数: **週次でほぼゼロ**（資料上確認できる事実: 同上）
  - 体調変化検知リードタイム: **要ベースライン計測**（追加確認必要論点: §9）

---

## 2. 戦略目標マッピング

`docs/business-requirement.md` §6 To-Be Vision および §9 Strategic Recommendations から戦略目標を抽出し、ST-* ID として構造化する。

| 戦略目標 ID | 出典（一次情報内の該当記述・要約） | 概要 | 関連 KPI/OKR | 信頼度区分 |
|---|---|---|---|---|
| ST-01 | §1「投薬漏れ件数をゼロ運用化」／§9「投薬遵守率・予防薬遵守率」 | **投薬・予防薬の完全遵守**：日次投薬・月次フィラリア／ノミダニ・年次ワクチンの実施漏れをゼロに近づける | KPI-01, KPI-02, OKR-01 | 資料上確認できる事実 |
| ST-02 | §1「通院準備時間を 30分→5分」／§6「通院前は『サマリ生成』ボタン1つ」／§9 推奨理由(1) | **通院体験の質向上**：通院前情報整理の自動化と獣医師への正確な経過情報提供 | KPI-03, KPI-04, OKR-02 | 資料上確認できる事実 |
| ST-03 | §1「シニア犬ちょこの心雑音モニタリング」「ルナの低血糖予防」／§6「犬の健康寿命延伸（早期発見）」／§9 KPI「重大症状から獣医師連絡までのリードタイム」 | **体調変化の早期検知**：体重・食欲・症状の時系列把握とシニア／低血糖リスク観察 | KPI-05, KPI-06, KPI-07, OKR-03 | 資料上確認できる事実 |
| ST-04 | §1「家族間の『やった／やってない』確認の往復を週次でほぼ消滅」／§6「家族全員が同じビューを共有」 | **家族間タスク共有の確実化**：実施者・実施時刻のログ化と二重投薬／二重給餌防止 | KPI-08, KPI-09, OKR-04 | 資料上確認できる事実 |
| ST-05 | §6「家族メンバー間ロールベースアクセス制御」／§9 リスク対応「招待制家族グループ＋多要素認証」／§2 制約「家族外への共有はしない」 | **プライバシー保護**：家族のみ閲覧の徹底とアクセス制御運用 | （非機能 KPI として KPI-09 に統合） | 資料上確認できる事実 |

> 注: ST-05 は単独の数値 KPI を立てず、ST-04 配下の KPI-09（不正アクセス発生件数）に統合する。これは「ゼロ件維持が前提」の非機能要件であるため、独立した OKR を立てると過剰計測になる（合理的仮説: KPI 過多防止のための統合判断）。

---

## 3. KPI 定義（SMART）

各 KPI は SMART 原則（Specific / Measurable / Achievable / Relevant / Time-bound）を満たす。Achievable 根拠は一次情報の有無により信頼度区分を分けて記載する。

| KPI ID | 指標名 | 算出式 | 目標値 | Achievable 根拠 | 期限 | 担当 | データソース | 収集頻度 | 紐付 ST | 信頼度区分 |
|---|---|---|---|---|---|---|---|---|---|---|
| KPI-01 | 日次投薬遵守率 | （投薬完了記録件数 ÷ 投薬予定件数）× 100 | **≥ 98%**（週次平均） | 一次情報で「ゼロ運用化」明記（§1, §9）。98% は実運用での飲み忘れ許容枠を考慮した合理的仮説 | MVP リリース後 3ヶ月 | 家族代表 | アプリ内投薬ログ（DAT-01） | 日次集計／週次レビュー | ST-01 | 合理的仮説（目標値）／資料上確認できる事実（方向性） |
| KPI-02 | 予防薬・ワクチン遵守率 | （実施件数 ÷ 予定件数）× 100（月次フィラリア／ノミダニ／年次ワクチン） | **100%**（期限内実施） | §1・§9「ゼロ運用化」直接記載 | MVP リリース後 6ヶ月（1サイクル達成） | 家族代表 | 予防薬ログ（DAT-02）／ワクチン記録（DAT-03） | 月次／年次 | ST-01 | 資料上確認できる事実 |
| KPI-03 | 通院前サマリ生成所要時間 | サマリ生成ボタン押下〜PDF/画面表示完了までの所要秒数（中央値） | **≤ 5 分**（中央値） | §1, §6, §9 で「30分→5分」明示 | 中期リリース（6-18ヶ月） | 開発者 | アプリ操作ログ（DAT-04） | 通院都度 | ST-02 | 資料上確認できる事実 |
| KPI-04 | 通院前準備総所要時間（人手作業含む） | 通院日の前日18時〜通院出発時刻までに記録・整理に費やした実時間（家族自己申告） | **≤ 5 分**（中央値） | §1「30分→5分」明示。自己申告であり計測誤差は内包 | 中期リリース後 3ヶ月 | 家族代表 | アプリ内アンケート（DAT-05） | 通院都度 | ST-02 | 資料上確認できる事実（目標値）／合理的仮説（計測手段精度） |
| KPI-05 | 週次体重測定実施率 | （週次体重測定実施頭数 ÷ 4頭）× 100 | **≥ 90%**（月次平均） | §9 主要 KPI に「週次体重測定実施率」明記。90% は実運用での欠測許容 | MVP リリース後 3ヶ月 | 家族全員 | 体重ログ（DAT-06） | 週次／月次集計 | ST-03 | 合理的仮説（目標値）／資料上確認できる事実（指標自体） |
| KPI-06 | 体調変化検知リードタイム | 異常イベント（食欲低下／体重 5% 以上減／嘔吐等）の最初の記録時刻 〜 獣医師連絡時刻 | **≤ 24 時間**（中央値） | §9「重大症状から獣医師連絡までのリードタイム」を KPI として明示。具体目標値は §9 で「追加確認必要」と明記 | 長期リリース後 6ヶ月 | 家族代表 | 症状ログ（DAT-07）／連絡ログ（DAT-08） | 都度 | ST-03 | 追加確認必要論点（目標値）／資料上確認できる事実（指標自体） |
| KPI-07 | シニア犬・ハイリスク犬の観察テンプレ実施率 | （ちょこ＋ルナの観察テンプレ実施件数 ÷ 予定件数）× 100 | **≥ 80%**（月次） | §9 長期ロードマップ「シニア犬向け観察テンプレ」記載。目標値は合理的仮説 | 長期（18ヶ月以降） | 家族全員 | 観察テンプレログ（DAT-09） | 日次／月次集計 | ST-03 | 合理的仮説（目標値・運用負荷との両立） |
| KPI-08 | 家族間タスク重複・抜け件数 | 同一タスクの重複完了記録件数 ＋ 期限超過未完了件数（月次） | **0 件**（月次） | §1「往復を週次でほぼ消滅」／§5 I6「二重投薬・餌の二重給餌」根本解消方針 | MVP リリース後 3ヶ月 | 家族全員 | タスクログ（DAT-10）／重複検出（DAT-11） | 月次 | ST-04 | 資料上確認できる事実（方向性）／合理的仮説（厳密ゼロ化期間） |
| KPI-09 | 不正アクセス・家族外漏えい発生件数 | 招待外メンバーのアクセス試行成功件数 ＋ 家族外共有事象件数 | **0 件**（全期間） | §2 制約・§9 リスク対応で「家族のみ」明示。ゼロ件は非機能要件 | 全期間 | 開発者 | 認証ログ（DAT-12） | 月次レビュー | ST-04, ST-05 | 資料上確認できる事実 |

---

## 4. OKR 定義

`docs/business-requirement.md` §9 推奨 KPI 群を Objective に集約し、各 Objective に 3-4 個の Key Result を設計する。OKR 数は 4 個（推奨 3-5 個の範囲内）。
出典: Google OKR Playbook 推奨（3-5 Objective × 3-5 Key Result per Objective）— <https://www.whatmatters.com/faqs/okr-meaning-definition-example>

### OKR-01: 投薬・予防薬の運用漏れをゼロに近づける

- **Objective**: 4匹それぞれの投薬・予防薬・ワクチンを「家族の誰でも・抜けなく・二重なく」実施できる状態を確立する
- **Key Results**:
  - **KR-01-1**: 日次投薬遵守率（KPI-01）を週次平均 **98% 以上** にする（期限: MVP+3ヶ月／信頼度: 合理的仮説）
  - **KR-01-2**: 月次予防薬・年次ワクチンの期限超過件数（KPI-02 補完）を **0 件** にする（期限: MVP+6ヶ月／信頼度: 資料上確認できる事実）
  - **KR-01-3**: 二重投薬発生件数（KPI-08 部分集合）を **0 件／月** にする（期限: MVP+3ヶ月／信頼度: 資料上確認できる事実）
- **紐付 ST**: ST-01, ST-04
- **信頼度区分**: 資料上確認できる事実（方向性）／合理的仮説（具体数値）

### OKR-02: 通院体験を「家族にも獣医師にも価値あるもの」に変える

- **Objective**: 通院前準備の負荷を最小化し、獣医師に渡す情報品質を最大化する
- **Key Results**:
  - **KR-02-1**: 通院前サマリ生成所要時間（KPI-03）の中央値を **5 分以内** にする（期限: 中期リリース／信頼度: 資料上確認できる事実）
  - **KR-02-2**: 通院前準備総所要時間（KPI-04）の中央値を **5 分以内** にする（期限: 中期+3ヶ月／信頼度: 資料上確認できる事実）
  - **KR-02-3**: 通院後の家族満足度（5段階）を **4.0 以上** にする（期限: 中期+6ヶ月／信頼度: 追加確認必要論点 — ベースライン未取得）
  - **KR-02-4**: 獣医師から「情報が十分」とのフィードバック取得率を **80% 以上** にする（期限: 長期／信頼度: 追加確認必要論点 — 獣医師連携プロトコル未確定、§10.4）
- **紐付 ST**: ST-02
- **信頼度区分**: 資料上確認できる事実（KR-02-1, KR-02-2）／追加確認必要論点（KR-02-3, KR-02-4）

### OKR-03: 体調変化の早期検知でシニア・ハイリスク犬を守る

- **Objective**: 体重・食欲・症状の時系列把握を仕組み化し、異変の検知から獣医師連絡までを短縮する
- **Key Results**:
  - **KR-03-1**: 週次体重測定実施率（KPI-05）を月次平均 **90% 以上** にする（期限: MVP+3ヶ月／信頼度: 合理的仮説）
  - **KR-03-2**: 体調変化検知リードタイム（KPI-06）の中央値を **24 時間以内** にする（期限: 長期+6ヶ月／信頼度: 追加確認必要論点 — §9 で目標値未設定）
  - **KR-03-3**: シニア犬ちょこ＋低血糖リスク犬ルナの観察テンプレ実施率（KPI-07）を **80% 以上** にする（期限: 長期／信頼度: 合理的仮説）
- **紐付 ST**: ST-03
- **信頼度区分**: 合理的仮説（KR-03-1, KR-03-3）／追加確認必要論点（KR-03-2）

### OKR-04: 家族全員が安心して使える共有プラットフォームを維持する

- **Objective**: 家族間のタスク共有を確実化し、家族外への情報漏えいを発生させない
- **Key Results**:
  - **KR-04-1**: 家族間タスク重複・抜け件数（KPI-08）を月次 **0 件** にする（期限: MVP+3ヶ月／信頼度: 合理的仮説）
  - **KR-04-2**: 不正アクセス・家族外漏えい件数（KPI-09）を **0 件** で維持する（期限: 全期間／信頼度: 資料上確認できる事実）
  - **KR-04-3**: 家族メンバー全員（人数は追加確認必要論点）の週次アクティブ率を **100%** にする（期限: MVP+6ヶ月／信頼度: 合理的仮説 — 家族人数未確定のため母数 TBD）
- **紐付 ST**: ST-04, ST-05
- **信頼度区分**: 資料上確認できる事実（KR-04-2）／合理的仮説（KR-04-1, KR-04-3）

---

## 5. 計測データ定義

### 5.1 定量データ

| データ ID | 名称 | 型 | 粒度 | 収集頻度 | 保持期間（ISO 8601） | 紐付 KPI/OKR |
|---|---|---|---|---|---|---|
| DAT-01 | 投薬実施ログ | record (dog_id: string, drug_id: string, scheduled_at: timestamp, executed_at: timestamp, executor_id: string) | 1記録／犬／薬／予定時刻 | リアルタイム | P3Y（追加確認必要論点 — §10.4 写真ストレージ容量見込みに準ずる） | KPI-01, KR-01-1, KR-01-3 |
| DAT-02 | 予防薬実施ログ | record (dog_id, prevention_type [filaria/flea_tick], scheduled_at, executed_at, executor_id) | 1記録／犬／予防薬／月 | 月次 | P3Y | KPI-02, KR-01-2 |
| DAT-03 | ワクチン接種記録 | record (dog_id, vaccine_type, scheduled_at, executed_at, vet_clinic_name?, next_due_at) | 1記録／犬／ワクチン／年 | 年次 | P5Y（合理的仮説 — 一般的なワクチン接種証明の保管慣行） | KPI-02, KR-01-2 |
| DAT-04 | サマリ生成操作ログ | record (dog_id?, generation_started_at: timestamp, generation_completed_at: timestamp, duration_ms: int, user_id) | 1記録／生成イベント | リアルタイム | P1Y | KPI-03, KR-02-1 |
| DAT-05 | 通院前準備時間アンケート | record (visit_id, dog_id, self_reported_minutes: int, submitted_at, user_id) | 1記録／通院 | 通院都度 | P3Y | KPI-04, KR-02-2 |
| DAT-06 | 体重測定ログ | record (dog_id, measured_at, weight_kg: float, measurer_id) | 1記録／犬／週 | 週次 | P5Y（体重トレンドの長期保持） | KPI-05, KR-03-1 |
| DAT-07 | 症状記録ログ | record (dog_id, observed_at, symptom_type: enum, severity: 1-5, free_text?, photo_ref?, observer_id) | 1記録／犬／症状イベント | 都度 | P3Y | KPI-06, KR-03-2 |
| DAT-08 | 獣医師連絡ログ | record (dog_id, contacted_at, channel [phone/visit/other], related_symptom_id?, user_id) | 1記録／連絡イベント | 都度 | P3Y | KPI-06, KR-03-2 |
| DAT-09 | 観察テンプレ実施ログ | record (dog_id, template_id [senior_heart_murmur / low_blood_sugar / etc], executed_at, checklist_result: JSON, executor_id) | 1記録／犬／テンプレ／日 | 日次 | P3Y | KPI-07, KR-03-3 |
| DAT-10 | タスク完了ログ（横断） | record (task_id, task_type, dog_id?, scheduled_at, completed_at, executor_id) | 全タスク横断 | リアルタイム | P1Y | KPI-08, KR-04-1, KR-04-3 |
| DAT-11 | タスク重複検出ログ | record (original_task_id, duplicate_task_id, detected_at, dog_id?, task_type) | 1記録／重複検出 | 都度（バックエンドで自動検出） | P1Y | KPI-08, KR-04-1 |
| DAT-12 | 認証・アクセスログ | record (user_id?, attempted_at, result [success/denied], ip_hash, action) | 全認証イベント | リアルタイム | P1Y | KPI-09, KR-04-2 |
| DAT-13 | 家族メンバーアクティブログ | record (user_id, session_started_at, session_ended_at?, device_type) | セッション単位 | リアルタイム | P90D（短期分析のみ） | KR-04-3 |
| DAT-14 | 重大症状エスカレーション計算用 | derived (first_abnormal_event_at, vet_contact_at, lead_time_hours: float) | 1記録／症状エスカレーション | 派生（バッチ日次） | P3Y | KPI-06, KR-03-2 |

### 5.2 定性データ

| データ ID | 名称 | 収集手段 | 分析方針 | 紐付 KPI/OKR |
|---|---|---|---|---|
| DAT-Q01 | 通院後家族満足度アンケート | アプリ内アンケート（5段階＋自由記述）、通院翌日にプッシュ通知 | 5段階の月次平均値モニタリング、自由記述はキーワード集計（手動） | KR-02-3 |
| DAT-Q02 | 獣医師フィードバック | 通院時の口頭ヒアリングまたは PDF サマリへのコメント欄（紙運用も可） | 「情報が十分」と回答した件数比率を集計 | KR-02-4（運用プロトコル要追加確認 — §10.4） |
| DAT-Q03 | 家族運用負荷ヒアリング | 月次レトロスペクティブ（家族会議でのフリーフォーマット） | 入力負荷・通知頻度の調整インプット | KPI 全般の継続改善 |

---

## 6. データ収集設計（目的志向・実装インターフェース仕様）

各 KPI/OKR ID ごとに、計測対象イベント名（snake_case 既定）、属性スキーマ、計測実装手段、計測ポイント、収集タイミングを定義する。

### 6.1 実装インターフェース仕様一覧

| KPI/OKR ID | イベント名 | 属性スキーマ（必須は *） | 計測手段 | 計測ポイント | タイミング |
|---|---|---|---|---|---|
| KPI-01, KR-01-1 | `medication_administered` | dog_id*: string, drug_id*: string, scheduled_at*: ISO8601, executed_at*: ISO8601, executor_user_id*: string, dose_taken: enum [full/partial/skipped], notes: string | Application Insights カスタムイベント＋ DB 永続化（DAT-01） | フロントエンド（記録 UI） | リアルタイム |
| KPI-01, KR-01-3 | `medication_duplicate_detected` | dog_id*, drug_id*, original_event_id*, duplicate_event_id*, time_gap_seconds*: int | バックエンドジョブ（重複検出ロジック）→ Application Insights | バックエンド | リアルタイム（書込時バリデーション） |
| KPI-02, KR-01-2 | `prevention_administered` | dog_id*, prevention_type*: enum [filaria/flea_tick/vaccine], scheduled_at*, executed_at*, executor_user_id*, next_due_at: ISO8601 | アプリ DB＋ Application Insights | フロントエンド | 月次／年次（実施時） |
| KPI-03, KR-02-1 | `summary_generation_started` / `summary_generation_completed` | dog_id, generation_id*: string, started_at*, completed_at*, duration_ms*: int, user_id*, output_format: enum [pdf/screen] | OpenTelemetry トレース（span: summary_generation）＋ Application Insights | バックエンド（サマリ生成 API）／フロント計測併用 | 通院都度（操作時） |
| KPI-04, KR-02-2 | `visit_preparation_self_report` | visit_id*, dog_id*, self_reported_minutes*: int, submitted_at*, user_id* | アプリ内アンケート → DB（DAT-05） | フロントエンド（通院後アンケート画面） | 通院都度 |
| KPI-05, KR-03-1 | `weight_recorded` | dog_id*, measured_at*, weight_kg*: float, measurer_user_id*, measurement_method: enum [scale/estimate] | アプリ DB＋ Application Insights | フロントエンド（体重入力 UI） | 週次 |
| KPI-06, KR-03-2 | `symptom_recorded` / `vet_contacted` | symptom_recorded: { dog_id*, observed_at*, symptom_type*: enum, severity*: int[1-5], photo_ref: string, observer_user_id* } / vet_contacted: { dog_id*, contacted_at*, channel*: enum [phone/visit/email/other], related_symptom_id: string, user_id* } | アプリ DB＋ Application Insights＋ OpenTelemetry（バッチで KPI-06 を派生算出） | フロントエンド（記録）＋バックエンド（バッチで lead_time 派生） | 都度／日次バッチ |
| KPI-07, KR-03-3 | `observation_template_executed` | dog_id*, template_id*: enum [senior_heart_murmur/low_blood_sugar_prevention/etc], executed_at*, checklist_result*: JSON, executor_user_id* | アプリ DB＋ Application Insights | フロントエンド（テンプレ実施 UI） | 日次 |
| KPI-08, KR-04-1 | `task_completed` / `task_overdue_detected` | task_completed: { task_id*, task_type*: enum, dog_id, scheduled_at*, completed_at*, executor_user_id* } / task_overdue_detected: { task_id*, scheduled_at*, detected_at*, hours_overdue*: int } | アプリ DB＋バックエンド定期ジョブ＋ Application Insights | フロントエンド／バックエンド | リアルタイム（完了）＋日次バッチ（overdue 検出） |
| KPI-09, KR-04-2 | `auth_attempt` / `family_external_share_attempt` | auth_attempt: { user_id, attempted_at*, result*: enum [success/denied/mfa_required], ip_hash*, user_agent_hash } / family_external_share_attempt: { attempted_at*, share_method, blocked*: bool } | 認証基盤ログ（Application Insights / Azure AD B2C 等）＋ DB（DAT-12） | バックエンド（認証ミドルウェア） | リアルタイム |
| KR-04-3 | `user_session_started` / `user_session_ended` | user_id*, session_id*, started_at*/ended_at*, device_type: enum [ios/android/web] | Application Insights セッションテレメトリ | フロントエンド | リアルタイム |
| KR-02-3 | `visit_satisfaction_submitted` | visit_id*, dog_id*, rating*: int[1-5], free_text: string, user_id*, submitted_at* | アプリ内アンケート → DB（DAT-Q01） | フロントエンド（通院翌日プッシュ通知導線） | 通院翌日 |
| KR-02-4 | `vet_feedback_recorded` | visit_id*, dog_id*, sufficiency_rating: enum [sufficient/partial/insufficient], free_text, recorded_at*, user_id* | アプリ内入力（家族による代理記録） → DB（DAT-Q02） | フロントエンド（通院後入力 UI） | 通院都度（運用プロトコル要追加確認 — §10.4） |

### 6.2 計測実装手段の補足

- **Application Insights**: アプリ操作の標準テレメトリ＋カスタムイベント送信に使用。家族 4-6 名規模のため SKU は Basic で十分（合理的仮説）。
- **OpenTelemetry**: サマリ生成 API のレイテンシ追跡（KPI-03）など span ベースの計測が必要な箇所で採用。Application Insights エクスポーター経由でストレージ統合。
- **アンケート**: KR-02-3／KR-02-4 など定性指標は、通院後プッシュ通知導線でアプリ内アンケートとして実装。
- **派生計算（バッチ）**: KPI-06 の lead_time、KPI-08 の月次集計は日次／月次バッチで派生テーブル（DAT-14 等）に書き込む方式とする。

### 6.3 PII（個人情報）取扱い方針

- 一次情報 §2 制約・§9 リスク対応に基づき、IP アドレス・User Agent は **ハッシュ化** して保存（生値は保持しない）。
- 写真（症状記録）は家族メンバーのみアクセス可能なストレージに保存し、家族外共有のリンク生成は実装上ブロックする（KR-04-2 の前提）。
- 詳細な PII 取扱い設計は ARD Step 4.2 ／ aas ワークフローで非機能要件として再定義する（追加確認必要論点）。

---

## 7. ID 命名規約と後続工程引き継ぎ

### 7.1 ID 命名規約

| ID 種別 | 正規表現 | 例 |
|---|---|---|
| 戦略目標 ID | `^ST-[0-9]{2,3}$` | ST-01 〜 ST-05 |
| KPI ID | `^KPI-[0-9]{2,3}$` | KPI-01 〜 KPI-09 |
| OKR ID | `^OKR-[0-9]{2,3}$` | OKR-01 〜 OKR-04 |
| Key Result ID | `^KR-[0-9]{2,3}-[0-9]+$` | KR-01-1, KR-02-3, KR-04-2 等 |
| データ ID | `^DAT-[0-9]{2,3}$` | DAT-01 〜 DAT-14（および定性 DAT-Q01〜Q03） |

> 定性データ ID `DAT-Q*` は定量データ ID `DAT-NN` と区別するため Q プレフィクスを付加（命名規約の補足拡張。本書内一意性は保たれる）。

### 7.2 後続工程への引き継ぎ

#### UC 設計（ARD Step 4.1 / 4.2）で参照すべき KPI/OKR ID 一覧

ユースケース骨格／詳細に紐付ける KPI/OKR と、対応する想定ユースケース概念:

- **KPI-01, KPI-02, OKR-01**: 投薬記録 UC、予防薬リマインド UC、二重投薬防止 UC
- **KPI-03, KPI-04, OKR-02**: 通院前サマリ生成 UC、通院記録 UC
- **KPI-05, KPI-06, KPI-07, OKR-03**: 体重記録 UC、症状記録 UC、観察テンプレ実施 UC、獣医師連絡 UC
- **KPI-08, OKR-04**: 家族タスクダッシュボード UC、タスク重複検出 UC
- **KPI-09, ST-05**: 家族メンバー招待 UC、認証 UC、アクセス制御 UC

#### APP 設計（aas ワークフロー）で参照すべき KPI/OKR ID 一覧

`docs/catalog/app-catalog.md` の「対応 KPI/OKR」列に転記対象（APP-ID は aas 実行時に決定）:

- **記録系 APP**: KPI-01, KPI-02, KPI-05, KPI-07, KPI-08
- **通院サマリ系 APP**: KPI-03, KPI-04
- **症状・獣医師連携系 APP**: KPI-06
- **家族管理・認証系 APP**: KPI-09
- **分析・ダッシュボード系 APP**（存在する場合）: OKR-01〜OKR-04 全体

#### テスト仕様（`docs/test-specs/`）で参照すべき KPI/OKR ID と運用ルール

- 各テストケースのトレーサビリティヘッダに `<!-- kpi: KPI-XX -->` または `<!-- okr: OKR-XX -->` を記載する（運用ルール提案）。
- E2E テストでは KPI-03（サマリ生成 5 分以内）など計測可能な KPI を性能テストの合格基準にマッピングする。
- KPI-09（認証・アクセス制御）はセキュリティテストの必須カバレッジ項目とする。
- 詳細運用は `docs/catalog/app-catalog.md` 整備後に `aas` ワークフロー側で具体化する。

#### 要追加確認論点（信頼度区分=追加確認必要論点 の項目一覧）

| 項目 ID | 内容 | 確認先候補 |
|---|---|---|
| KPI-06 目標値 | 体調変化検知リードタイムの具体目標値（§9 で未設定明示） | MVP 後のベースライン計測 |
| KR-02-3 ベースライン | 通院後家族満足度のベースライン未取得 | MVP リリース後 1ヶ月で初回計測 |
| KR-02-4 運用プロトコル | 獣医師フィードバック取得手順（PDF メール送付可否、共有許諾範囲） | かかりつけ動物病院（§10.4） |
| KR-04-3 家族人数 | 家族メンバー総数・役割分担が一次情報で未確定 | 家族会議でのヒアリング（§10.4） |
| DAT-01 等 保持期間 P3Y | 写真ストレージ容量見込みと保管期間ポリシー未確定 | §10.4 |
| シニア犬ちょこ観察頻度 | 心雑音観察の獣医師指示有無 | かかりつけ動物病院（§10.4） |
| ルナ低血糖対応プロトコル | 給餌頻度・緊急時手順 | かかりつけ動物病院（§10.4） |
| PII 取扱い詳細設計 | 写真ストレージのアクセス制御実装方式 | ARD Step 4.2 / aas |

---

## 検証

- **論理整合性**: ST-01〜ST-05 → KPI-01〜KPI-09 → OKR-01〜OKR-04 → DAT-01〜DAT-14 / DAT-Q01〜Q03 → §6 実装インターフェース仕様、の各 ID が相互参照されており、戦略目標から計測実装まで一気通貫でトレースできることを確認。
- **資料準拠**: 数値目標（30分→5分、投薬漏れゼロ、家族のみ閲覧、診断しない、4匹×家族共有等）は全て `docs/business-requirement.md` §1, §6, §9 に直接記載のあるものを「資料上確認できる事実」、追加で設定した数値（KPI-01 98%、KPI-05 90%、KPI-07 80% 等）は「合理的仮説」、§9 で未設定明示の KPI-06 目標値などは「追加確認必要論点」として区分済み。
- **必須セクション充足**: §1 Executive Summary／§2 戦略目標マッピング／§3 KPI 定義（SMART）／§4 OKR 定義／§5 計測データ定義（5.1 定量／5.2 定性）／§6 データ収集設計／§7 ID 命名規約と引き継ぎ、を全て収録。
- **ID 命名規約準拠**: ST-01〜05、KPI-01〜09、OKR-01〜04、KR-NN-N（KR-01-1〜KR-04-3）、DAT-01〜14 は §7.1 の正規表現に準拠。DAT-Q01〜Q03 は定性データ拡張命名として §7.1 で補足明記。
- **信頼度区分付与**: §3 KPI 全 9 項目、§4 OKR 全 4 項目（各 KR 個別にも）、§5 データ・§6 実装仕様の補足にも信頼度区分を付与済み。
- **データ収集設計の必須要素**: §6.1 で各 KPI/OKR ID に対し イベント名（snake_case）／属性スキーマ／計測実装手段（Application Insights / OpenTelemetry / アンケート）／計測ポイント／タイミング を全て記載。
- **目標値分類**: 一次情報未記載の目標値は「合理的仮説」または「追加確認必要論点」に分類済み（KPI-01, KPI-05, KPI-06, KPI-07, KR-02-3, KR-02-4 等）。
- **Markdown 構文**: テーブルヘッダ整合・テンプレ未置換プレースホルダ（`{...}`）残存なしを目視確認。

<!-- validation-confirmed -->

## 既知の制約

- 一次情報（`docs/business-requirement.md`）は家族単位の非営利業務を対象とするため、一般的な営利事業における財務 KPI（売上・利益等）は本書のスコープ外。家計コスト・健康寿命・家族心理負担に最適化した KPI 構成としている。
- KPI-01（98%）、KPI-05（90%）、KPI-07（80%）等の具体パーセンテージ目標は一次情報に直接記載がなく、運用許容枠を考慮した合理的仮説として設定。MVP リリース後のベースライン計測で見直すこと。
- 家族メンバー総数が未確定（§10.4 / KR-04-3）のため、KR-04-3 のアクティブ率分母は TBD。
- 獣医師との連携プロトコル（KR-02-4、DAT-Q02）が未確定のため、運用開始時に獣医師ヒアリングが必要。
- PII・写真ストレージの詳細設計は本書では概要方針のみ記載。ARD Step 4.2 ／ aas ワークフローで非機能要件として再定義する。

## 次にやるサブタスク

1. ARD Step 4.1（ユースケース骨格）開始時に §7.2「UC 設計で参照すべき KPI/OKR ID 一覧」を入力として参照し、各 UC に KPI/OKR ID をトレーサビリティ列として付与する。
2. aas ワークフロー（アプリケーション選定）で §7.2「APP 設計で参照すべき KPI/OKR ID 一覧」を `docs/catalog/app-catalog.md` の「対応 KPI/OKR」列に転記する。
3. §7.2「要追加確認論点」を ARD Step 4.2 詳細化フェーズの質問票へ展開する（特に獣医師連携プロトコル、家族人数、KPI-06 目標値）。
4. MVP リリース計画に KPI-01〜KPI-05 のベースライン計測手順を組み込む（§9 にも既存タスクあり）。
