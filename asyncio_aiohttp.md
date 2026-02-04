
import asyncio
import aiohttp

from aiohttp import web

"""
python 的asyncio库提供完整的一步IO框架，适用于网络请求，文件操作，爬虫，api服务等
1、async  await 关键字
async用于定义协程函数，而await用于挂起当前协程，等待另一个协程完成，只有在协程内部才能使用await
async def fetch_data():
    print("开始获取数据......")
    await asyncio.sleep(2)
    print("数据获取完成......")
    return {"status":"success","data":200}

asyncio.run(fetch_data())
"""

"""
并发执行多个任务：
使用asyncio.gather()可以并发运行多个协程，并等待所有结果返回
异步编程适合IO密集型任务，不建议用于cpu密集型计算，合理使用任务调度极大提升吞吐量
async def task(name,delay):
    print(f'任务{name}开始')
    await asyncio.sleep(delay)
    print(f'任务{name}完成')
    return {"status":"success","data":200}

async def main():
    results = await asyncio.gather(
        task("A",1),
        task("B",2),
        task("C",1)
    )
    return results
asyncio.run(main())
"""

"""
 事件循环原理与任务调度机制
  JavaScript是单线程，以来事件循环 event loop 实现异步非阻塞操作，主线程执行栈中的同步任务完成之后，事件循环会从任务队列取出回调函数执行
  
  宏任务与微任务
  事件循环区分宏任务 macro task 和微任务 micro task, 每次宏任务执行完毕后，系统会清空当前微任务队列
  宏任务：setTimeout setInterval  IO  UI渲染
  微任务：Promise.then  MutationObserver   queueMicroTask
  
  https://developer.aliyun.com/article/1617759
  1、事件循环 是asyncio的核心概念，负责管理和调度异步任务的执行，通过事件循环，可以调度任务并处理任务的完成或等待状态
  async def main():
    task = asyncio.create_task(example_coroutine())
    result = await task
    print(result)
asyncio.run(main())
  2、异步任务
  异步任务可以是asyncio中的协程函数。  asyncio.create_task()   asyncio.ensure_future()
"""

"""
aiohttp 是一个基于 asyncio的http 客户端/服务端框架 用于构建异步的http客户端和服务端，提供简单易用api 使得编写高性能，可扩展的web应用和处理
异步http请求变得更方便
使用aiohttp构建http客户端
"""
''' 发送get请求'''
"""
async def fetch_data(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def main():
    url = "https://jsonplaceholder.typicode.com/posts/1"
    result = await fetch_data(url)
    print(result)
asyncio.run(main())
"""

'''发送post请求'''
"""
async def send_data(url, data):
    async with aiohttp.ClientSession() as session:
        async with session.post(url, json=data) as response:
            return await response.text()
async def main():
    url = "https://jsonplaceholder.typicode.com/posts"
    data = {'title': 'Example', 'body': 'Content'}
    result = await send_data(url, data)
    print(result)
asyncio.run(main())
"""

'''构建http服务器'''
"""
async def handle(request):
    return web.Response(text="Hello, aiohttp!")


app = web.Application()
app.router.add_get('/', handle)

if __name__ == '__main__':
    web.run_app(app)
"""
"""  添加路由和视图
async def hello(request):
    return web.Response(text="Hello, World!")
    
async def greet(request):
    name = request.match_info.get('name', 'Anonymous')
    return web.Response(text=f"Hello, {name}!")
    
app = web.Application()
app.router.add_get('/', hello)
app.router.add_get('/greet/{name}', greet)

if __name__ == '__main__':
    web.run_app(app)
"""
