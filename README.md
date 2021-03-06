# ComplexEventExtraction
chinese compound event extraction，中文复合事件抽取，包括条件事件、因果事件、顺承事件、反转事件等事件抽取，并形成事理图谱。

# 关于项目
目前，知识图谱在学术界如火如荼地进行，但受限于知识图谱各个环节中的性能问题，还尚未能够在工业界大规模运用。而与知识图谱中以实体为知识节点，实体关系为实体关系边对知识进行组织不同，事件图谱，又称事理图谱，在目前也是很火的一个研究方向。就事理图谱而言,其从技术实现难度上不亚于知识图谱。  
本人目前在事件图谱上的实验工作有：  
顺承事件图谱(https://github.com/liuhuanyong/SequentialEventExtration)  
因果事件图谱(https://github.com/liuhuanyong/CausalityEventExtraction)  
目前，想到其实中文的事件在显式上的表达上遵循的是中文的复句表现形式。因此，打算对阶段性的工作进行整理，进一步形成中文复合事件抽取项目(https://github.com/liuhuanyong/ComplexEventExtraction)  
接下来，将自己对事理图谱工作的一些理解整理出来，对事件图谱的类型、事件表示的相关方面进行归纳  。

# 1、事件图谱（事理图谱）的类型
| 事件 | 含义 | 形式化 | 事件应用 | 图谱场景 | 举例 |
| --- | --- | --- | --- | --- | --- |
| 因果事件 | 某一事件导致某一事件发生 | A导致B  | 事件预警  |因果溯源 由因求果  | <地震,房屋倒塌> |
| 条件事件 | 某事件条件下另一事件发生 | 如果A那么B  |事件预警  |时机判定  | <限制放宽,立即增产> |
| 反转事件 | 某事件与另一事件形成对立 | 虽然A但是B  |预防不测  |反面教材  | <起步晚,发展快>|
| 顺承事件 | 某事件紧接着另一事件发生 |  A接着B  |事件演化  |未来意图识别  | <去旅游,买火车票>  |

# 2、事件的表示
以因果事件为例：
已知句子：这几天非洲闹猪瘟，导致国内猪肉涨价

|表示形式 | 含义 | 举例 | 优点 | 缺点 |
| --- | --- | --- | --- | --- |
| 短句 | 以中文标点符号为分割边界形成的短句 | 这几天非洲闹猪瘟&国内猪肉涨价  | 方便、最原始信息  |噪声多，不易融合|
| 词序列 | 对短句进行分词、词性标注、停用词形成的词序列 | 非洲闹猪瘟&国内猪肉涨价  |语义丰富、较短句形式短  |停用规则不易控制  |
| 短语 | 依存句法分析/语义角色标注，形成主谓短语、动宾短语、主谓宾短语 | 非洲闹猪瘟&猪肉涨价  |语义凝固简洁  |受限于依存、语义角色性能  |

# 关于项目结构
本项目列举了汉语句子表顺承、条件、并列、转折的关联词，详见complex_sentence.py,例如：

    '''转折事件'''
    def pattern_but(self):
        wds = [[['与其'], ['不如'],'but'],
                [['虽然','尽管','虽'],['但也','但还','但却','但'],'but'],
                [['虽然','尽管','虽'],[ '但','但是也','但是还','但是却',],'but'],
                [['不是'],['而是'],'but'],
                [['即使','就算是'],['也','还'],'but'],
                [['即便'],['也','还'],'but'],
                [['虽然','即使'],['但是','可是','然而','仍然','还是','也', '但'],'but'],
                [['虽然','尽管','固然'],['也','还','却'],'but'],
                [['与其','宁可'],['决不','也不','也要'],'but'],
                [['与其','宁肯'],['决不','也要','也不'],'but'],
                [['与其','宁愿'],['也不','决不','也要'],'but'],
                [['虽然','尽管','固然'],['也','还','却'],'but'],
                [['不管','不论','无论','即使'],['都', '也', '总', '始终', '一直'],'but'],
                [['虽'],['可是','倒','但','可','却','还是','但是'],'but'],
                [['虽然','纵然','即使'],['倒','还是','但是','但','可是','可','却'],'but'],
                [['虽说'],['还是','但','但是','可是','可','却'],'but'],
                [['无论'],['都','也','还','仍然','总','始终','一直'],'but'],
                [['与其'],['宁可','不如','宁肯','宁愿'],'but']]

# 实验结果
本项目基于1000W资讯进行实验，共得到古复合中文事件模式237条，top10的模式结果为：

        模式         频次
        but_虽然_但	1484690
        but_尽管_但	1006669
        condition_如果_就	763451
        more_或_或	716354
        more_也_还	675549
        condition_如果_那么	494417
        more_不仅_也	483610
        condition_只有_才	432495
        more_不仅_还	429681
        condition_无论_都	399225

# 应用结果

# 1、事件举例

| 事件类型 | 事件1 | 事件2 |
| --- | --- | --- |
| 反转事件 | 不是	太多	而是	太少 |虽然	小幅提涨	但是	成交不多|
| 反转事件 | 不是	在消费	而是	在社交 | 虽然	幅度不算大	但是	形态收好 |
| 反转事件 | 不是	多了	而是	少了| 虽然	缓慢	但是	步伐坚定 |
| 反转事件 | 不是	目的	而是	手段 | 虽然	觉得有点坑	但是	毫无办法|
| 反转事件 | 不是	太多	而是	太少 | 虽然	速缓	但是	质更优 |
| 反转事件 | 不是	封闭的	而是	开放包容的 | 虽然	起步稍晚	但是	热度不减 |
| 反转事件 | 不是	一个结果	而是	一种逻辑 | 虽然	压力比较大	但是	努力过 |
| 反转事件 | 不是	周期性的	而是	结构性的 | 虽然	没有功劳	但是	我也有苦劳 |
| 条件事件 | 一旦	时机成熟	就	坚决推行 |如果	数据疲软	那么	将打压瑞郎  |
| 条件事件 | 一旦	触发	就	不可逆了 | 如果	美元涨	那么	黄金应该跌 |
| 条件事件 | 一旦	形成	就	很难改变 | 如果	惯性下跌	那么	请及时平仓 |
| 条件事件 | 一旦	产生恐慌	就	会手忙脚乱 |如果	英国退欧	那么	金价将上涨  |
| 条件事件 | 一旦	制定了目标	就	必须完成 |如果	比值上升	那么	进口将盈利  |
| 条件事件 | 一旦	停产	就	失去了份额 | 如果	是趋势	那么	就顺势操作 |
| 条件事件 | 一旦	超调贬值	就	会失控 | 如果	看跌	那么	赶紧跑 |

# 2、图谱展示

1、反转事件图谱
![image](https://github.com/liuhuanyong/ComplexEventExtraction/blob/master/img/but.png)

2、条件事件图谱
![image](https://github.com/liuhuanyong/ComplexEventExtraction/blob/master/img/condition.png)


# 总结
1、本项目对事件图谱的类型、表现形式进行了归纳，并结合复合事件模式与语料进行了实验。  
2、实验表明，反转事件，其实在某种程度上可以用来构造反义词词典，例如"不是A而是B"这种模式，可以得到很多反义的词或短语，这让我想到了我的一个反义词项目接口：(https://github.com/liuhuanyong/ChineseAntiword) ,我们可以用wordvector找相近词，可以靠这种方式收集反义词，对了，还可以加上情绪。  
3、实验表明，汉语显示标记其实在中文文本当中还是用的很普遍的，我统计了以下，跑了1000W文本，有超过半数的文本中包含以上模式。因此，如果能够把显示事件图谱做好，感觉用处还是很多的。  
4、本项目还有很多不足，比如模式上，比如对事件类型和事件表示的看法上，欢迎补充。  
5、If any question about the project or me ,see https://liuhuanyong.github.io/  
