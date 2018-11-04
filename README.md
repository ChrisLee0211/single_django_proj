# single_django_proj
很多时候发现flask确实很轻量，但是django能否也做到一个py文件就跑起来呢？
其实是可以的。

##环境参数：
> django == 2.0
> python == 3.6


## 用法说明
正常以来说运行一个django的流程大概是这样：
- runserver指令启动
- settings模块加载
- 请求转发到路由url分配到对应视图函数

那么如果一个文件里把setting配置、view视图、url路由都包含了，那不就可以实现单文件运行django了吗？
所以，我们不再用```django-admin startproject xxx```来创建项目，而是直接新建一个xxx文件夹以及一个xxx.py文件。
大概思路是导入如下的包:
```
from django.conf import settings 
from django.http import HttpResponse
from django.urls import path
```
把view视图、url配置直接写在里面，以及要修改的setting也直接配置加载：
```
setting = {
    'DEBUG':True,
    'ROOT_URLCONF':__name__
}
#设置 ROOT_URLCONF 为 __name__ 也就是这个文件本身，也就是说，我们打算把 urlpatterns 这个变量写进这个文件中。

settings.configure(**setting)
#加载setting修改部分

def home(request):
    return HttpResponse('Hello world!')

urlpatterns = [path('^$',home,name='home')]

if __name__ == '__main__':
    import sys
    from django.core.management import execute_from_command_line
    execute_from_command_line(sys.argv)

```
那么运行怎么运行呢？注意最后的if __name__部分，之前在正常django项目中我们总是调用manage.py。
追溯manage.py代码可知，它调用了django的```exute_from_command_line(**command_line_args)```方法来运行我们的应用，我们直接把它import进来。

## 运行指令
在该文件夹目录下：
```
python xxx.py runserver
```
然后在浏览器打开本地ip即可看到‘Hello World’，实现单个文件运行django
