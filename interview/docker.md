**错误总结**

- centos启动docker失败
```
命令： service docker start
报错： Redirecting to /bin/systemctl start  docker.service
Job for docker.service failed. See 'systemctl status docker.service' and 'journalctl -xn' for details.
解决： 查看文件系统 /etc/docker/daemon.json 有没有这个文件，没有测创建它包括二级目录 docker
在daemon.json文件中输入以下内容:
{ "storage-driver": "devicemapper" }
如果daemon.json文件包含格式不正确的JSON，Docker将无法启动。
```

- docker解决的问题
```
为了使我们的项目跨平台使用，比如某个项目程序员在开发环境运行正常，但是运维发布在正式环境出现错误。
由于运行环境，所用的依赖包版本导致的一系列问题，docker就是解决了这样的问题。
```

- docker 使用步骤
```
1. 安装docker
2. 把我们本地项目制作成一个docker镜像
    - 以flask项目为例
    - 安装所需的包 pip install gunicorn gevent
    - 编写配置文件/gunicorn.conf.py
        workers = 5    # 定义同时开启的处理请求的进程数量，根据网站流量适当调整
        worker_class = "gevent"   # 采用gevent库，支持异步处理请求，提高吞吐量
        bind = "0.0.0.0:80"
    - gunicorn start:app -c gunicorn.conf.py 执行命令测试我们项目能否正常运行。
    - requirements.txt 创建一个所需的包文件
    - 创建一个 Dockerfile 文件，以便 Docker 镜像的构建：/Dockerfile
        FROM python:3.6
        WORKDIR /Project/demo
        
        COPY requirements.txt ./
        RUN pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
        
        COPY . .
        
        CMD ["gunicorn", "start:app", "-c", "./gunicorn.conf.py"]
    - sudo docker build -t 'testflask' . （构建我们的镜像）
    - docker images 查看我们的对像是否构建成功。
3. 注册docker hub账号，把我们的镜像上传到docker hub
4. 部署项目的时候，运维人员只需要在docker hub上把我们上传的镜像拉下来就可以发布运行。

参考链接： https://zhuanlan.zhihu.com/p/78432719
https://www.cnblogs.com/qiaoyeye/p/10677136.html

docker资料
阮一峰docker博客：http://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html
docker一小时入门：https://blog.csphere.cn/archives/22
```

