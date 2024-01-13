---


---

<h1 id="task1"><span class="prefix"></span><span class="content">task1</span><span class="suffix"></span></h1>
<ol>
<li>
<p>踩坑记录：<br>
1）. 这里指令是错误的：</p>
<pre><code>python startup.py --idea "write a cli blackjack game"			
Error: No such option: --idea
</code></pre>
<p><strong>因为idea是必要的第一个参数，直接写参数内容就好了，后面的option才需要 --xxx 		<br>
应该是 python <a href="http://startup.py">startup.py</a> “write a cli blackjack game” --code-review --run-tests # 后面两个参数可选</strong></p>
<p>2）. Team中的start_project将被弃用，并且调用会报错，推荐使用run_project代替。<br>
3）. asyncio.run不能调用里面有asyncio.run函数的方法，不然会报错，里面需要异步的话，用await</p>
</li>
<li>
<p>知识点记录<br>
1）. 环境搭建<br>
我这边用的conda创建了一个独立的环境，避免之前的东西对学习agent产生影响，安装号conda后，相关的指令是类似下面这样的</p>
<pre><code>	# 创建独立环境
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
<p>至此，基本环境已经装好，在装的过程中，有碰到一些类似警告的Error，我这边选择了忽略，后续没有发生，报错在下方：<br>
<img src="https://raw.githubusercontent.com/l5276261/MarkDownPic/master/imgs/2024-01-13/j6Pwrk0hl5bS4MLA.png" alt="安装MetaGPT库中遇到的错误"></p>
<p>2）然后就是按照教程试着跑一下MetaGPT<br>
a. 首先由于过程有点繁琐，自己买了api-key，也不想学习的时候时刻挂着科学工具，因此自己弄了个代理（代理相关教学youtube上有，搭代理也免费，不想浪费时间在这，可以时刻开科学工具或者用教程中提供的代理地址： <a href="https://api.openai-forward.com/v1">https://api.openai-forward.com/v1</a><br>
b. 由于我是git clone项目后安装的环境，因此我直接在本地copy了一下config/config.yaml 到 config/key.yaml，然后配置了</p>
<pre><code>OPENAI_API_KEY="xxx"
OPENAI_API_MODEL="gpt-3.5-turbo-1106" # 穷，只买了3.5的key
OPENAI_BASE_URL="xxxx/v1" # 这里值得一提，如果自己搭了代理，或者用教程中的https://api.openai-forward.com/v1，一定不要忘了后面的v1，不然会有问题
</code></pre>
<p>c. 先尝试下对不对，按照教程，在./metagpt目录下，执行</p>
<pre><code>python startup.py --idea "write a cli blackjack game"
</code></pre>
<p>报错：Error: No such option: --idea，原因和解决方案在上面的踩坑记录里，解决完后执行成功，给了一个Project<br>
d. 尝试自己创建一个Team，然后执行</p>
<pre><code># asyncio，用来异步执行脚本的
import asyncio
# metagpt库，因为我们已经pip install -e . ，因此在当前conda环境中，任何目录都能执行from metagpt.xxx import xxx
# 这里我们import 了架构师，工程师，产品经理，项目经理
from metagpt.roles import (
    Architect,
    Engineer,
    ProductManager,
    ProjectManager,
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
<ol>
<li>什么是智能体（Agent）?<br>
<strong>智能体 = LLM + 记忆 + 规划 + 工具 + 神经 + 直觉</strong></li>
</ol>
<blockquote>
<p>LLM：大语言模型</p>
<p>记忆：<br>
1）感觉记忆：模型在训练过程中的文本or图像or其他形式的vec表示。<br>
2）短期记忆：在上下文中学习的知识，这部分是有效但是短线且有限的，因为上下文有窗口长度限制。<br>
3）长期记忆：在查询的过程中可以感知到的外部向量存储，可以通过检索快速访问。</p>
<p>规划：类似人类规划一样的能力，有以下四个点<br>
1）反思：一个框架，为Agent提供动态记忆和自我反思的能力，以提升思维能力。<br>
2）自我批评：允许Agent通过改进过去的决策和纠正之前的错误，来迭代提升能力。<br>
3）思维链条：模型被指导“逐步思考”，以便模型将困难的任务分解成一个一个小的简单的任务去解决。<br>
4）思维树：通过在一个步骤探索多个解决方向的可能性，拓展了思维链条的宽度。</p>
<p>工具：包括各种API，分为build something及use something。比如代码解释器、在线搜索之类的。</p>
<p>神经： 智能体需要像人拥有各种神经模块一样，运用各种小的模型，来完成某个具体的任务，不能由一个大模型来主导，一不经济，二不实际。</p>
<p>直觉：直觉就是LLM的本能，在训练中已经完全会了的能力，类比我们人类的喝水吃饭，或者被人问起1+1等于多少，这些都是不需要额外的启动sys2，直接用sys1就能轻松解决的问题。</p>
</blockquote>
<ol start="2">
<li>智能体如何协作？<br>
多智能体 = 智能体 + 环境 + SOP + 评审 + 路由 + 订阅</li>
</ol>
<blockquote>
<p>环境：一个具体的业务场景，比如一个办公室，或者一个飞书群，微信群。可以让智能体在当中发生交互or协奏。这个环境还包括了智能体之间的相对关系，比如物理距离，或者是上下级关系之类的，这些都属于环境，决定者智能体之间的交互的方式。<br>
SOP：决定了每一个智能体是什么角色，需要怎么怎么去做工作的流转。<br>
评审：决定了每一个智能体的输出要达到怎样的程度。<br>
路由：在什么样的交叉review的效果下，输出能达到接近完美。<br>
订阅：各个智能体之间的工作流，比如产品经理可能会订阅老板，架构师可能会订阅产品经理，程序员可能会订阅策划。<br>
小结：其实智能体之间的协作，就是将人类世界的人与人之间的协作搬到了数字世界，其中的某些环节是否合理，是否可靠，都是动态可调整的。</p>
</blockquote>

