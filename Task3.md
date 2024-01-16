---


---

<h1 id="踩坑记录"><span class="prefix"></span><span class="content">踩坑记录</span><span class="suffix"></span></h1>
<ol>
<li>
<p>init的传参从普通的参数变成了**kwargs</p>
</li>
<li>
<p>Role的成员变量_rc变成了rc</p>
</li>
<li>
<p>在创建TutorialAssistant时，并没有向文档说的那样，“如果action是Action类的实例，会检查是否设置为人工操作（is_human）。如果是，则会发出警告，并创建一个新的Action实例并将其赋值给变量i”，而是直接用action赋值给了i</p>
</li>
<li>
<p>Acion里没有成员变量language，需要在类里声明： language:str = “”</p>
</li>
<li>
<p>Role的_setting里也没有is_human变量，存储在Role对象里，直接self.is_human即可</p>
</li>
<li>
<p>Action的set_prefix也不需要第二个参数profile了，只用Prefix</p>
</li>
<li>
<p>Role的成员变量_actions变成了actions</p>
</li>
<li>
<p>Role的成员变量_states变成了states</p>
</li>
<li>
<p>在Role中，也需要将 topic, main_title, total_content, languae声明一遍，不然在__init__中直接赋值会报错</p>
</li>
<li>
<p>RoleReactMoRoleReactMode.REACTde.REACT改成RoleReactMode.REACT</p>
</li>
</ol>
<h1 id="baseline代码"><span class="prefix"></span><span class="content">BaseLine代码</span><span class="suffix"></span></h1>
<pre><code>
import asyncio

from metagpt.actions import Action

from metagpt.roles import Role

from metagpt.roles.role import RoleReactMode

from metagpt.schema import Message

from metagpt.logs import logger

import random

  

class SimplePrint(Action):

randomValue: str=""

  

def __init__(self, name="SimplePrint", randomValue: str="", *args, **kwargs):

super().__init__(name=name, *args, **kwargs)

self.randomValue = randomValue

  

async def run(self):

# 在Windows环境下，result可能无法正确返回生成结果，在windows中在终端中输入python3可能会导致打开微软商店

print(self.randomValue)

# 采用下面的可选代码来替换上面的代码

# result = subprocess.run(["python", "-c", code_text], capture_output=True, text=True)

# import sys

# result = subprocess.run([sys.executable, "-c", code_text], capture_output=True, text=True)

logger.info(f"echo {self.randomValue}")

return f"echo {self.randomValue}"

  

class DispatchTask(Action):

runCnt:int = 2

  

def __init__(self, name="DispatchTask", runCnt:int=2, *args, **kwargs):

super().__init__(name=name, *args, **kwargs)

self.runCnt = runCnt

  

async def run(self):

for i in range(self.runCnt):

pass

return f"run {self.runCnt} times!"

  

class SimplePrintRole(Role):

times:int = 0

def __init__(

self,

name: str = "Stitch",

profile: str = "Print Tool Man",

goal: str = "Print a random value",

):

super().__init__(name=name, profile=profile, goal=goal)

self._init_actions([SimplePrint(randomValue=f"my number is 1"), SimplePrint(randomValue=f"my number is 2"), SimplePrint(randomValue=f"my number is 3")])

  

def _init_actions(self, actions):

self._reset()

for idx, action in enumerate(actions):

if not isinstance(action, Action):

i = action("", llm=self._llm)

else:

i = action

i.set_prefix(self._get_prefix())

self.actions.append(i)

self.states.append(f"{idx}. {action}")

async def run(self, times:int):

self.times = times

rsp = await self.react()

# Publish the reply to the environment, waiting for the next subscriber to process

self.publish_message(rsp)

return rsp

def recv(self, message: Message) -&gt; None:

"""add message to history."""

# self._history += f"\n{message}"

# self._context = self._history

if message in self.rc.memory.get():

return

self.rc.memory.add(message)

def _set_state(self, state: int):

"""Update the current state."""

self.rc.state = state

logger.debug(self.actions)

self.rc.todo = self.actions[self.rc.state] if state &gt;= 0 else None

async def _react(self) -&gt; Message:

while True:

await self._think()

if self.rc.todo is None:

break

msg = await self._act()

return msg

async def _think(self) -&gt; None:

"""Determine the next action to be taken by the role."""

if self.rc.todo is None:

self._set_state(0)

return

  

if self.rc.state + 1 &lt; len(self.states):

self._set_state(self.rc.state + 1)

elif self.times &gt; 1:

self._init_actions([SimplePrint(randomValue=f"my number is 4"), SimplePrint(randomValue=f"my number is 5"), SimplePrint(randomValue=f"my number is 6")])

self._set_state(0)

self.times -= 1

else:

self.rc.todo = None

  

async def _act(self) -&gt; Message:

"""Perform an action as determined by the role.

  

Returns:

A message containing the result of the action.

"""

todo = self.rc.todo

resp = await todo.run()

logger.info(resp)

return Message(content=resp, role=self.profile)

  

async def main():

role = SimplePrintRole()

await role.run(2)

  

asyncio.run(main())

</code></pre>

