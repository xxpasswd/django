### 对django源码的一些注释

#### 本人为什么要阅读django源码

1. 加深对django的理解，其实不仅仅是对django的理解
2. 学习源码中的一些优秀设计思想
3. 学习大神写代码的技巧

#### 思考一下，django框架是如何构建起来的？

1. 可能这个问题有点大，一下就把大家吓住了，其实我们可以先从小的地方开始，阅读一下代码量比较小的bottle框架的源码，对web框架有了一个简单认识后，
再来阅读django会简单很多

2. 如果自己要构建一个web框架，需要考虑什么？这个就需要我们对web框架有个基本的认识，比如web框架由哪些东西组成，它的流程又是怎么样的

3. 有了对web框架的认识，我们就有主线了，阅读源码就会事半功倍

#### django源码的各个目录

1. apps: django的apps
2. conf: 配置模块
3. contrib: 常用的功能模块
4. core: 一些核心模块
5. db: 数据库相关
6. dispatch: 调度模块
7. forms: 表单相关
8. http: 请求和响应相关
9. middleware: 中间件相关
10. template, templatetags: 模板相关
11. urls: 路由相关
12. utils: 实用功能模块
13. views: 视图相关

#### web框架有哪些基本组件？

1. 路由
2. 视图
3. 模板

#### 从哪儿开始阅读

每个web框架的实现或许各有不同，但是主要流程是大体相同的。所以从请求到响应的这个流程的顺序来阅读源码，我觉得是最合适的顺序


#### WSGIHandler（django/core/handlers/wsgi.py) 请求入口

django传递给wsgi服务器的application就是WSGIHandler

一个请求的处理流程：

请求前的中间件---->视图中间件---->视图函数---->请求后的中间件

用函数的方式形象一下：

```
def process_request(request)
    try:
        # 请求前的中间件
        response = pre_middleware(request)
        if response:
            return response
        
        # 视图中间件
        response = view_middleware(request, view_fun)
        
        if not response:
            try:
                # 执行视图函数
                reponses = view_fun(request)
            except:
                # 异常中间件
                responser = exception_middleware()
        
        if hasattr(response, 'render'):
            # 模板中间件
            response = template_middleware(response)
            try:
                response = response.render()
            except:
                response = exception_middleware()
        
        # 请求后的中间件
        response = post_middleware(request, response)
            
    except:
        response = exception()
        
    return response    
```

主体流程通了，接下来就是各种运行过程中用到的模块了

#### 已阅读索引

1. autoreload 机制  django\utils\autoreload.py



