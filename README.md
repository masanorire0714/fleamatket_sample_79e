# FURIMA
- メルカリクローンのフリーマーケットサイトです。
- TECH CAMP 79期短期集中コースEチームで作成。
- 作成期間 2020/7/21 ~ 2020/8/12
![スクリーンショット 2020-08-14 17 53 20](https://user-images.githubusercontent.com/67250685/90232191-3de28d80-de57-11ea-9f75-306346db7ac9.jpg)
# 主な使用言語
- Ruby
- Ruby on Rails
- Haml
- Sass
- JavaScript
- jQuery
- AWS
# 機能紹介
- 新規会員登録・ログインをすると商品の購入、出品ができます。
- 新規会員登録、ログインがお済みでない方も商品の一覧、詳細を閲覧可能です。
- 決済方法はご自身のクレジットカードを登録して購入できます。
# URL
IPアドレス：18.179.248.73
# Basic認証
ID：s_team_e
<br>
Pass：team_e_y
# テスト用アカウント
## 購入者用
メールアドレス：teame_e@sample.com
<br>
パスワード：teameteame
## 購入用カード情報
番号：4242424242424242
<br>
期限：12/23
<br>
セキュリティカード：123
## 出品者用
メールアドレス：yamada@sample.com
<br>
パスワード：yamataro
# 制作メンバー&実装内容の紹介
## 大室晶寛
- DB設計
- ユーザー新規登録・ログイン画面（マークアップ）
- カード登録機能
- 商品購入機能
- 商品検索機能
## 久保田顕
- スクラムマスター
- トップページ（マークアップ）
- 商品詳細ページ
- 商品編集機能
- 商品削除機能
## 熊谷里美
- ユーザー新規登録・ログイン画面（サーバーサイド）
- ウィザード形式フォーム実装
- マイページ
- ユーザー情報編集機能
## 和田真学
- デプロイ担当、AWS EC2~S3導入
- Basic認証
- トップページ / ヘッダー・フッター（マークアップ）
- 商品出品機能
- カテゴリー機能
# DB 設計
## users テーブル
| Column   | Type   | Options    |
| -------- | ------ | ---------- |
| nickname | string | null:false |
| email   | string | null:false, unique: true |
| password | string | null:false |
| last_name |string| null:false |
| first_name | string | null:false |
| last_name_kana |string | null:false |
| first_name_kana | string | null:false |
| birth_date | date | null:false |
| tel_number | string ||
| profile_text | text ||
| profile_image | string ||
### Association
- has_many :bought_items, foreign_key: “buyer_id”, class_name: “Item”, dependent: :destroy
- has_many :selling_items, -> { where(“buyer_id is NULL”) }, foreign_key: “seller_id”, class_name: “Item”, dependent: :destroy
- has_many :sold_items, -> { where(“buyer_id is not NULL”) }, foreign_key: “seller_id”, class_name: “Item”, dependent: :destroy
- has_many :cards, dependent: :destroy
- has_one :address, dependent: :destroy
## addresses テーブル
| Column   | Type   | Options    |
| -------- | ------ | ---------- |
| send_last_name | string | null:false |
| send_first_name |string | null:false |
| send_last_name_kana |string | null:false |
| send_first_name_kana | string | null:false |
| postal_code | integer | null:false |
| prefecture_id(active_hash) | integer | null:false |
| city |string | null:false |
| house_number | integer | null:false |
| room_number | integer ||
| user | references | null:false, foreign_key: true |
### Association
- belongs_to :user
## items テーブル
| Column   | Type   | Options    |
| -------- | ------ | ---------- |
| name | string | null:false |
| text | text | null:false |
| price | string | null:false |
| condition | string | null:false |
| category | references | null:false, foreign_key: true|
| brand | references | foreign_key: true |
| delivery_fee | string | null:false |
| prefecture_id(active_hash) | integer |null: false|
| days | string | null:false |
| seller | references | null:false, foreign_key: true |
| buyer | references | foreign_key: true |
### Association
- belongs_to :seller, class_name: “User”, foreign_key:“seller_id”
- belongs_to :buyer, class_name: “User”, foreign_key: “buyer_id”
- belongs_to :category
- belongs_to :brand
- has_many :images, dependent: :destroy
## images テーブル
| Column   | Type   | Options    |
| -------- | ------ | ---------- |
| image | string | null:false |
| item | references | null:false, foreign_key: true |
### Association
- belongs_to :item
## categories テーブル
| Column   | Type   | Options    |
| -------- | ------ | ---------- |
| name | string | null:false |
| ancestry | string |  |
### Association
- has_many :items
- has_ancestry
## brands テーブル
| Column   | Type   | Options    |
| -------- | ------ | ---------- |
| name | string |  |
### Association
- has_many :items
## cards テーブル
| Column   | Type   | Options    |
| -------- | ------ | ---------- |
| user | references | null:false, foreign_key: true |
| card_number | string | null:false |
| customer_id | string | null:false |
### Association
- belongs_to :user