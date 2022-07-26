# Hadoopについて5分で理解する
配属から約1ヶ月経ち、分からないということが分かるようになったので、手早くまとめたいと思います。
もし今後、Hadoop,Hiveなどに触れる機会がある方は参考にしてみてください。

## とりあえず知っとくHadoop関連ワード
- Apache Hadoop : 大規模データセットの並列分散処理を実現するミドルウェア
  - MapReduce : 分散処理エンジン
  - YARN : リソースマネージャー (Yet Another Resource Negotiator)
  - HDFS : 分散ファイルシステム (Hadoop Distributed File System)
- Hive, Hive QL : Hadoopなどで動作するクエリエンジン
- Hbase : 分散型データベース管理システム（NoSQL

## Apache Hadoopとは
> Apache Hadoopとは、大規模データを効率的に分散処理・管理するためのソフトウェア基盤（ミドルウェア）の一つ<br>
> (引用：IT用語辞典)

### Hadoopの基本コンポーネント
|コンポーネント|概要|
|---|---|
|MapReduce|分散システム上でデータを処理するためのプログラミングモデル|
|YARN|処理に必要なリソースのマネジメントを行う|
|HDFS|大規模データを一定のブロックに分割し、分散して保存する|
#### もう少しだけ詳しく
- MapReduce : 以下の流れで分散処理を行う
  - map : 入力をkeyとvalueのペアに変換する処理。各mapをノードごとに分散する
  - shuffle : mapの出力をソートし、reducerへ送る
  - reduce : 受け取ったkeyとvalueのペア群から必要とする集計結果などを算出し、書き出しをする
- YARN : 
  - リソースをコンテナ単位で管理する
    - CPUリソースとメモリリソースを管理
    - つまりどれくらい早く、どれくらいの容量でその処理を実行するかを割り振る
  - [参考:あの日見たYARNのお仕事を僕達はまだ知らない。](https://qiita.com/keigodasu/items/09f7e0a15d721b0b5212)
- HDFS : 
  - 複数のコンピュータのローカルディスクを、一つのストレージのように扱う
  - 容易にスケールアウトができる

### Hadoopエコシステム
#### Hive
Hiveは、Hadoop上でDWHを構築する時に使うオープンソースのソフトウェア。<br>
HiveのSQLライクな言語HiveQLを使う事で、Hadoop内のデータを手軽に処理する事ができる。<br>
Hive QLを使ってSQLを実行する際、Hive がSQL構文を理解し、MapReduce処理としてHadoop上で実行される。が、RDBのように短時間でデータは帰ってこない。<br>
データ量が多いと平気で1~2時間クエリが走っている（Tez使ってるのに、、、）。
#### HBase
HBase とは Google のほとんどのサービス基盤を支えるで BigTable を元に設計されたオープンソースソフトウェア<br>
NoSQL の一つであり、Hadoop の分散ファイルシステムである HDFS 上で動作する列指向型の分散データベース
#### 他の分散処理エンジン
- Tez : MapReduceより早い。Hiveインタフェースと組み合わせて数倍~数百倍のパフォーマンスで分散処理ができる
- Apache Spark
  - MapReduceを拡張して、使いやすく高速化したもの
  - 分散処理結果をストレージではなくメモリに書き出す

## 参考文献
- [【入門】Hadoop とは？MapReduce の使い方やエコシステム一覧](https://hogetech.info/bigdata/hadoop)
- [Hadoopとは](https://qiita.com/uenohara/items/8d71ce907886422734e2)
- [Hadoopエコシステムをざっくり整理してみる](https://qiita.com/nao_yoshi/items/0519b0ab8bc75fcc7088)
- [Amazon EMR の Apache Hadoop](https://aws.amazon.com/jp/elasticmapreduce/details/hadoop/)
- [HBase とは？HBase 初心者が最初に知っておいた方がいいと思った事まとめ](https://techblog.kyamanak.com/entry/2018/03/24/192202)