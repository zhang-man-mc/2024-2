





1.未登录访问受保护的路由，会被重定向到登录页面 



![72510188482](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1725101884823.png)



2.服务器怎么知道请求时登录状态？



3.改变为0.00.0网址。并且复习一下是不是广播地址





4.发送邮件

4.1 电子邮件格式

~~~
1.信封   （用户不关注）
2.内容
	2.1首部
	2.2主体
~~~

首部关键字，后面加上冒号：

"To:"   后面填入一个或多个收件人的电子邮件地址

“Subject:” 是邮件的主题

邮件首部还有一项是抄送“Cc:”，表示应给某某人发送一个邮件副本



5.实现用户之间的关注------多对多关系

 5.1  添加第三张表，称为`关联表`

 5.2 如果关系的两侧都在同一个表中，这种关系称为 `自引用关系` 。

![72520793497](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1725207934978.png)

 5.3 关联表follows, 每一行表示一个用户关注了另一个用户





6.REST

6.1 资源。  例如，博客应用中，用户、博客文章，评论都是资源

-  每个资源都要使用唯一的URL表示。
- 某一类资源的集合也要有一个URL。
- API还可以为某类资源的逻辑子集定义集合URL。表示资源集合的URL习惯在末端加上一个斜线，代表一种“子目录结构”

6.2 Web浏览器之外的客户端很难提供对cookie的支持。所以在API中使用cookie并不是一个很好的设计选择。

所以身份验证，

- 最佳方式使用`HTTP身份验证`，用户凭据包含在每个请求的Authorization首部中。Flask-HttpAuth扩展

- 为了避免总是发送敏感信息（例如密码），可以使用基于令牌的验证方式

  ​

  ​

  2024年9月7日22:23:27

  再看一遍flask博客，发现书中的模版，表单，表单验证，错误定制页面，以及数据库表的物理外键，对于前后端项目来说已经不实用了。因为前后可以自己实现html,以及表单数据的验证，都放在了前端，错误的定制页面也在前段实现，后端仅仅返回所需要的数据。

  那么，这部分的仅仅是了解一下。主要的精力看博客怎么实现的。


身份验证的扩展：

·Flask-Login：管理已登录用户的用户会话

·Werkzeug：计算密码散列值并进行核对

·itsdangerous：生成并核对加密安全令牌

·Flask-Mail：发送与身份验证相关的电子邮件





自己做一个前端登录页面，后端用flask。 用前后端分离，那么后端返回的是json格式，是和Java一样的json描述吗？

遗留：

1. 在视图函数中连接数据库，查询数据。
2. EP的提示无样式




通过flask shell先在Mysql中建好表，但是执行db.create_all()，数据库中并没有新增已定义好的模型

解决：利用print()函数在shell中一直在打印变量。执行print(db.Model.__subclasses__())返回的结果为空列表，这表示SqlAlchemy还未定义模型，可是我已经在别的文件中定义好了模型。

然后把该文件的模型在shell中导入，再次打印print(db.Model.__subclasses__())，返回的结果列表中就有已经定义的模型了。

![72581205919](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1725812059197.png)

最后执行db.create_all()，数据库出现新增的表了

~~~
# 查看定义的模型
print(db.Model.__subclasses__())

>>> from app.model import User
>>> db.create_all()
   
>>> jack = User('jack', 'zmc')
>>> db.session.add(jack)       
>>> db.session.commit()    

~~~

注：在命令行界面中不用print函数。直接输入变量名，shell自动会打印

才发现，前后端分离的项目不能用flask_login的会话验证用户身份，因为前后端分离会导致跨域，而cookie，session是不能跨域的。必须用令牌验证用户身份



2024年9月19日13:28:00

后端监听的端口不能设置为localhost，否则移动端无法访问后端。正确写法后端应该监听的是本机局域网ip

~~~~
FLASK_RUN_HOST=192.168.1.13
~~~~

2024年9月20日17:52:33

执行数据库相关操作，导致表卡主了。原因是死锁了，找到事务进程id, kill 进程id

~~~
show processlist; //列出当前的操作process

# 这一步会得到线程id
SELECT * FROM information_schema.INNODB_TRX

# 杀进程
kill 线程id

~~~



# 在蓝本中实现应用功能

蓝本与应用类似，也可以定义路由和错误处理程序。不同的是，在蓝本中定义的路由和错误处理程序处于休眠状态，直到蓝本注册到应用上之后，它们才成为应用的一部分。



在蓝本中编写视图函数主要有两点不同：

- 路由的装饰器由蓝本提供，因此使用的是main.route
- url_for()函数的用法不同。Flask会为蓝本中的全部端点加上一个命名空间，其URL使用url_for(main.index)获取。在蓝本中可以省略蓝本名，例如url_for(.index)，使用当前请求的蓝本名不足端点名。






# 项目的目录解读

api目录下是实现了rest式的web服务，服务器的主要功能是为客户端提供数据存取服务。

main目录下是与前端交互的核心逻辑

auth目录是用户身份验证的逻辑

其实api为客户端服务的，没有api也能跑项目



# 查询分页

flask_sqlanchemy提供了paginate()方法，第一个参数是必须的--页数





# 前端js实现 相对当前时间。其中昨天的时间显示为实际值

dayjs自定义相对时间的输出值：

难点：其他的时间单位单位会携带“前”后缀。

​            d单位输出时，不要携带“前”后缀，而显示实际值。

解决：昨天的时间不通过dayjs().fromNow()函数，而是提前判断为昨天时间，立即返回一个时分字符串

~~~
// main.js

import updateLocale from 'dayjs/plugin/updateLocale'
dayjs.extend(updateLocale)

dayjs.updateLocale('zh-cn', {
    relativeTime: {
      future: "%s后",
      past: "%s前",
      s: '几秒',
      m: "1分钟",
      mm: "%d分钟",
      h: "1小时",
      hh: "%d小时",
      dd: "%d天",
      M: "1个月",
      MM: "%d月",
      y: "1年",
      yy: "%d年"
    }
  })
  
  
// PostCard.vue
  computed: {
    from_now() {
      if(this.isYesterday(this.post.timestamp)){
        console.log('111')
        return `昨天 ${this.$dayjs(this.post.timestamp).format('HH:mm')}`
      }
      return this.$dayjs(this.post.timestamp).fromNow()
    }
  },
  methods: {
    isYesterday(date) {
      // 将输入的日期字符串转换为 dayjs 对象
      const inputDate = this.$dayjs(date).startOf('day');
      // 获取当前日期的 dayjs 对象
      const today = this.$dayjs().startOf('day');
      // 计算 inputDate 与今天日期的差值，单位为天
      const diff = today.diff(inputDate, 'day')
      // 如果差值为 -1，则说明 inputDate 是昨天
      console.log("diff",diff)
      return diff === 1
    }
  }
}
~~~

# 登录后才会显示关注者的tab

用户推出登录后会立刻清空本地保存的token，所以在同一台设备上，不用担心匿名用户会访问到上一个用户的信息。前端根据localstorage的token是否存在，决定是否渲染"关注"的tab



# 用户登录后，只保存了token到本地，并未保存用户的数据。点击个人资料页面时，前端代码并不知道自己是谁，怎么向后端查询该用户信息？

解决：携带token，发送查询用户的api即可。后端会对jw对应到属于某个用户。



# 用户最后访问时间

问题：使用@before_app_request会在所有请求前进行拦截，包括登录请求。当登录请求或匿名用户时未携带token，函数中使用current_user需要@jwt_required装饰,就会报错

解决：将 `jwt_required()` 与 `optional=True` 参数一起使用。这将允许访问路由，无论是否随请求一起发送 JWT。

并将更新时间函数放在登录成功后执行一次

~~~
@jwt_required(optional=True)
~~~

# 路由参数

当定义了一个路由如`@app.route('/user/<name>')`，这个路由期望在URL路径中有一个变量`name`。这意味着你应该使用形如`http://localhost:8080/user/some-name`的URL。

~~~
@app.route('/user/<name>')
def user():
   pass
~~~



如果你想要通过查询参数（即URL的`?name=...`部分）传递`name`，你需要调整路由定义来使用查询参数。Flask默认不会把查询参数映射到`<name>`这样的路径参数中。

~~~
@app.route('/user')
def user():
    # 获取查询参数
    name = request.args.get('name', default=None)
    if name:
        return f"Hello, {name}!"
    el
~~~



# 主页显示用户名

![72700620195](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727006201957.png)

登录成功后，返回token,用户名。前端将token保存至localstorage, 将用户信息保存至pinia，pinia对用户名保存至localstorage持久化。

退出登陆时，删除localstorage中的token，用户名



用户名在post页面，将用户名通过路由参数传给userData页面





# 数据库时间格式为datetime,传给前端变成了 Sun, 22 Sep 2024 20:09:07 GMT，dayjs解析晚了8小时

解决：后端返回给前端时将datetime转为字面字符串。

~~~
    def datetime_to_str(datetime):
        return datetime.strftime('%Y-%m-%d %H:%M:%S')
~~~



# 页头根据是否登录，采用检测本地的localstoage方法不够好，因为当用户对当前页面后退到登录页面时,localstorage的值可能未变化，导致页头需要隐藏或展示的值不能实时变化。而且登录成功后，需要手动刷新页面，才会触发对localstorage的检测



用户是否登录 根据前端的localstorage的username是否存在进行判断。当移除localstorage的username时，会手动更新一次pinia状态，那么所有依赖pinia的页头都可以同步更新了



遗留问题：已登录的用户，后退页面放回到登录页时，页头的内容仍然是已登录的状态。这需要变未登录的状态吗





# vue router跳转另外一个页面时传递整个对象：

~~~
**定义路由：**
const router = new VueRouter({
  routes: [
    {
      path: '/target-route/:obj',
      name: 'TargetRoute',
      component: TargetRouteComponent
    }
  ]
});


**传递参数：**
// 使用 router.push 或 this.$router.push
this.$router.push({
  name: 'TargetRoute',
  params: { obj: encodeURIComponent(JSON.stringify(yourObject)) }
});
// 注意：这里使用 encodeURIComponent 确保对象被正确编码


**接收参数：**
export default {
 beforeRouteEnter(to, from, next) {
    next((vm) => {
      const params = to.params.obj
      vm.user = params ? JSON.parse(decodeURIComponent(params)) : null
      vm.$nextTick(() => {})
    })
  },
}
~~~



编辑资料==>传递user对象

编辑资料【管理员】 ===> 传递user对象

这样第一次打开编辑资料页面时，表单自动会有数据



# Flask怎么获取前端的post请求携带的数据？

1.背景：前端将element plus表单数据通过post请求发给Flask, Flask通过request.args.get(), request.form.get()获取值始终为空。

解决：使用j = get_json()

~~~~
@main.route('/edit-profile', methods=['POST'])
def edit_peofile():
    user_info = request.get_json()
~~~~



# 前端保存一个状态为 当前用户 ，也就是登录的用户。以区分编辑资料时，区分身份

解决：就用localstorage中的username来区分即可

还需要加一个当前用户是否是管理员字段。



# 当页面刷新时，pinia状态被清除为ubdifined

解决：手动更新一次pinia状态

~~~
mounted() {
    // 页面刷新手动加载一次pinia
    this.currentUser.loadAdmin()
  },
~~~



 # 黑白灰样式

~~~~
# 黑色背景
background-color: #000000;

# 灰色字体
color: #9d9d9d;


# 悬浮变为白色
color: #ffffff;

~~~~

# 当前用户关注了另外一个用户后，想要页面立即刷新出关注后的状态。

原解决方法：在前端关注逻辑结束时，重新请求一次这个用户的数据

更好的：flask后端关注后，返回结果是重定向请求该用户数据的url。那么前端就不用显视多执行一个请求用户数据的操作，而让这个操作直接由服务的在关注时直接完成。





# 现象：C组件在mounted()中不能获取到该有效的props

解决 :在mounted()函数中使用nextTick()回调函数

![72718429689](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727184296897.png)





# 现象：依赖items，想要将结果更新原始数据列表。但props只能读，不能反向更新

解决：使用组件插槽。插槽中的button可以直接读取items，同时可将最终结果更新原始列表

![72719323124](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727193231241.png)



# auth目录的路由，前端需要加上前缀 /auth



# 花了3个多小时 在博客上 加上邮箱认证

犯蠢了，在工厂函数中没有加config[""]，直接用配置信息保存在变量中。flask应用并不会读变量，而是读config类中的变类。导致一直返回 `“由于目标计算机积极拒绝，无法连接。”`。还以为是自己电脑的端口被占用，邮箱授权码过期等问题。我试了其他项目邮箱都可以正常发送



![72726156810](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727261568106.png)

解决：

![72726173200](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727261732006.png)



# 匿名用户访问受保护视图时，跳转至登录页面

解决：flask的errorhandle拦截不了jwt的401，需要使用@jwt.unauthorized_loader来装饰。

​             重定向至前端登录页面

~~~
@jwt.unauthorized_loader
def missing_token_callback(error):
    return redirect(current_app.config['FRONTEND'])
~~~





# 绑定邮箱

背景：服务端渲染时，未登录时会跳转到登录页面，登录成功后自动跳转到 请求认证路由。

   自动跳转 在前后端分离上 很难实现

![72731659048](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727316590484.png)



解决方案： 邮件包含验证码而不是点击链接。从而跳转问题 就不存在了

![72733007498](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727330074984.png)



# Docker部署

1.外部数据库

~~~
docker run --name mysql -d -e MYSQL_RANDOM_ROOT_PASSWORD=yes -e MYSQL_DATABASE=flasky -e MYSQL_USER=flasky -e MYSQL_PASSWORD=1234  mysql/mysql-server:latest
~~~

2.web_blog 后端运行

~~~
# 构建镜像
docker build -t flasky:latest .

# 运行容器
docker run -d -p 192.168.1.13:8081:5000 -e DATABASE_URL=mysql+pymysql://flasky:1234@dbserver/flasky -e MAIL_USERNAME=zmc_li@foxmail.com -e MAIL_PASSWORD=jakkuvskichwbbaa -e SECRET_KEY=57d40f677aff4d8d96df97223c74d217 flasky:latest

# 运行容器 连接mysql容器
docker run --name backend -d -p 192.168.1.13:8081:5000 --link mysql:dbserver -e DATABASE_URL=mysql+pymysql://flasky:1234@dbserver/flasky -e MAIL_USERNAME=zmc_li@foxmail.com -e MAIL_PASSWORD=jakkuvskichwbbaa flasky:latest


~~~

Dockerfile

~~~~
FROM python:3.12-alpine

ENV FLASK_APP flasky.py
ENV FLASK_CONFIG production

RUN adduser -D flasky
USER flasky

WORKDIR /home/flasky

COPY requirements requirements
RUN python -m venv venv
RUN venv/bin/pip install -r requirements/docker.txt

COPY app app
COPY migrations migrations
COPY flasky.py config.py boot.sh ./

# run-time configuration
EXPOSE 5000
ENTRYPOINT ["./boot.sh"]

~~~~



前端运行：

~~~
# 打镜像
docker build -t flasky_front:latest .

#运行容器
docker run --name flasky_front -d -p 5173:80 flasky_front:latest

# 绑定安装
docker run --name flasky_front_bind -dp 5173:80 --mount type=bind,src="${pwd}",target=/src flasky_front:latest

# 利用nodeon监视文件更改
docker run --name flasky_front_bind_nodemon -dp 5173:80 -w /app --mount type=bind,src="${pwd}",target=/src  node:18.17.1 sh -c "yarn install && yarn run dev"  flasky_front:latest
~~~



# 数据库开放远程用户权限

~~~
# 登录MySQL
mysql -u root -p 

# 如果返回为空
SELECT user, host FROM mysql.user WHERE user = 'LAPTOP-R3BSJ27E';

# 则创建个角色
CREATE USER 'LAPTOP-R3BSJ27E'@'LAPTOP-R3BSJ27E' IDENTIFIED BY '1234';

# 授予权限
# ON后面的* . *，第一个*表示所有数据库，第二个*表示所有数据库表。这将授予新用户对所有数据库和表的所有权限
GRANT ALL PRIVILEGES ON *.* TO 'LAPTOP-R3BSJ27E'@'LAPTOP-R3BSJ27E';

#管理员身份启动cmd
# 重启数据库
net stop mysql
net start mysql
~~~

~~~
# python 代码
SQLALCHEMY_DATABASE_URI = os.environ.get('DEV_DATABASE_URL') or \
                              'mysql+pymysql://LAPTOP-R3BSJ27E:1234@192.168.1.13:3306/backend_flask?charset=utf8'
~~~

# 启动容器时端口不可获得。

解决：查看本机的8081端口有哪些进程，全部结束掉即可

![72749963925](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1727499639258.png)



# 后端连接Mysql，Redis

redis的前置准备：

- 复制redis.conf放到E:\ruanjian\redis\目录下

- 修改 redis.conf

  logfile ""

  dir ./

  bind 0.0.0.0

运行命令：

~~~
# 创建网络
docker network create database_n

# 运行MySql容器
docker run --name mysql -d --network database_n --network-alias mysql -v mysql_data:/var/lib/mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes -e MYSQL_DATABASE=flasky -e MYSQL_USER=flasky -e MYSQL_PASSWORD=1234 mysql/mysql-server:latest

# 运算Redis容器
docker run --network database_n --network-alias myredis -v E:\ruanjian\redis\redis.conf:/usr/local/etc/redis/redis.conf -v redis_data:/data --name my-redis -d redis redis-server /usr/local/etc/redis/redis.conf  --save 60 1 --loglevel warning

# 运行后端容器：
docker run --name backend -d -p 192.168.1.13:8081:5000 --network database_n  -e DATABASE_URL=mysql+pymysql://flasky:1234@mysql/flasky -e MAIL_USERNAME=zmc_li@foxmail.com -e MAIL_PASSWORD=jakkuvskichwbbaa -e REDIS_URL=redis://:1234@myredis:6379/0  flasky:latest

# 运行前端容器
docker run --name flasky_front -d -p 5173:80 flasky_front:latest

~~~



附Dockerfile:

后端：

~~~
FROM python:3.12-alpine

ENV FLASK_APP flasky.py
ENV FLASK_CONFIG production

RUN adduser -D flasky
USER flasky

WORKDIR /home/flasky

COPY requirements requirements
RUN python -m venv venv
RUN venv/bin/pip install -r requirements/docker.txt

COPY app app
COPY migrations migrations
COPY flasky.py config.py boot.sh ./


# run-time configuration
EXPOSE 5000
ENTRYPOINT ["./boot.sh"]

~~~

前端：

~~~
# syntax=docker/dockerfile:1


ARG NODE_VERSION=18.17.1

FROM node:${NODE_VERSION} as build-stage

# 设置工作目录
WORKDIR /app

# 拷贝 package.json 和 package-lock.json 文件到容器中
COPY package*.json ./

# 安装依赖
RUN npm install

# 拷贝所有源代码到容器中
COPY . .

# 构建项目
RUN npm run build

# 使用 nginx 作为基础镜像
FROM nginx:latest as production-stage

# 将构建好的项目文件复制到 nginx 静态文件目录
COPY --from=build-stage /app/dist /usr/share/nginx/html

# 使用容器内的默认 nginx 配置文件
COPY nginx.conf /etc/nginx/nginx.conf

# 暴露容器端口
EXPOSE 80

# 启动 nginx 服务
CMD ["nginx", "-g", "daemon off;"]

~~~



compose.yaml

执行命令： docker-compose up -d --build

~~~
services:
  flasky:
    build: .
    ports:
      - 192.168.1.13:8081:5000
    env_file: .env
    links:
      - mysql:dbserver
    restart: always
  mysql:
    image: "mysql/mysql-server:latest"
    volumes:
      - mysql_data:/var/lib/mysql
    env_file: .env-mysql
    restart: always
  redis:
    image: "redis:latest"
    volumes:
      - E:\ruanjian\redis\redis.conf:/usr/local/etc/redis/redis.conf
      - redis_data:/data
    command: redis-server /usr/local/etc/redis/redis.conf
    env_file: .env-redis

volumes:
  mysql_data:
  redis_data:

~~~



修改镜像名称：

~~~~
docker tag 原名字 新名字
~~~~



# mysql 存储emoj表情

背景：前端输入emoj表情，数据库报错 Incorrect string value: '\\xF0\\x9F\\x98\\x8

错因：数据库设置utf8mb4，但后端设置charset=utf8

解决：

- 需要保证数据库的编码为utf8mb4，排序规则为：utf8mb4_0900_ai_ci
- 后端链接数据库url中设置charset=utf8mb4

`emoj表情占用4个字节，而传统的uft8编码只会分配3个字节`





# 云服务器 部署：

访问后端端口：4289

 1.去云服务器设置防火墙 端口

![72848425255](C:\Users\19125\Desktop\2024-2月面试\job准备\python\Flask框架\基础知识\web_博客.assets\1728484252551.png)



2.开启云服务器centos7端口：

~~~
sudo firewall-cmd --zone=public --add-port=1717/tcp --permanent
sudo firewall-cmd --zone=public --add-port=4289/tcp --permanent
~~~

3.运行docker容器

~~~
# 前端
# 本地拷贝到云服务器
docker sava -o flasky_front.tar nizhenshi/flasky_front_prod:latest
将压缩包拷贝到/root/user/目录
docker load -i flasky_front.tar 
# 运行  上一条命令自动识别出镜像名称为 nizhenshi/flasky_front_prod:latest
docker run --name flasky_front -d -p 1717:80  nizhenshi/flasky_front:latest

# 后端
# 本地拷贝到云服务器
docker sava -o flasky_backend.tar nizhenshi/flasky:latest
 docker save -o redis.tar redis
docker save -o mysql.tar mysql/mysql-server


将压缩包拷贝到/root/user/目录
docker load -i flasky_backend.tar
docker load -i redis.tar
docker load -i mysql.tar


# 创建网络
docker network create database_n

# 运行MySql容器
docker run --name mysql -d --network database_n --network-alias mysql -v mysql_data:/var/lib/mysql -e MYSQL_RANDOM_ROOT_PASSWORD=yes -e MYSQL_DATABASE=flasky -e MYSQL_USER=flasky -e MYSQL_PASSWORD=1234 mysql/mysql-server:latest

# 运算Redis容器
docker run --network database_n --network-alias myredis -v /root/user/redis.conf:/usr/local/etc/redis/redis.conf -v redis_data:/data --name my-redis -d redis redis-server /usr/local/etc/redis/redis.conf  --save 60 1 --loglevel warning

# 运行后端容器：
docker run --name backend -d -p 4289:5000 --network database_n  -e DATABASE_URL=mysql+pymysql://flasky:1234@mysql/flasky -e MAIL_USERNAME=zmc_li@foxmail.com -e MAIL_PASSWORD=idycznxncyvhdeef -e REDIS_URL=redis://:1234@myredis:6379/0  nizhenshi/flasky
~~~



~~~
# 查看所有容器 包括已暂停的
docker ps -a

# 移除冲突容器
docker rm NAME/ID 

# 删除镜像
docker rmi NAME/ID
~~~



docker 国内镜像源：

~~~
  "registry-mirrors": [
    "https://9cpn8tt6.mirror.aliyuncs.com",
    "https://registry.docker-cn.com",
    "https://hub-mirror.c.163.com",
    "https://docker.m.daocloud.io"
  ]
~~~





# 邮箱

注册时提供了的邮箱，必要要进行认证才是管理员



后面绑定邮箱时候，若等于指定邮箱，则为管理员。

管理员改变为其他邮箱时，不会取消掉管理员身份







# 表单无数据时按钮不可点击

~~~
data() {
    return {
      ruleForm: {
        user: '',
        pass: ''
      },
      isChange:false
    }
  },
  watch:{
    ruleForm:{
      deep:true,
      handler(){
        this.isChange = true
      }
    }
  }
}

<el-button  :disabled="!isChange" @click="submitForm">提交</el-button>
~~~



直接判断数据是否为空即可，不用侦听器了（`适合表单初始就有值的情况`）：

~~~
computed: {
    formHasValue() {
      return this.ruleForm.user != '' || this.ruleForm.pass != ''
    }
},  
<el-button type="primary" :disabled="!formHasValue" @click="login">提交</el-button>
~~~



# 前端开发和生产环境需要切换后端ip

解决：通过VITE提供的环境变量来自动切换后端ip，可以专注界面和业务代码于开发了，再也不用手动来回切换了。

~~~
# api/index.py
const url_py =  (import.meta.env.DEV == true) ? 'http://192.168.1.13:8081':'http://117.72.109.0:4289'
~~~

同时对于请求的打印也可以只让他在开发环境中出现，生成环境则不打印。

~~~~
if (import.meta.env.DEV) {
  console.log(error)
  console.log('==>请求结束')
}

~~~~

 

点击评论，会将整个posts列表作为url参数传入分享页面，所以分享页面的文章就可以直接展示。

再从分享页面将posts.id传入到评论组件，  





# 如何根据表单内容是否发生了变化，而让按钮变得可选中状态？

解决：将网络请求的表单值赋给A，B变量。设置A变量保存初始的表单字符串值，B变量保存请求返回的对象，并绑定表单。监听判断A，B变量的对象字符串是否相等。

踩坑：网络请求的表单值不能直赋值给A，B变量，否则A，B变量指向的是同一个引用，不管表单如何变化，A，B变量都相等。

~~~

data() {
    return {
      formLabelAlign: {},
      originalForm: {},
      isChange: false
    }
  },
watch: {
    formLabelAlign: {
      deep: true,
      handler(newVal) {
        # 重点。originalForm不需要在使用JSON.stringify包裹，它已经是字符串了
        this.isChange = JSON.stringify(newVal) !== this.originalForm
      }
    }
  },
 methods: {
    getUserInfo(userId) {
      userApi.getUser(userId).then((res) => {
        if (res.data.msg == 'success') {
          this.user = res.data.data
          # 重点 。保存初始的字符串值，而不是直接赋值
          this.originalForm = JSON.stringify(res.data.data)
          this.formLabelAlign = res.data.data
        }
      })
    },
  }
  
  <ButtonClick
  :disabled="!isChange"
  />
~~~



