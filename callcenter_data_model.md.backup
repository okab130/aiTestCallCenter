# コールセンターシステム データモデル設計書

## 概要

コールセンターにおける問い合わせ対応業務を管理するシステムのデータモデル設計

---

## テーブル一覧

### マスタテーブル
1. customers - 顧客マスタ
2. users - ユーザ管理（オペレーター・管理者）
3. departments - 部署マスタ
4. categories - 問い合わせカテゴリマスタ
5. products - 製品・サービスマスタ
6. faq - FAQ（よくある質問）マスタ
7. response_templates - 定型文テンプレートマスタ

### トランザクションテーブル
8. inquiries - 問い合わせ管理
9. inquiry_responses - 対応履歴
10. file_uploads - アップロード管理
11. inquiry_assignments - 担当者割当履歴
12. inquiry_transfers - エスカレーション・転送履歴
13. customer_contacts - 顧客連絡先履歴
14. inquiry_tags - 問い合わせタグ付け
15. sla_metrics - SLA（Service Level Agreement）メトリクス
16. call_logs - 通話ログ
17. email_logs - メールログ
18. chat_logs - チャットログ
19. satisfaction_surveys - 顧客満足度調査
20. knowledge_base - ナレッジベース

---

## テーブル定義

### 1. customers（顧客マスタ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| customer_id | BIGINT | NOT NULL | 顧客ID | PK, 自動生成 |
| customer_code | VARCHAR(50) | | 顧客コード | ユニーク |
| customer_type | VARCHAR(20) | NOT NULL | 顧客区分 | individual/corporate |
| company_name | VARCHAR(200) | | 法人名 | 法人の場合 |
| last_name | VARCHAR(100) | | 姓 | |
| first_name | VARCHAR(100) | | 名 | |
| last_name_kana | VARCHAR(100) | | 姓（カナ） | |
| first_name_kana | VARCHAR(100) | | 名（カナ） | |
| email | VARCHAR(255) | | メールアドレス | |
| phone_primary | VARCHAR(20) | | 主電話番号 | |
| phone_secondary | VARCHAR(20) | | 副電話番号 | |
| postal_code | VARCHAR(10) | | 郵便番号 | |
| prefecture | VARCHAR(50) | | 都道府県 | |
| city | VARCHAR(100) | | 市区町村 | |
| address_line1 | VARCHAR(200) | | 住所1 | |
| address_line2 | VARCHAR(200) | | 住所2 | |
| birth_date | DATE | | 生年月日 | |
| gender | VARCHAR(10) | | 性別 | male/female/other |
| customer_rank | VARCHAR(20) | | 顧客ランク | VIP/standard/new |
| registration_date | DATE | NOT NULL | 登録日 | |
| last_contact_date | DATETIME | | 最終接触日 | |
| total_inquiries | INT | DEFAULT 0 | 総問い合わせ数 | |
| is_blacklist | BOOLEAN | DEFAULT FALSE | ブラックリストフラグ | |
| blacklist_reason | TEXT | | ブラックリスト理由 | |
| notes | TEXT | | 備考 | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| created_by | BIGINT | | 作成者 | FK: users |
| updated_at | DATETIME | | 更新日時 | |
| updated_by | BIGINT | | 更新者 | FK: users |

### 2. users（ユーザ管理）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| user_id | BIGINT | NOT NULL | ユーザーID | PK, 自動生成 |
| username | VARCHAR(100) | NOT NULL | ユーザー名 | ユニーク |
| email | VARCHAR(255) | NOT NULL | メールアドレス | ユニーク |
| password_hash | VARCHAR(255) | NOT NULL | パスワードハッシュ | |
| employee_code | VARCHAR(50) | | 社員番号 | |
| last_name | VARCHAR(100) | NOT NULL | 姓 | |
| first_name | VARCHAR(100) | NOT NULL | 名 | |
| department_id | BIGINT | | 部署ID | FK: departments |
| role | VARCHAR(50) | NOT NULL | 役割 | admin/supervisor/operator |
| skill_level | INT | DEFAULT 1 | スキルレベル | 1-5 |
| extension_number | VARCHAR(20) | | 内線番号 | |
| max_concurrent_inquiries | INT | DEFAULT 5 | 最大同時対応数 | |
| is_available | BOOLEAN | DEFAULT TRUE | 対応可能フラグ | |
| availability_status | VARCHAR(20) | | ステータス | available/busy/break/offline |
| last_login | DATETIME | | 最終ログイン | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| updated_at | DATETIME | | 更新日時 | |

### 3. departments（部署マスタ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| department_id | BIGINT | NOT NULL | 部署ID | PK, 自動生成 |
| department_code | VARCHAR(50) | NOT NULL | 部署コード | ユニーク |
| department_name | VARCHAR(100) | NOT NULL | 部署名 | |
| parent_department_id | BIGINT | | 親部署ID | FK: departments（自己参照） |
| manager_user_id | BIGINT | | 責任者ID | FK: users |
| email | VARCHAR(255) | | 部署メール | |
| phone | VARCHAR(20) | | 部署電話番号 | |
| description | TEXT | | 説明 | |
| display_order | INT | DEFAULT 0 | 表示順 | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| updated_at | DATETIME | | 更新日時 | |

### 4. categories（問い合わせカテゴリマスタ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| category_id | BIGINT | NOT NULL | カテゴリID | PK, 自動生成 |
| category_code | VARCHAR(50) | NOT NULL | カテゴリコード | ユニーク |
| category_name | VARCHAR(100) | NOT NULL | カテゴリ名 | |
| parent_category_id | BIGINT | | 親カテゴリID | FK: categories（自己参照） |
| category_level | INT | DEFAULT 1 | カテゴリ階層 | 1=大分類, 2=中分類, 3=小分類 |
| responsible_department_id | BIGINT | | 担当部署ID | FK: departments |
| sla_response_minutes | INT | | SLA応答時間（分） | |
| sla_resolution_hours | INT | | SLA解決時間（時間） | |
| priority_default | VARCHAR(20) | DEFAULT 'medium' | デフォルト優先度 | low/medium/high/urgent |
| icon | VARCHAR(50) | | アイコン名 | |
| color_code | VARCHAR(10) | | カラーコード | #RRGGBB |
| description | TEXT | | 説明 | |
| display_order | INT | DEFAULT 0 | 表示順 | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| updated_at | DATETIME | | 更新日時 | |

### 5. products（製品・サービスマスタ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| product_id | BIGINT | NOT NULL | 製品ID | PK, 自動生成 |
| product_code | VARCHAR(50) | NOT NULL | 製品コード | ユニーク |
| product_name | VARCHAR(200) | NOT NULL | 製品名 | |
| product_type | VARCHAR(50) | | 製品タイプ | hardware/software/service |
| category_id | BIGINT | | カテゴリID | FK: categories |
| version | VARCHAR(50) | | バージョン | |
| release_date | DATE | | リリース日 | |
| end_of_support_date | DATE | | サポート終了日 | |
| manufacturer | VARCHAR(200) | | メーカー | |
| model_number | VARCHAR(100) | | 型番 | |
| description | TEXT | | 説明 | |
| manual_url | VARCHAR(500) | | マニュアルURL | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| updated_at | DATETIME | | 更新日時 | |

### 6. faq（よくある質問マスタ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| faq_id | BIGINT | NOT NULL | FAQ ID | PK, 自動生成 |
| category_id | BIGINT | | カテゴリID | FK: categories |
| product_id | BIGINT | | 製品ID | FK: products |
| question | TEXT | NOT NULL | 質問 | |
| answer | TEXT | NOT NULL | 回答 | |
| keywords | TEXT | | キーワード | カンマ区切り |
| view_count | INT | DEFAULT 0 | 閲覧回数 | |
| helpful_count | INT | DEFAULT 0 | 役立った数 | |
| not_helpful_count | INT | DEFAULT 0 | 役立たなかった数 | |
| display_order | INT | DEFAULT 0 | 表示順 | |
| is_public | BOOLEAN | DEFAULT TRUE | 公開フラグ | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| created_by | BIGINT | | 作成者 | FK: users |
| updated_at | DATETIME | | 更新日時 | |
| updated_by | BIGINT | | 更新者 | FK: users |

### 7. response_templates（定型文テンプレートマスタ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| template_id | BIGINT | NOT NULL | テンプレートID | PK, 自動生成 |
| template_code | VARCHAR(50) | NOT NULL | テンプレートコード | ユニーク |
| template_name | VARCHAR(200) | NOT NULL | テンプレート名 | |
| category_id | BIGINT | | カテゴリID | FK: categories |
| subject | VARCHAR(500) | | 件名 | メール用 |
| body | TEXT | NOT NULL | 本文 | |
| template_type | VARCHAR(20) | NOT NULL | タイプ | email/chat/sms |
| language | VARCHAR(10) | DEFAULT 'ja' | 言語 | ja/en |
| variables | TEXT | | 変数リスト | JSON形式 |
| usage_count | INT | DEFAULT 0 | 使用回数 | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| created_by | BIGINT | | 作成者 | FK: users |
| updated_at | DATETIME | | 更新日時 | |
| updated_by | BIGINT | | 更新者 | FK: users |

### 8. inquiries（問い合わせ管理）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| inquiry_id | BIGINT | NOT NULL | 問い合わせID | PK, 自動生成 |
| inquiry_number | VARCHAR(50) | NOT NULL | 問い合わせ番号 | ユニーク, INQ-YYYYMMDD-0001 |
| customer_id | BIGINT | NOT NULL | 顧客ID | FK: customers |
| product_id | BIGINT | | 製品ID | FK: products |
| category_id | BIGINT | | カテゴリID | FK: categories |
| subject | VARCHAR(500) | NOT NULL | 件名 | |
| description | TEXT | NOT NULL | 詳細内容 | |
| channel | VARCHAR(20) | NOT NULL | 受付チャネル | phone/email/chat/web/social |
| priority | VARCHAR(20) | NOT NULL | 優先度 | low/medium/high/urgent |
| status | VARCHAR(20) | NOT NULL | ステータス | new/assigned/in_progress/pending/resolved/closed/canceled |
| assigned_user_id | BIGINT | | 担当者ID | FK: users |
| assigned_department_id | BIGINT | | 担当部署ID | FK: departments |
| assigned_at | DATETIME | | 割当日時 | |
| first_response_at | DATETIME | | 初回応答日時 | |
| resolved_at | DATETIME | | 解決日時 | |
| closed_at | DATETIME | | クローズ日時 | |
| due_date | DATETIME | | 期限日時 | SLAベース |
| estimated_resolution_date | DATETIME | | 解決予定日 | |
| resolution_type | VARCHAR(50) | | 解決区分 | solved/workaround/duplicate/cannot_reproduce/wont_fix |
| resolution_summary | TEXT | | 解決内容サマリ | |
| customer_satisfaction | INT | | 顧客満足度 | 1-5 |
| response_time_minutes | INT | | 応答時間（分） | |
| resolution_time_hours | DECIMAL(10,2) | | 解決時間（時間） | |
| is_sla_breached | BOOLEAN | DEFAULT FALSE | SLA違反フラグ | |
| is_escalated | BOOLEAN | DEFAULT FALSE | エスカレーションフラグ | |
| escalation_level | INT | DEFAULT 0 | エスカレーションレベル | 0-3 |
| parent_inquiry_id | BIGINT | | 親問い合わせID | FK: inquiries（関連問い合わせ） |
| tags | TEXT | | タグ | JSON配列 |
| internal_notes | TEXT | | 内部メモ | 顧客には非表示 |
| is_deleted | BOOLEAN | DEFAULT FALSE | 論理削除フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| created_by | BIGINT | | 作成者 | FK: users |
| updated_at | DATETIME | | 更新日時 | |
| updated_by | BIGINT | | 更新者 | FK: users |

### 9. inquiry_responses（対応履歴）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| response_id | BIGINT | NOT NULL | 対応ID | PK, 自動生成 |
| inquiry_id | BIGINT | NOT NULL | 問い合わせID | FK: inquiries |
| response_type | VARCHAR(20) | NOT NULL | 対応タイプ | call/email/chat/internal_note/status_change |
| direction | VARCHAR(10) | | 方向 | inbound/outbound |
| user_id | BIGINT | | 対応者ID | FK: users |
| response_text | TEXT | | 対応内容 | |
| previous_status | VARCHAR(20) | | 変更前ステータス | |
| new_status | VARCHAR(20) | | 変更後ステータス | |
| duration_seconds | INT | | 対応時間（秒） | 通話・チャット時間 |
| is_customer_visible | BOOLEAN | DEFAULT TRUE | 顧客表示フラグ | |
| template_id | BIGINT | | 使用テンプレートID | FK: response_templates |
| satisfaction_rating | INT | | この対応の満足度 | 1-5 |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| created_by | BIGINT | | 作成者 | FK: users |

### 10. file_uploads（アップロード管理）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| file_id | BIGINT | NOT NULL | ファイルID | PK, 自動生成 |
| inquiry_id | BIGINT | | 問い合わせID | FK: inquiries |
| response_id | BIGINT | | 対応ID | FK: inquiry_responses |
| file_name | VARCHAR(255) | NOT NULL | ファイル名 | |
| original_file_name | VARCHAR(255) | NOT NULL | オリジナルファイル名 | |
| file_path | VARCHAR(500) | NOT NULL | ファイルパス | |
| file_size | BIGINT | NOT NULL | ファイルサイズ（バイト） | |
| file_type | VARCHAR(100) | | MIMEタイプ | |
| file_extension | VARCHAR(10) | | 拡張子 | |
| upload_source | VARCHAR(20) | | アップロード元 | customer/operator/system |
| description | TEXT | | 説明 | |
| is_public | BOOLEAN | DEFAULT FALSE | 公開フラグ | |
| download_count | INT | DEFAULT 0 | ダウンロード回数 | |
| virus_scan_status | VARCHAR(20) | | ウイルススキャン状態 | pending/clean/infected |
| virus_scan_date | DATETIME | | スキャン日時 | |
| is_deleted | BOOLEAN | DEFAULT FALSE | 論理削除フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| created_by | BIGINT | | 作成者 | FK: users |

### 11. inquiry_assignments（担当者割当履歴）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| assignment_id | BIGINT | NOT NULL | 割当ID | PK, 自動生成 |
| inquiry_id | BIGINT | NOT NULL | 問い合わせID | FK: inquiries |
| from_user_id | BIGINT | | 割当元ユーザーID | FK: users |
| to_user_id | BIGINT | NOT NULL | 割当先ユーザーID | FK: users |
| from_department_id | BIGINT | | 割当元部署ID | FK: departments |
| to_department_id | BIGINT | | 割当先部署ID | FK: departments |
| assignment_type | VARCHAR(20) | NOT NULL | 割当タイプ | auto/manual/round_robin/skill_based |
| reason | TEXT | | 割当理由 | |
| notes | TEXT | | 備考 | |
| assigned_at | DATETIME | NOT NULL | 割当日時 | |
| assigned_by | BIGINT | | 割当実行者 | FK: users |

### 12. inquiry_transfers（エスカレーション・転送履歴）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| transfer_id | BIGINT | NOT NULL | 転送ID | PK, 自動生成 |
| inquiry_id | BIGINT | NOT NULL | 問い合わせID | FK: inquiries |
| transfer_type | VARCHAR(20) | NOT NULL | 転送タイプ | escalation/transfer/delegation |
| from_user_id | BIGINT | NOT NULL | 転送元ユーザーID | FK: users |
| to_user_id | BIGINT | NOT NULL | 転送先ユーザーID | FK: users |
| from_department_id | BIGINT | | 転送元部署ID | FK: departments |
| to_department_id | BIGINT | | 転送先部署ID | FK: departments |
| escalation_level | INT | | エスカレーションレベル | 1-3 |
| reason | TEXT | NOT NULL | 転送理由 | |
| notes | TEXT | | 申し送り事項 | |
| status | VARCHAR(20) | DEFAULT 'pending' | ステータス | pending/accepted/rejected |
| accepted_at | DATETIME | | 承認日時 | |
| rejected_reason | TEXT | | 拒否理由 | |
| transferred_at | DATETIME | NOT NULL | 転送日時 | |
| transferred_by | BIGINT | | 転送実行者 | FK: users |

### 13. customer_contacts（顧客連絡先履歴）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| contact_id | BIGINT | NOT NULL | 連絡先ID | PK, 自動生成 |
| customer_id | BIGINT | NOT NULL | 顧客ID | FK: customers |
| contact_type | VARCHAR(20) | NOT NULL | 連絡先タイプ | email/phone/mobile/fax |
| contact_value | VARCHAR(255) | NOT NULL | 連絡先 | |
| is_primary | BOOLEAN | DEFAULT FALSE | 主連絡先フラグ | |
| is_verified | BOOLEAN | DEFAULT FALSE | 確認済みフラグ | |
| verified_at | DATETIME | | 確認日時 | |
| label | VARCHAR(50) | | ラベル | home/office/personal |
| notes | TEXT | | 備考 | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| updated_at | DATETIME | | 更新日時 | |

### 14. inquiry_tags（問い合わせタグ付け）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| tag_id | BIGINT | NOT NULL | タグID | PK, 自動生成 |
| tag_name | VARCHAR(50) | NOT NULL | タグ名 | ユニーク |
| tag_type | VARCHAR(20) | | タグタイプ | technical/business/urgency/quality |
| color_code | VARCHAR(10) | | カラーコード | #RRGGBB |
| description | TEXT | | 説明 | |
| usage_count | INT | DEFAULT 0 | 使用回数 | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |

### 15. sla_metrics（SLAメトリクス）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| metric_id | BIGINT | NOT NULL | メトリクスID | PK, 自動生成 |
| inquiry_id | BIGINT | NOT NULL | 問い合わせID | FK: inquiries |
| category_id | BIGINT | | カテゴリID | FK: categories |
| priority | VARCHAR(20) | NOT NULL | 優先度 | |
| sla_response_target_minutes | INT | | SLA応答目標（分） | |
| actual_response_minutes | INT | | 実際の応答時間（分） | |
| is_response_breached | BOOLEAN | DEFAULT FALSE | 応答時間違反フラグ | |
| sla_resolution_target_hours | INT | | SLA解決目標（時間） | |
| actual_resolution_hours | DECIMAL(10,2) | | 実際の解決時間（時間） | |
| is_resolution_breached | BOOLEAN | DEFAULT FALSE | 解決時間違反フラグ | |
| business_hours_only | BOOLEAN | DEFAULT TRUE | 営業時間のみカウント | |
| pause_time_minutes | INT | DEFAULT 0 | 一時停止時間（分） | 顧客待ち時間等 |
| calculated_at | DATETIME | | 計算日時 | |
| created_at | DATETIME | NOT NULL | 作成日時 | |

### 16. call_logs（通話ログ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| call_id | BIGINT | NOT NULL | 通話ID | PK, 自動生成 |
| inquiry_id | BIGINT | | 問い合わせID | FK: inquiries |
| customer_id | BIGINT | | 顧客ID | FK: customers |
| user_id | BIGINT | | オペレーターID | FK: users |
| call_direction | VARCHAR(10) | NOT NULL | 通話方向 | inbound/outbound |
| caller_number | VARCHAR(20) | | 発信者番号 | |
| called_number | VARCHAR(20) | | 着信者番号 | |
| call_start_time | DATETIME | NOT NULL | 通話開始時刻 | |
| call_end_time | DATETIME | | 通話終了時刻 | |
| call_duration_seconds | INT | | 通話時間（秒） | |
| wait_time_seconds | INT | | 待機時間（秒） | |
| call_status | VARCHAR(20) | | ステータス | answered/missed/busy/failed |
| transfer_count | INT | DEFAULT 0 | 転送回数 | |
| hold_count | INT | DEFAULT 0 | 保留回数 | |
| hold_duration_seconds | INT | DEFAULT 0 | 保留時間（秒） | |
| recording_url | VARCHAR(500) | | 録音URL | |
| recording_duration_seconds | INT | | 録音時間（秒） | |
| notes | TEXT | | 通話メモ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |

### 17. email_logs（メールログ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| email_id | BIGINT | NOT NULL | メールID | PK, 自動生成 |
| inquiry_id | BIGINT | | 問い合わせID | FK: inquiries |
| response_id | BIGINT | | 対応ID | FK: inquiry_responses |
| customer_id | BIGINT | | 顧客ID | FK: customers |
| direction | VARCHAR(10) | NOT NULL | 方向 | inbound/outbound |
| from_address | VARCHAR(255) | NOT NULL | 送信元アドレス | |
| to_address | VARCHAR(255) | NOT NULL | 送信先アドレス | |
| cc_address | TEXT | | CCアドレス | カンマ区切り |
| bcc_address | TEXT | | BCCアドレス | カンマ区切り |
| subject | VARCHAR(500) | | 件名 | |
| body_text | TEXT | | 本文（テキスト） | |
| body_html | TEXT | | 本文（HTML） | |
| message_id | VARCHAR(255) | | メッセージID | |
| in_reply_to | VARCHAR(255) | | 返信先メッセージID | |
| email_status | VARCHAR(20) | | ステータス | sent/delivered/failed/bounced |
| sent_at | DATETIME | | 送信日時 | |
| delivered_at | DATETIME | | 配信日時 | |
| opened_at | DATETIME | | 開封日時 | |
| attachment_count | INT | DEFAULT 0 | 添付ファイル数 | |
| is_spam | BOOLEAN | DEFAULT FALSE | スパムフラグ | |
| spam_score | DECIMAL(5,2) | | スパムスコア | |
| created_at | DATETIME | NOT NULL | 作成日時 | |

### 18. chat_logs（チャットログ）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| chat_id | BIGINT | NOT NULL | チャットID | PK, 自動生成 |
| inquiry_id | BIGINT | | 問い合わせID | FK: inquiries |
| customer_id | BIGINT | | 顧客ID | FK: customers |
| user_id | BIGINT | | オペレーターID | FK: users |
| session_id | VARCHAR(100) | NOT NULL | セッションID | |
| chat_start_time | DATETIME | NOT NULL | チャット開始時刻 | |
| chat_end_time | DATETIME | | チャット終了時刻 | |
| message_count | INT | DEFAULT 0 | メッセージ数 | |
| customer_message_count | INT | DEFAULT 0 | 顧客メッセージ数 | |
| operator_message_count | INT | DEFAULT 0 | オペレーターメッセージ数 | |
| average_response_seconds | INT | | 平均応答時間（秒） | |
| wait_time_seconds | INT | | 待機時間（秒） | |
| chat_transcript | TEXT | | チャット内容（全文） | JSON形式 |
| chat_platform | VARCHAR(20) | | プラットフォーム | web/line/facebook/whatsapp |
| customer_satisfaction | INT | | 顧客満足度 | 1-5 |
| is_bot_assisted | BOOLEAN | DEFAULT FALSE | Bot支援フラグ | |
| bot_accuracy | DECIMAL(5,2) | | Bot精度 | 0-100% |
| created_at | DATETIME | NOT NULL | 作成日時 | |

### 19. satisfaction_surveys（顧客満足度調査）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| survey_id | BIGINT | NOT NULL | 調査ID | PK, 自動生成 |
| inquiry_id | BIGINT | NOT NULL | 問い合わせID | FK: inquiries |
| customer_id | BIGINT | NOT NULL | 顧客ID | FK: customers |
| user_id | BIGINT | | 担当者ID | FK: users |
| overall_rating | INT | | 総合評価 | 1-5 |
| response_speed_rating | INT | | 応答速度評価 | 1-5 |
| solution_quality_rating | INT | | 解決品質評価 | 1-5 |
| staff_attitude_rating | INT | | 対応態度評価 | 1-5 |
| ease_of_contact_rating | INT | | 連絡のしやすさ評価 | 1-5 |
| nps_score | INT | | NPS（推奨度） | 0-10 |
| would_recommend | BOOLEAN | | 推奨意向 | |
| positive_comments | TEXT | | 良かった点 | |
| negative_comments | TEXT | | 改善点 | |
| suggestions | TEXT | | 提案・要望 | |
| survey_sent_at | DATETIME | | 調査送信日時 | |
| survey_completed_at | DATETIME | | 調査完了日時 | |
| response_time_hours | DECIMAL(10,2) | | 回答時間（時間） | |
| survey_method | VARCHAR(20) | | 調査方法 | email/sms/web/ivr |
| created_at | DATETIME | NOT NULL | 作成日時 | |

### 20. knowledge_base（ナレッジベース）

| カラム名 | データ型 | NULL | 説明 | 備考 |
|----------|----------|------|------|------|
| knowledge_id | BIGINT | NOT NULL | ナレッジID | PK, 自動生成 |
| title | VARCHAR(500) | NOT NULL | タイトル | |
| content | TEXT | NOT NULL | 内容 | |
| category_id | BIGINT | | カテゴリID | FK: categories |
| product_id | BIGINT | | 製品ID | FK: products |
| keywords | TEXT | | キーワード | カンマ区切り |
| related_inquiry_id | BIGINT | | 関連問い合わせID | FK: inquiries |
| knowledge_type | VARCHAR(20) | NOT NULL | タイプ | solution/troubleshooting/how_to/best_practice |
| difficulty_level | VARCHAR(20) | | 難易度 | beginner/intermediate/advanced |
| view_count | INT | DEFAULT 0 | 閲覧回数 | |
| helpful_count | INT | DEFAULT 0 | 役立った数 | |
| not_helpful_count | INT | DEFAULT 0 | 役立たなかった数 | |
| last_used_date | DATETIME | | 最終使用日 | |
| version | INT | DEFAULT 1 | バージョン | |
| is_public | BOOLEAN | DEFAULT FALSE | 公開フラグ | |
| is_verified | BOOLEAN | DEFAULT FALSE | 検証済みフラグ | |
| verified_by | BIGINT | | 検証者 | FK: users |
| verified_at | DATETIME | | 検証日時 | |
| is_active | BOOLEAN | DEFAULT TRUE | 有効フラグ | |
| created_at | DATETIME | NOT NULL | 作成日時 | |
| created_by | BIGINT | | 作成者 | FK: users |
| updated_at | DATETIME | | 更新日時 | |
| updated_by | BIGINT | | 更新者 | FK: users |

---

## リレーションシップ

### 主要な外部キー制約

1. **inquiries（問い合わせ）**
   - customer_id → customers.customer_id
   - product_id → products.product_id
   - category_id → categories.category_id
   - assigned_user_id → users.user_id
   - assigned_department_id → departments.department_id
   - parent_inquiry_id → inquiries.inquiry_id（自己参照）

2. **inquiry_responses（対応履歴）**
   - inquiry_id → inquiries.inquiry_id
   - user_id → users.user_id
   - template_id → response_templates.template_id

3. **file_uploads（アップロード管理）**
   - inquiry_id → inquiries.inquiry_id
   - response_id → inquiry_responses.response_id

4. **inquiry_assignments（担当者割当）**
   - inquiry_id → inquiries.inquiry_id
   - from_user_id → users.user_id
   - to_user_id → users.user_id
   - from_department_id → departments.department_id
   - to_department_id → departments.department_id

5. **inquiry_transfers（エスカレーション・転送）**
   - inquiry_id → inquiries.inquiry_id
   - from_user_id → users.user_id
   - to_user_id → users.user_id

6. **各ログテーブル**
   - inquiry_id → inquiries.inquiry_id
   - customer_id → customers.customer_id
   - user_id → users.user_id

---

## インデックス推奨

### 高速検索のための複合インデックス

```sql
-- inquiries
CREATE INDEX idx_inquiries_customer_status ON inquiries(customer_id, status);
CREATE INDEX idx_inquiries_assigned_user ON inquiries(assigned_user_id, status);
CREATE INDEX idx_inquiries_category_status ON inquiries(category_id, status);
CREATE INDEX idx_inquiries_created_at ON inquiries(created_at DESC);
CREATE INDEX idx_inquiries_due_date ON inquiries(due_date);

-- inquiry_responses
CREATE INDEX idx_responses_inquiry_created ON inquiry_responses(inquiry_id, created_at);

-- file_uploads
CREATE INDEX idx_files_inquiry ON file_uploads(inquiry_id);

-- call_logs
CREATE INDEX idx_calls_customer_date ON call_logs(customer_id, call_start_time);

-- email_logs
CREATE INDEX idx_emails_customer_date ON email_logs(customer_id, created_at);

-- chat_logs
CREATE INDEX idx_chats_customer_date ON chat_logs(customer_id, chat_start_time);
```

---

## ステータス定義

### inquiry.status（問い合わせステータス）

| ステータス | 説明 |
|-----------|------|
| new | 新規（未割当） |
| assigned | 割当済み |
| in_progress | 対応中 |
| pending | 保留中（顧客待ち・他部署待ち） |
| resolved | 解決済み |
| closed | クローズ |
| canceled | キャンセル |

### inquiry.priority（優先度）

| 優先度 | 説明 | SLA目安 |
|--------|------|---------|
| low | 低 | 48時間以内 |
| medium | 中 | 24時間以内 |
| high | 高 | 4時間以内 |
| urgent | 緊急 | 1時間以内 |

### inquiry.channel（受付チャネル）

| チャネル | 説明 |
|---------|------|
| phone | 電話 |
| email | メール |
| chat | チャット |
| web | Webフォーム |
| social | SNS（Twitter/Facebook等） |
| fax | FAX |
| walk_in | 来店 |

---

## ビジネスルール

### 1. SLA（Service Level Agreement）

- カテゴリ・優先度別にSLA設定
- 営業時間のみカウント（オプション設定可能）
- 一時停止時間の除外（顧客待ち時間など）

### 2. エスカレーション

- レベル1: 通常オペレーター
- レベル2: シニアオペレーター・スーパーバイザー
- レベル3: マネージャー・専門家

### 3. 自動割当ロジック

- ラウンドロビン方式
- スキルベース割当
- 負荷分散（同時対応数考慮）
- 優先度ベース

### 4. 顧客ランク

- VIP: 優先対応、専任担当者
- standard: 通常対応
- new: 新規顧客（初回優遇）

---

## セキュリティ・監査

### 1. 個人情報保護

- 顧客情報の暗号化
- アクセスログ記録
- 閲覧権限制御

### 2. 変更履歴

- 全テーブルに`created_at`, `updated_at`, `created_by`, `updated_by`
- 重要な変更は別途監査ログテーブルに記録

### 3. 論理削除

- `is_deleted`フラグで論理削除
- 物理削除は定期的なアーカイブ処理で実施

---

## パフォーマンス最適化

### 1. パーティショニング

- `inquiries`, `inquiry_responses`: 作成日時でパーティション（月次）
- ログテーブル: 日次パーティション

### 2. アーカイブ戦略

- 6ヶ月以上前のクローズ案件を別テーブルへ移動
- ログは3ヶ月でアーカイブ

### 3. キャッシュ戦略

- FAQ、テンプレート等のマスタはRedisキャッシュ
- 顧客情報の一時キャッシュ

---

## 拡張性

### 将来的な追加テーブル候補

- **workflows**: ワークフロー定義
- **automation_rules**: 自動化ルール
- **shift_schedules**: シフト管理
- **performance_metrics**: オペレーター業績
- **customer_segments**: 顧客セグメント
- **quality_assurance**: 品質評価
- **voice_analytics**: 音声分析
- **sentiment_analysis**: 感情分析

---

## まとめ

このデータモデルは以下の要件を満たします：

✅ **基本機能**
- 顧客管理、問い合わせ管理、対応履歴、ファイル管理

✅ **運用管理**
- SLA管理、エスカレーション、担当者割当、転送履歴

✅ **マルチチャネル対応**
- 電話、メール、チャット、各チャネルの詳細ログ

✅ **品質管理**
- 顧客満足度調査、ナレッジベース、FAQ

✅ **分析・レポート**
- SLAメトリクス、各種ログデータ、統計情報

✅ **拡張性**
- 将来的な機能追加に対応可能な設計
