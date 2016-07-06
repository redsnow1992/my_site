# bash命令
## 查找到文件过多造成 `argument list too long: ls`
数 /data/spider/2016-07-05 目录下的txt文件数
```bash
-> ls /data/spider/2016-07-05/*/*.txt |wc -l
zsh: argument list too long: ls
# 改用find
-> find /data/spider/2016-07-05 -maxdepth 2 -name '*.txt' -print | wc -l
105595
```
