先启动django    
`python manage.py runserver`  

启动celery工作服务器(s2项目，celery4)  
`celery -A s2 worker -l info -P eventlet`

启动celery监控  
`celery flower -A s2`
