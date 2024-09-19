安装

```
apt install -y rsync inotify-tools
```

防火墙放行

873



```
/etc/rsyncd.conf
```



```
#!/bin/bash

Path=/home/test
Server=192.168.0.2
User=sync
module=sync_file

monitor() {
  /usr/bin/inotifywait -mrq --format '%w%f' -e create,close_write,delete $1 | while read line; do
    if [ -f $line ]; then
      rsync -avz $line --delete ${User}@${Server}::${module} --password-file=/etc/rsyncd.pass
    else
      cd $1 &&
        rsync -avz ./ --delete ${User}@${Server}::${module} --password-file=/etc/rsyncd.pass
    fi
  done
}

monitor $Path;

```





# 参考文档

https://tendcode.com/subject/article/rsync/



