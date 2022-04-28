## 线上redis需要禁用的命令 <!-- {docsify-ignore} -->

> 前一段深夜突然收到线上告警,redis可用空间不足,经排查是在某个逻辑中读取redis时使用了 keys 方法导致，在此总结下线上redis应该禁用的一些命令

* keys
* hgetall

-- 未完成 --


