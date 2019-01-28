# LtSearch - 轻量级搜索引擎
------
这是一个轻量级搜索引擎，适合中小型网站，仅仅实现快速搜索功能，索引数据量支持500W-1000W，最大勉强支撑3000W数据。

------
### 想法

一开始是需求一个小功能，能够实现搜索一句话，然后进行分词，查找到标题带有这些关键词的文章，反馈搜索信息，没有权重，模糊搜索，同义词功能。如果需要更为复杂的功能，建议使用其他大型搜索引擎。

------
### 算法

使用bitmap算法(不是位图算法)，如果需要学习，请自行查找。学习<Elasticsearch>索引倒序和Roaring中的bitmap索引（另外两个数组索引,增量索引，我没用）来实现核心功能。

------
### 依赖

    * PHP >= 7.1.21
    * SCWS (中文分词工具，通过修改 SplitWords.php来切换分词工具)

------

### 配置

Config.php 主要配置文件

     /**
     * 请修改此次路径到固定位置，尽量不要放到网站目录中。
     */

     const INDEX_DIR = '/tmp/ltSearch/index/'; //索引存放路径
     const WORDS_DIR = '/tmp/ltSearch/words/'; //文档存放路径

     /**
     * 这里是索引采用的存储方式 分别为 32位 和 64位 int类型。如果你的服务器php支持64位可以设置为64.(建议32)
     */
    const INDEX_UNIT_MAX = 32;

------
### 使用方式 

    * 可以参考test目录
    * 搜索前需要先建立索引

引入项目

    require __DIR__ . '/../vendor/autoload.php'; //注意调整所在目录位置

    use SzwSuny\LT\Search\LtSearch;

声明对象

    $ltSearch = new LtSearch();

建立索引

    $ltSearch->add(1,'这里是要被索引的文字');   //第一个参数 1 可以理解为文章ID,搜索结果数组中只会返回这个值。

修改索引

    $result = $ltSearch->update(1,'这里是修改后要索引的文字'); //将ID=1的文档修改成新的文字

删除索引

    $ltSearch->remove(2);   //删除ID=2的文档索引

搜索

    $result = $ltSearch->search('搜索内容');    //搜索结果将会返回符合条件的文档ID数组
