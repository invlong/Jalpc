# Linux常用命令记录

一.查询磁盘相关

```shell
# 查询某目录最深一层
du -h --max-depth=1 /home
```

```shell
# 查询http请求连接
netstat -n | awk '/^tcp/ {++S[$NF]} END {for(a in S) print a, S[a]}'
# 根据pid查看进程详情
netstat -nap | grep pid
# 查看docker进程pid
docker inspect -f '{{.State.Pid}}' 容器id
lsof -p 17514
# 根据端口查看进程
sudo chown -R invlong:invlong /data
# 查询close_wait
netstat -an | grep CLOSE
# 根据端口，查询pid
netstat -anp|grep 39956|awk '{printf $7}'|cut -d/ -f1
```

