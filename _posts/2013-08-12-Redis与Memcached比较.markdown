---
layout: post
title:  "Redis与Memcached比较"
date:   2013-08-12
author: cyan
category: server
tags: Redis Memcached 
---
如果简单地比较Redis与Memcached的区别：

1、  Redis不仅仅支持简单的k/v类型的数据，同时还提供list，set，zset，hash等数据结构的存储。

2、  Redis支持数据的备份，即master-slave模式的数据备份。

3 、 Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用。


在Redis中，并不是所有的数据都一直存储在内存中的。这是和Memcached相比一个最大的区别。Redis只会缓存所有的 key的信息，如果Redis发现内存的使用量超过了某一个阀值，将触发swap的操作，Redis根据“swappability = age*log(size_in_memory)”计 算出哪些key对应的value需要swap到磁盘。然后再将这些key对应的value持久化到磁盘中，同时在内存中清除。这种特性使得Redis可以 保持超过其机器本身内存大小的数据。当然，机器本身的内存必须要能够保持所有的key，毕竟这些数据是不会进行swap操作的。同时由于Redis将内存 中的数据swap到磁盘中的时候，提供服务的主线程和进行swap操作的子线程会共享这部分内存，所以如果更新需要swap的数据，Redis将阻塞这个 操作，直到子线程完成swap操作后才可以进行修改。

总结：

　　1.Redis使用最佳方式是全部数据in-memory。

　　2.Redis更多场景是作为Memcached的替代者来使用。

　　3.当需要除key/value之外的更多数据类型支持时，使用Redis更合适。

　　4.当存储的数据不能被剔除时，使用Redis更合适。

