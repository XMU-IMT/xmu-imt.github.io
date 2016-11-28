---
2	layout:     post
3	title:      "PullRequest小测试"
4	subtitle:   "期待你们的博文！"
5	date:       2016-11-28
6	author:     "连盛 | LancerLian"
7	header-img: "http://img2.imgtn.bdimg.com/it/u=2812104643,902679823&fm=21&gp=0.jpg"
8	tags:
9	    - markdown
10	    - 教程
11	---
12	
13	## 什么是 Markdown
14	
15	Markdown 是一种方便记忆、书写的纯文本标记语言，用户可以使用这些标记符号以最小的输入代价生成极富表现力的文档：譬如您正在阅读的这份文档。它使用简单的符号标记不同的标题，分割不同的段落，**粗体** 或者 *斜体* 某些文字，更棒的是，它还可以
16	
17	### 1. 制作一份待办事宜 [Todo 列表](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#13-待办事宜-todo-列表)
18	
19	- [ ] 支持以 PDF 格式导出文稿
20	- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
21	- [x] 新增 Todo 列表功能
22	- [x] 修复 LaTex 公式渲染问题
23	- [x] 新增 LaTex 公式编号功能
24	
25	### 2. 书写一个质能守恒公式[^LaTeX]
26	
27	$$E=mc^2$$
28	
29	### 3. 高亮一段代码[^code]
30	
31	```python
32	@requires_authorization
33	class SomeClass:
34	    pass
35	
36	if __name__ == '__main__':
37	    # A comment
38	    print 'hello world'
39	```
40	
41	### 4. 高效绘制 [流程图](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#7-流程图)
42	
43	```flow
44	st=>start: Start
45	op=>operation: Your Operation
46	cond=>condition: Yes or No?
47	e=>end
48	
49	st->op->cond
50	cond(yes)->e
51	cond(no)->op
52	```
53	
54	### 5. 高效绘制 [序列图](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#8-序列图)
55	
56	```seq
57	Alice->Bob: Hello Bob, how are you?
58	Note right of Bob: Bob thinks
59	Bob-->Alice: I am good thanks!
60	```
61	
62	### 6. 高效绘制 [甘特图](https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#9-甘特图)
63	
64	```gantt
65	    title 项目开发流程
66	    section 项目确定
67	        需求分析       :a1, 2016-06-22, 3d
68	        可行性报告     :after a1, 5d
69	        概念验证       : 5d
70	    section 项目实施
71	        概要设计      :2016-07-05  , 5d
72	        详细设计      :2016-07-08, 10d
73	        编码          :2016-07-15, 10d
74	        测试          :2016-07-22, 5d
75	    section 发布验收
76	        发布: 2d
77	        验收: 3d
78	```
79	
80	### 7. 绘制表格
81	
82	| 项目        | 价格   |  数量  |
83	| --------   | -----:  | :----:  |
84	| 计算机     | \$1600 |   5     |
85	| 手机        |   \$12   |   12   |
86	| 管线        |    \$1    |  234  |
87	
88	### 8. 更详细语法说明
89	
90	想要查看更详细的语法说明，可以参考我们准备的 [Cmd Markdown 简明语法手册][1]，进阶用户可以参考 [Cmd Markdown 高阶语法手册][2] 了解更多高级功能。
91	
92	总而言之，不同于其它 *所见即所得* 的编辑器：你只需使用键盘专注于书写文本内容，就可以生成印刷级的排版格式，省却在键盘和工具栏之间来回切换，调整内容和格式的麻烦。**Markdown 在流畅的书写和印刷级的阅读体验之间找到了平衡。** 目前它已经成为世界上最大的技术分享网站 GitHub 和 技术问答网站 StackOverFlow 的御用书写格式。
93	
94	---
95	
96	
97	
98	Author [@ghosert][3]     Collected by [Lancer](LancerLian.github.io)
99	2016 年 07月 07日    
100	
101	[^LaTeX]: 支持 **LaTeX** 编辑显示支持，例如：$\sum_{i=1}^n a_i=0$， 访问 [MathJax][4] 参考更多使用方法。
102	
103	[^code]: 代码高亮功能支持包括 Java, Python, JavaScript 在内的，**四十一**种主流编程语言。
104	
105	[1]: https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown
106	[2]: https://www.zybuluo.com/mdeditor?url=https://www.zybuluo.com/static/editor/md-help.markdown#cmd-markdown-高阶语法手册
107	[3]: http://weibo.com/ghosert
108	[4]: http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference