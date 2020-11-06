**redis和memcache的区别**

1.redis支持数据的持久化，memcached不能，当数据库挂掉之后，redis可以恢复，memcached数据就没了

2.redis支持丰富的数据类型（list,set,sorted set, string,hast）应用于不同的场景，memcached 只支持key-value的数据类型，memcached只适简单的k/v缓存

3.redis是单线程，memcached 是多线程，可以利用CPU多核的优势，效率非常优秀。

4.memcached可以缓存视频，图片等数据。

5.memcache 存储key不能超过250个字节，key的最大失效时间为30天，value不能超过1M，

