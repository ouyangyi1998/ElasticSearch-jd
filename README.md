Fork From https://github.com/ID-WangQiang/ElasticSearch-jd

### 项目介绍
学习ElasticSearch的入门实践，京东商城搜索风格。

### 技术工具
项目采用SpringBoot构建。前端采用Thymeleaf模板引擎+Vue渲染页面数据,使用axios解决前后端跨越问题。  
涉及到的其他技术工具有ElasticSearch，Jsoup，lombok

### 开发环境
Mac  IDEA  JDK8   
ElasticSearch 7.6.2    
ElasticSearch-analysis-ik-7.6.2 

### 如何运行
1.git clone git@github.com:ID-WangQiang/ElasticSearch-jd.git   
2.打开ElasticSearch，使用ElasticHD查看ES数据   
3.用IDEA打开项目，运行ElasticsearchJdApplication文件   
4.http://localhost:9090 测试项目ES搜索功能

### 开发要点
1. es的默认分词器为standard，不支持中文分词，需要配置ik分词器，同时进行数字+中文或者中文+英文的分词存在问题
2. 一段时候搜索次数太多，疑似会引发jd的防爬虫机制，会出现搜索结果不完整的情况
3. 爬虫机制通过构建url，获取到jd的搜索结果html（jd没有要求强制登录)通过构造jsoup的element来获取到商品的信息，然后把它们放入容器
4. 先通过构造bulkrequest，后面利用循环把indexrequest每一个分装好的商品装配上,最后通过restHighLevelClient把商品集中注入es

### 搜索机制
1. 先配置searchrequest，同时指定文档仓库 ”jd_goods"
2. 在配置searchsourcebuilder（建造者模式），这里配置分页和匹配机制，然后注入searchrequest
3. 最后解析结果，构造arraylist<map<string,object>>
