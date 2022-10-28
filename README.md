# zwc
version 1.2

#### 1. 文本过滤器<br>
	(1) select
	(2) further select
#### 2. QA模型
	(1) ernie-3.0-xbase
	(2) 预测答案时不考虑question和[CLS]
#### 3. 证据抽取方法
	(1)attention-based interpretation得到文本中每个词的重要度分数
	(2)依据文本的长度context_len和RATIONALE_RATIO抽取分数靠前context_len*RATIONALE_RATIO个词（TopK words）
	(3)对TopK words进行过滤，只保留文字和句号，其他标点符号删除，剩余的TopK words 即为TopK’ words
	(4)对这TopK’ words的重要度分数进行归一化，使他们的重要度分数和为1
	(5)对于每个TopK’ words倒排索引其所在的短句，将当前的TopK’ words的分数赋予其所在的短句，最后就可以得到每个短句的得分，所有的短句总和仍为1
	(6)根据短句的重要度分数选取证据
		① 设置一个sents用于存储证据
		② 将所有短句中重要度分数最高且不在sents中的短句纳入sents
		③ 若当前所选短句sents的分数之和超过既定的阈值 L 且 所选短句长度之和超过context_len*RATIONALE_RATIO，则将当前所选的短句作为最终的证据
