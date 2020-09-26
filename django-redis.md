按照给出的使用django-redis的大致流程:
```
pip install django-redis
```
```
/settings.py

CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://your_password@127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
        }
    }
}
```

我遇到了如下问题：
![avatar](https://github.com/ScCcWe/django-error-together/blob/master/%E9%94%99%E8%AF%AF.png)

百度了很久，还是没什么收获。网上大都是说redis模块的版本要小于3，将redis的版本降为2.10.6。这并没有实质性的解决问题。因为django-redis模块需要redis版本大于3.0。

后来又看了官方文档，看到一个
```
In some circumstances the password you should use to connect Redis is not URL-safe, in this case you can escape it or just use the convenience option in OPTIONS dict:

CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
            "PASSWORD": "mysecret"
        }
    }
}
Take care, that this option does not overwrites the password in the uri, so if you have set the password in the uri, this settings will be ignored.
```
我就跟着上面的设置方式，然后就成功了。
```
/settings.py

CACHES = {
    "default": {
        "BACKEND": "django_redis.cache.RedisCache",
        "LOCATION": "redis://127.0.0.1:6379/1",
        "OPTIONS": {
            "CLIENT_CLASS": "django_redis.client.DefaultClient",
            "PASSWORD": "your_password"
        }
    }
}
```
