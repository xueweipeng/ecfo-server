# ofo_gov_bi(未完成)

# 服务器环境搭建
- 阿里云ECS CentOS release 6.8 (Final)
- 程序安装目录 /data/app

## 升级Python版本
- 系统自带Python版本为2.6，需要首先升级至2.7

## 安装必须的依赖
- yum install mysql-devel (否则下面安装mysql-python时会报错)

## git clone代码
- 生成sshkey ssh-keygen -t rsa -C "yourname@ofo.com"
- cat ~/.ssh/id_rsa.pub
- 通过web登录，个人设置，配置ssh key
- cd /data/app 进入程序目录
- git clone git@yourproject.git

## 安装必须的依赖库(主要切换至venv)
- pip install -r requirements.txt

## 安装配置nginx
- yum install nginx
- 拷贝bi的nginx.cfg 并配置
- 启动nginx： nginx

## 运行
生产模式多进程
> gunicorn -D --workers=4 --bind=0.0.0.0:9100 manage:app

## 线上服务升级过程
- cd /data/apps/yourapp
- git pull origin master
- pip install -r requirements.txt (如果有package新增的话)
- 查看gunicorn是否正常： ps aux | grep gunicorn
- 杀掉进程： pkill -f "gunicorn"
- 重启gunicorn: gunicorn -D --workers=4 --bind=0.0.0.0:9100 manage:app
- 查看gunicorn是否正常： ps aux | grep gunicorn
- web 查看验证。

## nginx
- nginx的配置路径：/etc/nginx/conf.d/
- 重新载入配置： nginx -s reload
- 停止nginx: nginx -s stop
- 错误日志刷新显示：tail -f /var/log/nginx/error.log
- 错误日志刷新显示：tail -n 5 /var/log/nginx/error.log

## 当更新代码后，线上服务无法启动成功时
- 错误日志 tail -n 50 /data/apps/logs/yourlog.log
- 设置环境变量为线上debug模式: export FLASK_CONFIG=production_debug
- 启动flask自带的web服务器: python manage.py runserver -h 127.0.0.1 -p 9100
- 看到错误消息，修改。

## 使用supervisor进行配置
- 先进入到工程目录，编辑supervisor.conf 文件，然后使用下列命令进行启动服务等操作
- supervisord -c supervisor.conf                             通过配置文件启动supervisor
- supervisorctl -c supervisor.conf status                    察看supervisor的状态
- supervisorctl -c supervisor.conf reload                    重新载入 配置文件
- supervisorctl -c supervisor.conf start [all]|[appname]     启动指定/所有 supervisor管理的程序进程
- supervisorctl -c supervisor.conf stop [all]|[appname]      关闭指定/所有 supervisor管理的程序进程

## 定时任务
- 每天晚上10点备份master数据库。
- 查看任务：crontab -l
- 编辑任务：crontab -e
