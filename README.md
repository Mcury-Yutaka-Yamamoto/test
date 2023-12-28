# 第19章　給与システムのケーススタディ:実装

システムを設計するときは、テストファーストで開発することをこれまで徹底してきた。これまで長々と話してきた給与システムの設計についても同様のことを行おう。実際にコードを書くときはテストしながら進むので、非常に細かいステップを踏むのだが、ここでは本文中の区切りのいいところでコードを示すことにする。誤りのない完全なコードだけしか記載されていないからといって、私が最初からそのようなコードを書き上げたのだと勘違いしないでほしい。各コードを書き上げるまでには、幾度となくエディット、コンパイル、テストケースの実行を繰り返している。それぞれの些細な作業でしかないが、ステップごとにコードは確実に進化し、ここに記載したコードに到達しているのである。

ここではUMLによる記述もたくさん目にすることになる。このUMLは私がホワイトボードに簡単にスケッチして、ペアパートナーであるあなたに私の考えを示すためのダイアグラムだと思ってほしい。UMLは、あなたと私を結ぶコミュニケーションツールとしてとても役立つ。

トランザクションを表現する抽象クラスTransactionを図19-1に示す。このクラスはExecuteという名前のメソッドを持っている。これはもちろん、Commandパターンである。Transactionクラスの実装をリスト19-1に示す。

<image src="/image/図19-1 Transactionインターフェース.png" alt="図19-1 Transactionインターフェース">
図19-1 Transactionインターフェース

## 19.1 従業員を追加するトランザクション
従業員を追加するトランザクションの構造の候補 を図19-2に示す。このトランザクションの中で、 従業員の給与スケジュールと給与体系が結び付いていることに注意して欲しい。これはこれでかまわない。なぜなら、トランザクションという概念はコアモデル (本質的なもの)の一部ではなく、テクニックとしてひねり出したものにすぎないからだ。コアモデル(図18-6)はこの関連について何も知らないし、そもそもこの関連はテクニックの一部にすぎないから、いつだって変更可能なのだ。たとえば、従業員の給与スケジュールを変更するためのトランザクションをこの中に追加することはいつだってできる。
給与の支払い方法のデフォルトが、給与担当者に給与小切手を預けること(HoldMethod)である点にも注意して欲しい。従業員がデフォルト以外の支払い方法を希望する場合、ChgEmpトランザクションを使って変更処理を行わなければならない。
いつものように、テストファーストでコードを書き始めよう。リスト19-2に、AddSalariedTransactionが正常に動いているかをチェックするテストケースのひとつを示す。このテストケースをパスするコードはあとで記述する。