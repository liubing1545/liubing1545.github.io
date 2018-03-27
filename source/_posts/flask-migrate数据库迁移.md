---
title: flask-migrate数据库迁移
date: 2016-08-11 09:50:52
tags: [python, flask, flask-migrate]
---
在迭代开发中，会阶段性的变更数据库模型，更新数据库。  
变更时，为了不丢失数据库中的数据，可以使用数据库迁移工具。  

**flask－migrate**是对Alembic的轻量级封装，并且已经被集成到了flask-script中。  
## 安装
    $ pip install flask-migrate

## 配置
    from flask_sqlalchemy import SQLAlchemy as SQLAlchemy
    from flask_script import Manager, Shell
    from flask_migrate import Migrate, MigrateCommand
    
    app = Flask(__name__)
    manager = Manager(app)
    db = SQLAlchemy(app)
    migrate = Migrate(app, db)
    
    manager.add_command("shell", Shell(make_context=make_shell_context))
    manager.add_command('db', MigrateCommand)
    
    if __name__ == '__main__':
        manager.run()
        
## 创建迁移仓库
    $ python xxx.py db init
## 创建迁移脚本
    $ python xxx.py db migrate
## 数据库迁移
    $ python xxx.py db upgrade    
    