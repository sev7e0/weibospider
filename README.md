# 重新维护项目文档(:construction:building...)
## 项目说明 :triangular_flag_on_post:

本项目基于weibospider改造，主要替换了一些微博已经修改的接口，主体功能未做改动，后期看需要再决定。

## 基础环境配置 :triangular_flag_on_post:

1. 操作系统:computer:Linux、macOS、Windows

2. 安装python3

3. 安装MySQL5.7

4. 安装Redis

## 项目部署 :triangular_flag_on_post:

1. 克隆项目(推荐使用pycharm的checkout from version control功能),或使用命令行
```shell
git clone https://github.com/sev7e0/weibospider.git
```

2. 配置config/spider.yaml,项目中提供了模板及注释,一步一步慢慢配置.

3. 初始化数据库基础表,上一步骤中确认配置好mysql信息后,执行config/create_all.py,数据库将会自动初始化.

4. 手动向MySQL中插入基础数据(推荐使用datagrip或者Navicat),也可以使用命令行.
```sql
//密码改为自己的
insert into login_info (name,password,enable) value ('xxx','xxx',0);
//uid改成自己想要爬取的用户id(默认榜姐)
insert into seed_ids (uid,is_crawled,other_crawled,home_crawled) value ('1713926427',0,0,0)
```

5. 使用命令行启动worker,-Q后可以添加想要启动并接受执行的任务队列(窗口不要关闭).
```shell
celery -A tasks.workers -Q login_queue,user_crawler,fans_followers,search_crawler,home_crawler worker -l info -c 1
```

6. 以上步骤完成后执行first_task_execution/login_first.py,提示登录成功后,可以去Redis中查看相关cookie是否被缓存(推荐使用Redis客户端),或使用命令行
```shell
redis-cli -h 127.0.0.1 -p 6379 -a "java"
```

7. 确认登录成功后,执行first_task_execution/user_first.py会看到步骤5中的窗口打印日志,未报异常就可以查看wbuser表中爬取到的相关数据,至此一次简单的微博用户数据爬取完成.