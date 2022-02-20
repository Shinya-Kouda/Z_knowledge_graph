# Z_knowledge_graph

（本リポジトリは[Pelhans/Z_knowledge_graph](https://github.com/Pelhans/Z_knowledge_graph)をフォークし日本語訳したものです。）

ナレッジグラフをゼロから作り、百科事典的なナレッジグラフを構築し、それを使っていくつかの簡単なタスクを実施します。

ナレッジグラフの様々なタスクについて、初心者をガイドするセミチュートリアルのようなものです。 現在のところ、新たな追加は予定していません。

# はじめに
中国の百科事典的なナレッジグラフを構築するために、樂桂林さんのチームが作った[zhishi.me](http://zhishi.me/)を参考にさせていただきました。 百度百科、インタラクティブ百科、中国語wiki百科の知識を含めることが目標で、エンティティ数は数千万、関係数は数十億にのぼる。 百度百科と対話型百科のパートが完成し、百度百科は4,190,390項目、対話型百科は4,382,575項目となりました。 RDF形式に変換し、128,596,018個のトリプルを取得。 neo4jに格納されているのは、16,498,370ノード、56,371,456関係、61,967,517属性である。<br>

![百度百科事典ナレッジグラフデモ](http://pelhans.com/img/in-post/kg_neo4j_cypher/baidu_yanshi.png)

# リソースダウンロード

[バイドゥ SQL ファイルダウンロード バイドゥコム 抽出コード 1234](https://pan.baidu.com/s/1D-aZdziYdh4FzPGT1lSB4A)

[インタラクティブ百科事典 SQL ファイルダウンロード バイドゥコム 抽出コード rza6](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1WqDW_trdIXxNBxqT1j733Q)

[バイドゥ Neo4j ファイルダウンロード バイドゥコム 抽出コード z6fj](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1kUQLIb1TbHsWaIvYp-ncHQ)

[インタラクティブ百科事典 Neo4j ファイルダウンロード バイドゥコム 抽出コード kdkt](https://link.zhihu.com/?target=https%3A//pan.baidu.com/s/1Ba9oxM05fgCQw-cadPkhaw)

# カタログ
このマップで次のテクニックを試してみてください：    

* [Baidu百科事典とインタラクティブ百科事典からの知識の抽出](http://pelhans.com/2019/01/04/kg_from_0_note7/)    
    * 半構造化データ
       * Baidu Encyclopedia Crawler
       * インタラクティブ百科事典クローラー
    * 非構造化データ
       * WeChatパブリックアカウントクローラー
       * タイガースニッフィングWebクローラー
* 非構造化テキストからの知識の抽出    
    * [遠隔教師あり学習のためのNYT風コーパスの作成 - baidu_6w](http://pelhans.com/2019/01/04/kg_from_0_note9/)    
    * [ニューラルネットワークによる関係性抽出](https://github.com/thunlp/OpenNRE)    
   
* 知识存储    
    * [D2RQの使用について](http://pelhans.com/2019/02/11/kg_from_0_note10/)    
    * [Jenaの使用について](http://pelhans.com/2019/02/11/kg_from_0_note11/)    
* 知识融合    
    * [Silkの動作](http://pelhans.com/2019/02/12/kg_from_0_note12/)    
* KBQA    
    * [REfOをベースにしたシンプルなKBQA](http://pelhans.com/2018/09/03/kg_from_0_note3/)    
* 语义搜索
    * [elasticsearchをベースとしたシンプルなセマンティック検索 エンティティ検索、エンティティ属性検索、条件検索に対応](http://pelhans.com/2018/09/04/kg_from_0_note4/)

# データを取得する
## 半構造化データ

半構造化データは、scrapyフレームワークを用いて、百度百科とインタラクティブ百科から取得し、現在、映画領域と一般領域の2つのカテゴリに分類している。

* 一般ドメイン百科事典データ：Baidu百科事典4,190,390件、インタラクティブ百科事典3,677,150件。 クローリングの詳細は[ゼロからの知識グラフ構築（VII）百科事典の知識グラフ構築（I）百科事典からの知識抽出](http://pelhans.com/2019/01/04/kg_from_0_note7/)    
* 映画：百度百科は22,219本の映画と13,967人の俳優・女優を収録し、双方向百科は13,866本の映画と5,931人の俳優・女優を収録しています。 プロジェクトの詳細は、[ゼロからのナレッジグラフ構築（I）半構造化データの取得](http://pelhans.com/2018/09/01/kg_from_0_note1/)

## 非構造化データ

非構造化データの主なソースは、WeChat Public、TigerSniff.comニュース、Wikipediaからの非構造化テキストです。

WeChat Publicクローラー
ie/craw/weixin_spider に対応する公開番号で公開された記事のタイトル、投稿時間、公開名、記事内容、記事引用元を取得します。

TigerSniffing.comクローラー
ie/craw/news_spider に対応する TigerSniff.com のニュースのタイトル、簡単な説明、著者、投稿時間、ニュース内容を取得します。

## 構造化されていないテキストからの知識抽出
## Deepdiveに基づく知識抽出

Deepdiveは、スタンフォード大学のInfoLab Labが開発したオープンソースの知識抽出システムです。
弱教師付き学習により、非構造化テキストから構造化された関係データを抽出する。

この実際の戦略はOpenKGに基づいています[中国語の詳細をサポート：スタンフォード大学のオープンソース知識抽出ツール（トリプル抽出）](http://www.openkg.cn/dataset/cn-deepdive)，これに基づいて、映画の領域で俳優と映画の関係を抽出します。

詳しい紹介は[知識グラフをゼロから構築する（V）俳優と映画の関係を抽出するDeepdive](http://pelhans.com/2018/10/10/kg_from_0_note5/)をご覧ください。

## ニューラルネットワークによる関係性抽出

独自の百科事典クラスグラフを活用して、リモートで監視されたデータセットを構築し、OpenNREで実行します。結果のデータセットには、18,226のリレーショナルファクト、336,693のリレーションなし（NA）エンティティペア、354,919の合計エンティティペア、および462のリレーション（NAを含む）が含まれています。

詳しくはこちらをご覧ください[ゼロからの知識グラフの構築（9）百科事典知識グラフの構築（3）データセットの構築とニューラルネットワーク関係抽出の実践](http://pelhans.com/2019/01/04/kg_from_0_note9/)

# RDFへの構造化データ

構造化データのRDF化は、大きく分けて[ダイレクトマッピング](https://www.w3.org/TR/rdb-direct-mapping/)によるアプローチと[R2RML](https://www.w3.org/TR/r2rml/#)によるアプローチの2つがあります。 このように、R2RML言語ベースのアプローチは、より柔軟でカスタマイズが可能です。 R2RMLにはいくつかの優れたツールがありますが、ここではR2RML-KITをベースとしたd2rqツールを使用します。

詳しくは[知識グラフをゼロから構築する(II) データベースからRDF、Jena Accessへ](http://pelhans.com/2019/02/11/kg_from_0_note10/)

![D2RQの例](http://pelhans.com/img/in-post/kg_from_0/d2rq_web_view_lemmas.png)

## ナレッジストレージ
## Neo4j へのデータの預け入れ

グラフデータベースは、グラフ理論に基づいて実装された新しいタイプのNoSQLデータベースである。 そのデータの格納構造やデータの照会方法は、グラフ理論に基づいている。 グラフ理論におけるグラフの要素はノードとエッジであり、これはグラフデータベースにおけるノードとリレーションに相当する。 上記で取得したデータをNeo4jに格納します。

百科事典的グラフについては、[ゼロからの知識グラフ構築⑧百科事典的知識グラフの構築②neo4jへのデータ保存](http://pelhans.com/2019/01/04/kg_from_0_note8/)をご覧ください。   
映画領域については、[ゼロからのナレッジグラフ構築（VI）Neo4jへのデータ格納](http://pelhans.com/2018/11/06/kg_neo4j_cypher/)をご覧ください。

# KBQA
## REfOに基づくシンプルなKBQA
浙江大学がopenKGで提供している[REFO-based KBQA implementation and examples](http://openkg.cn/tool/eb483ee4-3be1-4d4b-974d-970d35307e8d)を参考に、自分の知識グラフに簡単な知識問答システムを実装してみましょう。   

詳しくは、[ゼロから作る知識グラフ(III) REfOを使った簡単な知識クイズ](http://pelhans.com/2018/11/06/kg_neo4j_cypher/)をご覧ください。

### 例
![REfOをベースとしたKBQAの例](http://pelhans.com/img/in-post/kg_from_0/example_REfO_KBQA.png)

## セマンティック検索
## elasticsearch を利用したシンプルなセマンティック検索
このプロジェクトは、ZZUさんの[elasticsearchに基づくKBQAの実装と事例](http://openkg.cn/tool/elasticsearch-kbqa)を簡略化し、独自のデータベースで実装したものです。

詳しくは、[知識グラフをゼロから作る (IV) ESによる簡易セマンティック検索](http://pelhans.com/2018/09/03/kg_from_0_note3/) をご覧ください。

### 例

![elasticsearchによるシンプルなセマンティック検索](http://pelhans.com/img/in-post/kg_from_0/semantic.JPG)
