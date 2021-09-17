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