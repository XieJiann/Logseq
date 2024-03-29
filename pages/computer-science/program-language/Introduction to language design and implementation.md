- 从设计一个语言开始，表达式evaluation 语言
	- Tiny Language 0：我们使用Add来构建抽象语法树，避免具体的语义
	  collapsed:: true
	  ![image.png](../assets/image_1668603424226_0.png){:height 147, :width 499}
		- 如果我们想要解释它的话，可以使用模式匹配来递归执行
		  collapsed:: true
		  ![image.png](../assets/image_1668603512443_0.png){:height 135, :width 383}
			- 上述的执行语义可以转为数学符号描述，formalization
				- 定义基本的term：符号，和值范围values
				  ![image.png](../assets/image_1668603567971_0.png){:height 68, :width 523}
				- 对于上述的三条执行规则，我们可以用数学语言描述
				  ![image.png](../assets/image_1668603598358_0.png)
			- 上述的执行过程依赖于一个语言栈，即我们需要保存临时变量
		- Lowering to a stack machine and interpret：引入一个存储栈去执行
			- 定义基本操作
				- 指令
				  ![image.png](../assets/image_1668603924453_0.png)
				- 程序
				- ![image.png](../assets/image_1668603932922_0.png){:height 53, :width 486}
				- 操作数
				  ![image.png](../assets/image_1668603950876_0.png){:height 40, :width 352}
				- 数据栈
				  ![image.png](../assets/image_1668603960302_0.png){:height 43, :width 474}
			- 基于栈的数据操作
			  ![image.png](../assets/image_1668603999064_0.png){:height 215, :width 522}
				- Cst：将数据压入栈顶
				- Add|Mul：取出栈顶的两个元素来操作
			- formalization
				- 压栈出栈的数学符号
				  ![image.png](../assets/image_1668604078599_0.png){:height 107, :width 500}
				- 基本的操作指令和数学符号的映射
				  ![image.png](../assets/image_1668604138573_0.png)
				- formalization
					- 这里我们使用方括号表示编译，即将上述函数翻译为一系列指令，我们得到对语言的编译规则为
					  ![image.png](../assets/image_1668604312038_0.png){:height 152, :width 492}
					- 对于一系列指令，我们编译到最后，使得栈上只有一个值，这个值就是执行结果
						- 栈平衡：在执行后，栈只会包含一个值
					- 编译对应的语义为
					  ![image.png](../assets/image_1668604358664_0.png){:height 76, :width 426}
			-
	- Tiny Language 1：引入一个有名变量
	  collapsed:: true
	  id:: 639b2805-145f-444e-805a-b0fe66998f11
	  ![image.png](../assets/image_1668604479634_0.png){:height 131, :width 485}
		- 这里语言的执行过程和Tiny Language 1类似，不过我们引入了一个环境保存变量
		  ![image.png](../assets/image_1668604519543_0.png){:height 225, :width 627}
			- 这里引入一个三元表达式，语义为 let (x, e1, e2): e2 是包含x的表达式，其中x = e1
		- formalization
			- 符号定义
			   ![image.png](../assets/image_1668604633013_0.png){:height 69, :width 714}
			- 一些标记，取值和赋值
			  ![image.png](../assets/image_1668604663879_0.png)
			- 执行规则
			  ![image.png](../assets/image_1668604684075_0.png)
		- 上述执行过程的问题是，我们需要通过变量名寻址，变量名寻址比较低效可以直接转为一个栈，利用索引寻址
	- Tiny  Language 2：将有名变量转为无名变量
	  collapsed:: true
	  ![image.png](../assets/image_1668604799970_0.png){:height 180, :width 341}
		- 构造一个使用语言栈的evaluation过程
		  ![image.png](../assets/image_1668604832943_0.png){:height 222, :width 580}
			- 该构造过程引入了一个二元函数，let(e1, e2): e1 作为e2 的变量
			- 上述语义同样也可以使用数学公式表达
			  ![image.png](../assets/image_1668686694067_0.png)
				- 其中s表示的是执行环境，即临时变量存储的栈
		- 同我们可以将一个有名的expr翻译为Nameless的expr
		  ![image.png](../assets/image_1668686833431_0.png)
			- 这里将加一个cenv，即编译时的环境，用来存储变量名
			- 这里我们通过index(cenv, x)找到对应变量的索引
		- 同样我们期望将其编译为一系列的指令
			- 为了编译Nameless expr，我们还需引入一些指令
			   ![image.png](../assets/image_1668687327419_0.png)
			- 1如何编译Let 指令
			  $$[\![Let(e1, e2)]\!] = [\![e1]\!];[\![e2]\!];Swap;Pop$$
				- 为了保持栈平衡，我们需要删除$[\![e1]\!]$，所以需要swap 和 pop
				- 例子
					- 例1，注意转为nameless expr的时候，下述x是不需要的
					  ![image.png](../assets/image_1668688023805_0.png)
						- 注：上述Var(1) 是因为上述索引开始永远是栈顶，即栈顶是0
					- 例2
					  ![image.png](../assets/image_1668688043517_0.png)
						- Add(Cst(1), let(x, Cst(2), Add(Var(x), Cst(7)))) =>
			- 如何编译Var 指令
				- 尽管在语言中Var(i) 表示第i个局部变量，但是i并不一定代表栈道地址，如上述例1，所以我们需要将bind index 转变为stack index
				- 这一步是通过引入另一个IR，index.expr 来实现的
				  collapsed:: true
				   ![image.png](../assets/image_1671109599108_0.png)
					- 将nameless.expr => indexd.expr 的核心便是stack_index的计算，编译过程如下
					  ![image.png](../assets/image_1671109664397_0.png)
						- 以上编译过程称为 abstract interpretation，即我们不关心具体的值，只关心变量的性质：是局部变量还是临时变量
						- 我们会模拟整个执行流程，在遇到Var(s) 时，我们将bind index 转为 stack index
							- 即考虑临时变量后的实际物理地址
							- 比如 注意右下角的Var0 -> Var1的过程
							  ![image.png](../../../assets/image_1671112863843_0.png)
						- 在得到stack index后，我们便可以自然的将其转变为machine language
						  ![image.png](../../../assets/image_1671112974922_0.png)
				-
				- 上述的编译过程包括 named.expr => nameless.expr => indexd.expr => machine language，四个语言。当然也可以将它们合并为一个pass
					- 现代编译器更倾向于多个pass，因为每个pass都可以优化，且方便debug，在内存足够的情况下
- 总结，这一节我们学到了
  ![image.png](../assets/image_1668688262034_0.png){:height 190, :width 423}
	- 即三种eval，两种 translate
	    expr => eval
	       |
	  namelss => eval
	       |
	     instr => eval
	- 这里面从栈指令层面翻译name 到 nameless，这里略