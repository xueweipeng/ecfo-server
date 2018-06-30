# 开发环境搭建
- 安装PyCharm

## 配置virtualenv (Mac)
- 安装[virtualenv](https://virtualenv.pypa.io/en/stable/)，
  - 若python中不存在pip，执行 sudo easy_install pip
  - 全局安装virtualenv, 执行 sudo pip install virtualenv
- 启动终端或iTerm，cd至本目录下，即项目根目录。
- 创建venv，执行 virtualenv venv
- 激活venv: 执行 source venv/bin/activate
- 失活venv。当最终需要切换至其它开发或者实验其他项目时，执行 deactivate
- 若使用PyCharm, 选择PyCharm - Preference - Project:- Project Interpreter中，选择项目目录下新创建的venv即可。

## 安装必须的依赖库(主要切换至venv)
开发用户
- pip install flask （会安装依赖MarkupSafe, Jinja2, Werkzeug, click, itsdangerous）
- 最终, 将稳定运行的依赖导出： pip freeze > requirements.txt 

最终用户
- pip install -r requirements.txt


## 运行
开发模式单进程
> python manage.py runserver -h 127.0.0.1 -p 8080 --no-reload

生产模式多进程
> gunicorn --workers=4 --bind=0.0.0.0:9100 manage:app

## 更新模型
开发，每次更新modles.py后：
- 首先生成迁移脚本： python manage.py db migrate -m 'add table xxx'
- 在versions检查迁移脚本，无误后本地测试：python manage.py db upgrade
- 代码测试无误后，提交至代码库

部署，取得代码
- 执行升级脚本：python manage.py db upgrade
- 如果需要插入初始化数据，一并执行。

## 编码规范
- 参考Flask的编码规范：[Pocoo 风格指南](http://dormousehole.readthedocs.io/en/latest/styleguide.html)
- 命名规范
  - 类名： CamelCase ，缩写词大写（ HTTPWriter 而不是 HttpWriter ）
  - 变量名： lowercase_with_underscores
  - 方法和函数名： lowercase_with_underscores
  - 常量： UPPERCASE_WITH_UNDERSCORES
  - 被保护的成员以单个下划线作为前缀，混合类则使用双下划线。
  

