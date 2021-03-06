# Design Reliability Engineering (a.k.a. Site Reliability Designing)

このリポジトリは， [SRE](https://sre.google) の思想を元に，その考えをデザインとエンジニアリングの複合領域に展開するためのドキュメント集です．  
名称 Design Reliability Engineering または Site Reliability Designing は仮であり，以下，DRE とします．

## 背景

デザインのビジネスへの長期的インパクトが語られて久しいですが，その一方優秀なデザイナーの絶対数というのは足りる見込みすらありません．

その要因の1つは，「優秀な」デザイナーというのは概して(狭義的の意味の)デザインドメイン外にも知識や理解，知見を多く持っていて，(狭義的の意味の)デザインに取り掛かる前段階(前とは限らないが便宜上)の作業がそれに比例して増えていることにあると私は思っています．  
それに加えて，デザイナーの指す範囲，つまりデザインの指す領域というのも年々拡大しつつあります．  
ユーザーが操作する画面のデザイン(いわゆる狭義のデザイン)を超え，そもそもの仕様策定や事業モデル自体へのデザイナーの寄与というのは大変大きな価値のあることであることに間違いはないですが，その他ツールの使い方やプラグイン探し，効率的なデザイナーの協調作業フローの確立，Figmaを使うならカラースタイルの整備やMain Componentsの使い方，デザイントークンの定義のためのStorybookへのコミットなどもあります．  
そして(デザインシステムを構築している場合が多いですがそれに限らず)デザイナー以外がわかる形で，デザイナー同士でのある種の暗黙の了解をきちんと書き下したり，場合によってはコンポーネントそれ自体や階層設計，APIのschemaを読み下し必要な挙動の列挙，取りうる状態をすべて書き出し，アクセシビリティチェック，デザインが許容できる範囲で表現されているかのQA，文章チェック，などなど，ここには収まりきらない量の狭義の意味ではないデザインがあるわけで，その多くがフロントエンド開発と密接に結びついています．

ある種の必然として，このような問題に対してDesign Engineerというデザインもエンジニアリングもやるという回答があります．  
ただ私はそれでは不十分だと考えます．1人の人間が1日に出せる活動は1日の1人ができるだけであり，Design Engineerになればデザインとエンジニアリングで1日2人分のアウトプットが出せるかというとまったくそんなことはないわけです．  
そしてチーム全体がDesign Engineerで構成されていればいいですが，そうもいかないのが現状です．たとえばプロトタイプを素早く作れるという意見がありますが，デザインもすべてコードをベースにしていると現状ではどうしてもチームのスケールが難しいです．  
つまり，Design Engineerの真の力を活かせるかどうかはチーム設計にかかっているということになります．

私は現状に2つの課題感をもっています．1つは拡大しすぎたデザインの指す領域，もう1つはDesign Engineerの活かし方です．この2つへの回答として，私はDREという考え方の提起をします．

## DREという考え方

DREを執り行うチームは，そのままデザインチームに入っても，開発チーム(フロントエンド)に入っても業務が遂行できるメンバーで構成されるべきです．そのような人材がいない場合は設置するべきではありません．かえって混乱の原因となります．  

DREはDesign Reliability Engineeringという名の通り，デザインの信頼性を高めるために仕事をします．

### デザインの信頼性

信頼性の高いデザインにはいくつかの定義があり，誰にとっての信頼性なのか，そしてどうあれば信頼性が高いのか，です．

#### ユーザーから見たデザインの信頼性

- 他のシステムとの一貫性 (戻るボタンを押したらページが戻れるなど，期待した動作がおこる)
- アクセシビリティ
- あらゆる状況できちんと表示が整っているか

#### デザイナーから見たデザインの信頼性

- システムで許容できる範囲できちんと表現されている
- 触りやすい(複雑な操作をせずとも完成品をいじれる)

#### エンジニアから見たデザインの信頼性

- 様々な状態が考慮されている
- デザイントークンの使い方に一貫性がある
- 簡易的な状態は自分でデザイン(組み立て)できるだけの資料がある(デザインシステムの担保すべき点)


---

ここまで書きたいTODO:

- 信頼性のバジェットの打ち立てが大きな仕事 (チームに寄る，場合によってはシステム的にやることを諦める選択も)
- アクセシビリティの自動テスト構築
- CDの構築，及びデザイナーが触りやすいように調整(いろんな状態を触って見れる形にする，もっというとDIがやりやすい，つまりUIをロジックと分離し状態をInjectionできるようにしておく)
- VRTの導入，値が低い場合には確認 (PR Review)
- PR Review: デザイナーの想定した挙動になっているか，状態の検討漏れはないか 多い場合は担当デザイナーが協力できるようにして(すぐ開ける，確認すべき状態のリストアップ)確認してもらう
- デザインレビュー: 状態に考慮漏れがないか，非明示な部分はデザインシステムで補えるか
- コードの共通化: デザインを見つつ共通化できそうなところ，したほうがいいところはデザインシステムのコンポーネントにまとめる
- テキストlinterの導入
- Figmaデータの整備 デザイン的に不整合がない形でAuto Layoutの整備，共通化やMain Componentにする部分，コンポーネントの整理，
- デザインファイルの整理，「より働きやすい環境」に
- 必要なプラグイン，システムの定義 (たとえばFigmaはレビューの仕組みがないので，Pluginで強化)
- 実プロダクトとデザインの乖離チェック，あまりに離れているよう(数字がほしい)であればDREが直接修正 業務時間の50%以上になるならエンジニアに修正依頼
- デザインファイルには直接なくとも推測できる部分についてチェック たとえば画面下部にあるメニューはきちんとボタンの上に表示されているかとか
- デザインシステムの整備 実際のデザインとの乖離が起きているならデザイナーと共有しつつ修正 (デザインかドキュメント) あきらかにデザインが間違っている場合はデザインとプロダクトのコードを修正し，ドキュメントが欠けている場合は書き足す
- 実プロダクトで間違った表現やテキストを見つけたら修正 (たとえば脆弱性の説明とか)
- デザインシステムの啓蒙 (デザイナーに考慮漏れで差し戻されるときに，本当にデザインシステムを読んでもわからないか確認など)
- 必要な状態の列挙 (予め)
- ひつようならサンプルデータの用意