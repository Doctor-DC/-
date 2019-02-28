### window启动
1. 先启动django    
`python manage.py runserver`  

1. 启动celery工作服务器(s2项目，celery4)  
`celery -A s2 worker -l info -P eventlet`  
 app中tasks.py与celery服务器关联，debug需要重启 

1. 启动celery监控  
`celery flower -A s2`  

1. flower  地址[http://localhost:5555
](http://localhost:5555)   

> celery 4.x暂时不支持windows平台，如果为了调试目的的话，可以通过替换celery的线程池eventle实现以达到在windows平台上运行的目的:





### 异常：
```
/anaconda/anaconda3/lib/python3.6/site-packages/celery/backends/amqp.py:67: CPendingDeprecationWarning: 
    The AMQP result backend is scheduled for deprecation in     version 4.0 and removal in version v5.0.     Please use RPC backend or a persistent backend.
  alternative='Please use RPC backend or a persistent backend.')

```
原因解析:
在celery 4.0中 rabbitmq 配置result_backbend方式变了:
以前是跟broker一样:
result_backend = 'amqp://guest:guest@localhost:5672//'
现在对应的是rpc配置:
result_backend = 'rpc://'

参考链接:
http://docs.celeryproject.org/en/latest/userguide/configuration.html#std:setting-event_queue_prefix

============


