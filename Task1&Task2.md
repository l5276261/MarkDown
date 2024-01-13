---


---

<h1 id="task1"><span class="prefix"></span><span class="content">task1</span><span class="suffix"></span></h1>
<ol>
<li>
<p>踩坑记录：<br>
1）. 这里指令是错误的：</p>
<pre><code># task1

 1. 踩坑记录：
	1）. 这里指令是错误的： 
	```
	python startup.py --idea "write a cli blackjack game"			
	Error: No such option: --idea
</code></pre>
<p><strong>	```
	**因为idea是必要的第一个参数，直接写参数内容就好了，后面的option才需要 --xxx 		<br>
	应该是 python <a href="http://startup.py">startup.py</a> “startup.py "write a cli blackjack game”" --code-review --run-tests # 后面两个参数可选</strong><br>
**
	2）. Team中的start_project将被弃用，并且调用会报错，推荐使用run_project代替。<br>
	3）. asyncio.run不能调用里面有asyncio.run函数的方法，不然会报错，里面需要异步的话，用await</p>
</li>
<li>
<p>知识点记录<br>


 2. 知识点记录
 1）. 环境搭建<br>

 		我这边用的conda创建了一个独立的环境，避免之前的东西对学习agent产生影响，安装号conda后，相关的指令是类似下面这样的</p>
<pre><code>
	```
		# 创建独立环境
        conda create --name LearnMetaGPT python=3.10
        # 切换到LearnMetaGPT环境 		
        conda activate LearnMetaGPT 		
        # clone 项目 		
        git clone https://github.com/geekan/MetaGPT.git 		
        # 切换到项目目录下 		
        cd yourPath/ 		
        # 安装MetaGPT库到python中 		
        pip install -e .
</code></pre>
<p>	```
	   至此，基本环境已经装好，在装的过程中，有碰到一些类似警告的Error，我这边选择了忽略，后续没有发生，报错在下方：<br>
<img src="
![安装MetaGPT库中遇到的错误](https://raw.githubusercontent.com/l5276261/MarkDownPic/master/imgs/2024-01-13/j6Pwrk0hl5bS4MLA.png" alt="安装MetaGPT库中遇到的错误"></p>
<p>)
	
	2）然后就是按照教程试着跑一下MetaGPT<br>

		a. 首先由于过程有点繁琐，自己买了api-key，也不想学习的时候时刻挂着科学工具，因此自己弄了个代理（代理相关教学youtube上有，搭代理也免费，不想浪费时间在这，可以时刻开科学工具或者用教程中提供的代理地址： <a href="https://api.openai-forward.com/v1">https://api.openai-forward.com/v1</a><br>

		b. 由于我是git clone项目后安装的环境，因此我直接在本地copy了一下config/config.yaml 到 config/key.yaml，然后配置了</p>
<pre><code>
	```
	OPENAI_API_KEY="xxx"
	OPENAI_API_MODEL="gpt-3.5-turbo-1106" # 穷，只买了3.5的key
	OPENAI_BASE_URL="xxxx/v1" # 这里值得一提，如果自己搭了代理，或者用教程中的https://api.openai-forward.com/v1，一定不要忘了后面的v1，不然会有问题
</code></pre>
<p>	```    
       c. 先尝试下对不对，按照教程，在./metagpt目录下，执行</p>
<pre><code>
       ```
	python startup.py --idea "write a cli blackjack game"
</code></pre>
<p>	```
       
       报错：Error: No such option: --idea，原因和解决方案在上面的踩坑记录里，解决完后执行成功，给了一个Project<br>

		d. 尝试自己创建一个Team，然后执行</p>
<pre><code>
		

		# asyncio，用来异步执行脚本的
		import asyncio
		# metagpt库，因为我们已经pip install -e . ，因此在当前conda环境中，任何目录都能执行from metagpt.xxx import xxx
		# 这里我们import 了架构师，引擎工程师，产品经理，项目经理
		from metagpt.roles import (
		    Architect,
		    Engineer,
		    ProductManager,
		    ProjectManager,
)
		)
		# import一个Team类，用这个类来构建自己的团队
		from metagpt.team import Team

		# 定义一个异步启动函数
		async def startup(idea: str):
		    company = Team()
		    company.hire(
		        [
		            ProductManager(),
		            Architect(),
		            ProjectManager(),
		            Engineer(),
		        ]
		    )

		    company.invest(investment=20.0)
		    company.run_project(idea=idea)

		    await company.run(n_round=5)

		# 执行这个函数
		if __name__ == "__main__":
			asyncio.run(startup(idea="write a cli blackjack game"))
</code></pre>
</li>
</ol>
<hr>
<h1 id="task2"><span class="prefix"></span><span class="content">Task2</span><span class="suffix"></span></h1>
<ul>
<li>---------------------------

# Task2
- 0:01 🌍大圆模型的发展路径</li>
<li>  
- 9:09 💡BERT的有效性和训练成本</li>
<li>  
- 18:23 🎮强化学习在模型推理中的应用</li>
<li>  
- 27:33 🗣️直觉、语音交互和多智能体协作</li>
<li>  
- 36:46 🔍智能体的需求和发展前景</li>
<li>  
- 45:55 🤖MetaGPT：AI智能体框架</li>
<li>  
- 55:11 📊MMU榜单、GPT-4性能和商业机会</li>
<li>  
- 64:24 🌟多智能体框架的愿景和影响</li>
<li>  
- 73:34 💼创业公司和MetaGPT软件公司实例</li>
<li>  
- 82:47 🔧智能体的功能和设计理念</li>
<li>  
- 91:59 🔬Hugging GPT的工作原理和开源竞争</li>
</ul>

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTI0NDc5MDg5N119
-->