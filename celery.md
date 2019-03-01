# django-celery-tutorial
![image](https://images2015.cnblogs.com/blog/870989/201607/870989-20160702162151906-1122811333.png)
> pip install celery==3.1.25  
 
## 项目根目录
### setting.py
    
```
djcelery.setup_loader()           #运行时，Celery便会去查看INSTALLD_APPS下包含的所有app目录中的tasks.py文件，找到标记为task的方法，将它们注册为celery task。
BROKER_URL= 'amqp://guest@localhost//'   #Broker代理地址
CELERY_RESULT_BACKEND = 'amqp://guest@localhost//'         #分布式任务队列Backend数据存储地址
CELERY_ACCEPT_CONTENT = ['json']
```    
  
### celery.py  


```
from __future__ import absolute_import, unicode_literals
import os
from celery import Celery
from django.conf import settings

# set the default Django settings module for the 'celery' program.
os.environ.setdefault('DJANGO_SETTINGS_MODULE', 'Recycle.settings')

app = Celery('Recycle')

# Using a string here means the worker doesn't have to serialize
# the configuration object to child processes.
# - namespace='CELERY' means all celery-related configuration keys
#   should have a `CELERY_` prefix.
# app.config_from_object('django.conf:settings', namespace='CELERY')

app.config_from_object('django.conf:settings')#, namespace='CELERY') #版本

# Load task modules from all registered Django app configs.
# app.autodiscover_tasks()
app.autodiscover_tasks(lambda: settings.INSTALLED_APPS)


@app.task(bind=True)
def debug_task(self):
    print('Request: {0!r}'.format(self.request))
```

### init.py

```
from __future__ import absolute_import, unicode_literals

# This will make sure the app is always imported when
# Django starts so that shared_task will use this app.
from .celery import app as celery_app

__all__ = ['celery_app']
```
## app目录
### tasks.py

```
from celery import shared_task
import time



@shared_task()
def add(a, b):
    time.sleep(10)
    # print(a + b)
    return a + b
```

### views.py
```
def ():
   add.delay(1,2)
```

### window启动

---

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





### celery4异常：

---

```
/anaconda/anaconda3/lib/python3.6/site-packages/celery/backends/amqp.py:67: CPendingDeprecationWarning: 
    The AMQP result backend is scheduled for deprecation in     version 4.0 and removal in version v5.0.     Please use RPC backend or a persistent backend.
  alternative='Please use RPC backend or a persistent backend.')

```
原因解析:
在celery 4.0中 rabbitmq 配置result_backbend方式变了,以前是跟broker一样:  
`
result_backend = 'amqp://guest:guest@localhost:5672//'
`
 
现在setting对应的是rpc配置:  
`
result_backend = 'rpc://'
`


参考链接:
http://docs.celeryproject.org/en/latest/userguide/configuration.html#std:setting-event_queue_prefix




