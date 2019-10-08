title: laravel 利用redis 的发布订阅保持数据一致性
date: 2019/01/20
categories:
- web
- redis
tags:
- 发布订阅
comments: true
---

#### 保持数据一致性
项目服务之间共用配置信息可以存储在远程的redis，怎样保存数据的一致性是个问题。可以使用redis 的发布订阅功能及时同步数据，思路
1. 数据有变化的服务更新远程redis，并publish key 值到频道
2. 订阅了数据的服务就会收到信息并去同步数据

可以把更新操作加到任务队列，这样就可以把业务和redis 处理分开。用laravel 这样实现
```php
# controller 
public function updateConfig(Request $request)
{
    // 其他逻辑...

    // 分发任务到队列
    cacheJob::dispatch('setCacheData', $request->all());
}

# 命令创建任务 php artisan make:job cacheJob
class cacheJob implements ShouldQueue{
    public function __construct($callback, $arguments)
    {
        $this->callback =$callback;
        $this->arguments =$arguments;
    }

    public function handle()
    {
       call_user_func($this->callback, $this->arguments);
    }

    # setCacheData
    public function setCacheData($params)
    {
        // set....
        $value = $params['data'];
        $this->redis = $this->getRedis();
        $this->redis->set('dataKey', $value);
        // publish 比如频道名test
        $this->redis->publish('test', 'dataKey');        
    } 
}
```
接下来再启动php artisan queue:work 去处理队列里面的任务。可以用supervisor 管理该进程，设置随系统启动，启动的进程个数。

那些订阅远程redis test 频道的客户端就会收到同步信息，可以在需要订阅数据的服务所在机器上简单建一个订阅服务，一旦收到信息就同步。如下图

![subscribe1](/images/20190120/subscribe1.png)

subscribe/publish 在同一台机器上，可以是不相关的两个服务。在subscribe 利用laravel构建自定义命令，**别忘记在Console/Kernel.php 添加命令类**。同样利用surpvisor 管理订阅进程，这样就可以保持数据一致性了！
```php
# php artisan make command subscribeTest
class subscribeTest extends Command{
    public function handle()
    {
        $this->redis = $this->getRedis();
        $this->redis->subscribe(['test'], function ($message) {
            // 获取数据....
            $this->getData($message);
        });
   }
}
```


欢迎大家给我留言，提建议，指出错误，一起讨论学习技术的感受！