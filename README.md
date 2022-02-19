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
希望在该图谱上尝试应用以下技术：    

* [百度百科与互动百科的知识抽取](http://pelhans.com/2019/01/04/kg_from_0_note7/)    
   * 半结构化数据    
      * 百度百科爬虫   
      * 互动百科爬虫    
   * 非结构化数据
      * 微信公众号爬虫  
      * 虎嗅网爬虫    
* 非结构化文本的知识抽取    
    * [制作类似于NYT的远程监督学习语料--baidu_6w](http://pelhans.com/2019/01/04/kg_from_0_note9/)    
    * [神经网络关系抽取](https://github.com/thunlp/OpenNRE)    
   
* 知识存储    
    * [D2RQ 的使用](http://pelhans.com/2019/02/11/kg_from_0_note10/)    
    * [Jena 的使用](http://pelhans.com/2019/02/11/kg_from_0_note11/)    
* 知识融合    
    * [Silk 实战](http://pelhans.com/2019/02/12/kg_from_0_note12/)    
* KBQA    
    * [基于 REfO 的简单KBQA](http://pelhans.com/2018/09/03/kg_from_0_note3/)    
* 语义搜索
    * [基于elasticsearch 的简单语义搜索 支持实体检索、实体属性检索和条件检索](http://pelhans.com/2018/09/04/kg_from_0_note4/)

# 获取数据
## 半结构化数据

半结构化数据从百度百科和互动百科获取，采用scrapy框架，目前电影领域和通用领域两类。

* 通用领域百科数据：百度百科词条4,190,390条，互动百科词条3,677,150条。爬取细节请见[从零开始构建知识图谱（七）百科知识图谱构建（一）百度百科的知识抽取](http://pelhans.com/2019/01/04/kg_from_0_note7/)    
* 电影领域: 百度百科包含电影22219部，演员13967人，互动百科包含电影13866部，演员5931 人。项目详细介绍请见[从零开始构建知识图谱（一）半结构化数据的获取](http://pelhans.com/2018/09/01/kg_from_0_note1/)

## 非结构化数据

非结构化数据主要来源为微信公众号、虎嗅网新闻和百科内的非结构化文本。

微信公众号爬虫获取公众号发布文章的标题、发布时间、公众号名字、文章内容、文章引用来源，对应 ie/craw/weixin_spider。虎嗅网爬虫 获取虎嗅网新闻的标题、简述、作者、发布时间、新闻内容，对应 ie/craw/news_spider。

# 非结构化文本的知识抽取
## 基于Deepdive的知识抽取    

Deepdive是由斯坦福大学InfoLab实验室开发的一个开源知识抽取系统。它通过弱监督学习，从非结构化的文本中抽取结构化的关系数
据 。本次实战基于OpenKG上的[支持中文的deepdive：斯坦福大学的开源知识抽取工具（三元组抽取）](http://www.openkg.cn/    dataset/cn-deepdive)，我们基于此，抽取电影领域的演员-电影关系。

详细介绍请见[从零开始构建知识图谱（五）Deepdive抽取演员-电影间关系](http://pelhans.com/2018/10/10/kg_from_0_note5/)

## 神经网络关系抽取

利用自己的百科类图谱，构建远程监督数据集，并在OpenNRE上运行。最终生成的数据集包含关系事实18226，无关系(NA)实体对336 693，总计实体对354 919，用到了462个关系(包含NA)。

详细介绍请见[从零开始构建知识图谱（九）百科知识图谱构建（三）神经网络关系抽取的数据集构建与实践](http://pelhans.com/2019/01/04/kg_from_0_note9/)

# 结构化数据到 RDF

结构化数据到RDF由两种主要方式，一个是通过[direct mapping](https://www.w3.org/TR/rdb-direct-mapping/)，另一个通过[R2RML](https://www.w3.org/TR/r2rml/#acknowledgements)语言这种，基于R2RML语言的方式更为灵活，定制性强。对于R2RML有一些好用的工具，此处我们使用d2rq工具，它基于R2RML-KIT。

详细介绍请见[从零开始构建知识图谱（二）数据库到 RDF及 Jena的访问](http://pelhans.com/2019/02/11/kg_from_0_note10/)

![D2RQ 示例](http://pelhans.com/img/in-post/kg_from_0/d2rq_web_view_lemmas.png)

# 知识存储
## 将数据存入 Neo4j

图数据库是基于图论实现的一种新型NoSQL数据库。它的数据数据存储结构和数据的查询方式都是以图论为基础的。图论中图的节本元素为节点和边，对应于图数据库中的节点和关系。我们将上面获得的数据存到 Neo4j中。

百科类图谱请见：[从零开始构建知识图谱（八）百科知识图谱构建（二）将数据存进neo4j](http://pelhans.com/2019/01/04/kg_from_0_note8/)    
电影领域的请见[从零开始构建知识图谱（六）将数据存进Neo4j](http://pelhans.com/2018/11/06/kg_neo4j_cypher/)

# KBQA
## 基于 REfO 的简单KBQA
基于浙江大学在openKG上提供的 [基于 REfO 的 KBQA 实现及示例](http://openkg.cn/tool/eb483ee4-3be1-4d4b-974d-970d35307e8d),在自己的知识图谱上实现简单的知识问答系统。    

详细介绍请见[从零开始构建知识图谱（三）基于REfO的简单知识问答](http://pelhans.com/2018/11/06/kg_neo4j_cypher/)

### 示例
![基于REfO 的 KBQA 示例](http://pelhans.com/img/in-post/kg_from_0/example_REfO_KBQA.png)

# 语义搜索
## 基于elasticsearch 的简单语义搜索
本项目是对浙大的[ 基于elasticsearch的KBQA实现及示例 ](http://openkg.cn/tool/elasticsearch-kbqa)的简化版本，并在自己的数据库上做了实现。

详细介绍请见[从零开始构建知识图谱(四)基于ES的简单语义搜索](http://pelhans.com/2018/09/03/kg_from_0_note3/)

### 示例

![基于elasticsearch的简单语义搜索](http://pelhans.com/img/in-post/kg_from_0/semantic.JPG)
