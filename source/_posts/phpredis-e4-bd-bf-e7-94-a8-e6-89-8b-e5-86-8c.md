---
title: phpredis使用手册
id: 415
categories:
  - Redis
  - 缓存技术
date: 2011-10-26 15:48:27
tags:
---

phpredis是php的一个扩展
Redis::__construct构造函数
$redis = new Redis();
connect, open 链接redis服务
参数_
host_: string，服务地址_
port_: int,端口号_
timeout_: float,链接时长 (可选, 默认为 0 ，不限链接时间)
注: 在redis.conf中也有时间，默认为300
pconnect, popen 不会主动关闭的链接
参考上面
setOption 设置redis模式
getOption 查看redis设置的模式
ping 查看连接状态
get 得到某个key的值（string值）
如果该key不存在，return false
set 写入key 和 value（string值）
如果写入成功，return ture
setex 带生存时间的写入值
$redis-&gt;setex('key', 3600, 'value'); // sets key → value, with 1h TTL.
setnx 判断是否重复的，写入值
$redis-&gt;setnx('key', 'value');
$redis-&gt;setnx('key', 'value');
delete  删除指定key的值
返回已经删除key的个数（长整数）
$redis-&gt;delete('key1', 'key2');
$redis-&gt;delete(array('key3', 'key4', 'key5'));
ttl
得到一个key的生存时间
persist
移除生存时间到期的key
如果key到期 true 如果不到期 false
mset （redis版本1.1以上才可以用）
同时给多个key赋值
$redis-&gt;mset(array('key0' =&gt; 'value0', 'key1' =&gt; 'value1'));
multi, exec, discard
进入或者退出事务模式
参数可选Redis::MULTI或Redis::PIPELINE. 默认是 Redis::MULTI
Redis::MULTI：将多个操作当成一个事务执行
Redis::PIPELINE:让（多条）执行命令简单的，更加快速的发送给服务器，但是没有任何原子性的保证
discard:删除一个事务
返回值
multi()，返回一个redis对象，并进入multi-mode模式，一旦进入multi-mode模式，以后调用的所有方法都会返回相同的对象，只到exec(）方法被调用。
watch, unwatch （代码测试后，不能达到所说的效果）
监测一个key的值是否被其它的程序更改。如果这个key在watch 和 exec （方法）间被修改，这个 MULTI/EXEC 事务的执行将失败（return false）
unwatch  取消被这个程序监测的所有key
参数，一对key的列表
$redis-&gt;watch('x');
$ret = $redis-&gt;multi() -&gt;incr('x') -&gt;exec();
subscribe *
方法回调。注意，该方法可能在未来里发生改变
publish *
发表内容到某一个通道。注意，该方法可能在未来里发生改变
exists
判断key是否存在。存在 true 不在 false
incr, incrBy
key中的值进行自增1，如果填写了第二个参数，者自增第二个参数所填的值
$redis-&gt;incr('key1');
$redis-&gt;incrBy('key1', 10);
decr, decrBy
做减法，使用方法同incr
getMultiple
传参
由key组成的数组
返回参数
如果key存在返回value，不存在返回false
$redis-&gt;set('key1', 'value1'); $redis-&gt;set('key2', 'value2'); $redis-&gt;set('key3', 'value3'); $redis-&gt;getMultiple(array('key1', 'key2', 'key3'));
$redis-&gt;lRem('key1', 'A', 2);
$redis-&gt;lRange('key1', 0, -1);

list相关操作
lPush
$redis-&gt;lPush(key, value);
在名称为key的list左边（头）添加一个值为value的 元素
rPush
$redis-&gt;rPush(key, value);
在名称为key的list右边（尾）添加一个值为value的 元素
lPushx/rPushx
$redis-&gt;lPushx(key, value);
在名称为key的list左边(头)/右边（尾）添加一个值为value的元素,如果value已经存在，则不添加
lPop/rPop
$redis-&gt;lPop('key');
输出名称为key的list左(头)起/右（尾）起的第一个元素，删除该元素
blPop/brPop
$redis-&gt;blPop('key1', 'key2', 10);
lpop命令的block版本。即当timeout为0时，若遇到名称为key **_i_**的list不存在或该list为空，则命令结束。如果timeout&gt;0，则遇到上述情况时，等待timeout秒，如果问题没有解决，则对key**_i+1_**开始的list执行pop操作
lSize
$redis-&gt;lSize('key');
返回名称为key的list有多少个元素
lIndex, lGet
$redis-&gt;lGet('key', 0);
返回名称为key的list中index位置的元素
lSet
$redis-&gt;lSet('key', 0, 'X');
给名称为key的list中index位置的元素赋值为value
lRange, lGetRange
$redis-&gt;lRange('key1', 0, -1);
返回名称为key的list中start至end之间的元素（end为 -1 ，返回所有）
lTrim, listTrim
$redis-&gt;lTrim('key', start, end);
截取名称为key的list，保留start至end之间的元素
lRem, lRemove
$redis-&gt;lRem('key', 'A', 2);
删除count个名称为key的list中值为value的元素。count为0，删除所有值为value的元素，count&gt;0从头至尾删除count个值为value的元素，count&lt;0从尾到头删除|count|个值为value的元素
lInsert
在名称为为key的list中，找到值为_pivot_ 的value，并根据参数Redis::BEFORE | Redis::AFTER，来确定，newvalue 是放在 pivot 的前面，或者后面。如果key不存在，不会插入，如果 pivot不存在，return -1
$redis-&gt;delete('key1'); $redis-&gt;lInsert('key1', Redis::AFTER, 'A', 'X'); $redis-&gt;lPush('key1', 'A'); $redis-&gt;lPush('key1', 'B'); $redis-&gt;lPush('key1', 'C'); $redis-&gt;lInsert('key1', Redis::BEFORE, 'C', 'X');
$redis-&gt;lRange('key1', 0, -1);
$redis-&gt;lInsert('key1', Redis::AFTER, 'C', 'Y');
$redis-&gt;lRange('key1', 0, -1);
$redis-&gt;lInsert('key1', Redis::AFTER, 'W', 'value');
rpoplpush
返回并删除名称为srckey的list的尾元素，并将该元素添加到名称为dstkey的list的头部
$redis-&gt;delete('x', 'y');
$redis-&gt;lPush('x', 'abc'); $redis-&gt;lPush('x', 'def'); $redis-&gt;lPush('y', '123'); $redis-&gt;lPush('y', '456'); // move the last of x to the front of y. var_dump($redis-&gt;rpoplpush('x', 'y'));
var_dump($redis-&gt;lRange('x', 0, -1));
var_dump($redis-&gt;lRange('y', 0, -1));

string(3) "abc"
array(1) { [0]=&gt; string(3) "def" }
array(3) { [0]=&gt; string(3) "abc" [1]=&gt; string(3) "456" [2]=&gt; string(3) "123" }

SET操作相关
sAdd
向名称为key的set中添加元素value,如果value存在，不写入，return false
$redis-&gt;sAdd(key , value);
sRem, sRemove
删除名称为key的set中的元素value
$redis-&gt;sAdd('key1' , 'set1');
$redis-&gt;sAdd('key1' , 'set2');
$redis-&gt;sAdd('key1' , 'set3');
$redis-&gt;sRem('key1', 'set2');
sMove
将value元素从名称为srckey的集合移到名称为dstkey的集合
$redis-&gt;sMove(seckey, dstkey, value);
sIsMember, sContains
名称为key的集合中查找是否有value元素，有ture 没有 false
$redis-&gt;sIsMember(key, value);
sCard, sSize
返回名称为key的set的元素个数

$redis-&gt;sCard('key')
sPop
随机返回并删除名称为key的set中一个元素

$redis-&gt;sPop('key')

sRandMember
随机返回名称为key的set中一个元素，不删除

$redis-&gt;sPop('key')
sInter
求交集

$redis-&gt;sInter(array('key1', 'key2')) or $redis-&gt;sInter('key1', 'key2')
sInterStore
求交集并将交集保存到output的集合
$redis-&gt;sInterStore('output', 'key1', 'key2', 'key3')
sUnion
求并集
$redis-&gt;sUnion('s0', 's1', 's2');
s0,s1,s2 同时求并集
sUnionStore
求并集并将并集保存到output的集合
$redis-&gt;sUnionStore('output', 'key1', 'key2', 'key3')；
sDiff
求差集
sDiffStore
求差集并将差集保存到output的集合
sMembers, sGetMembers
返回名称为key的set的所有元素
sort
排序，分页等
参数
'by' =&gt; 'some_pattern_*',
'limit' =&gt; array(0, 1),
'get' =&gt; 'some_other_pattern_*' or an array of patterns,
'sort' =&gt; 'asc' or 'desc',
'alpha' =&gt; TRUE,
'store' =&gt; 'external-key'
例子
$redis-&gt;delete('s'); $redis-&gt;sadd('s', 5); $redis-&gt;sadd('s', 4); $redis-&gt;sadd('s', 2); $redis-&gt;sadd('s', 1); $redis-&gt;sadd('s', 3);
var_dump($redis-&gt;sort('s')); // 1,2,3,4,5
var_dump($redis-&gt;sort('s', array('sort' =&gt; 'desc'))); // 5,4,3,2,1
var_dump($redis-&gt;sort('s', array('sort' =&gt; 'desc', 'store' =&gt; 'out'))); // (int)5
string命令
getSet
返回原来key中的值，并将value写入key
$redis-&gt;set('x', '42');
$exValue = $redis-&gt;getSet('x', 'lol'); // return '42', replaces x by 'lol'
$newValue = $redis-&gt;get('x')' // return 'lol'
append
string，名称为key的string的值在后面加上value
$redis-&gt;set('key', 'value1');
$redis-&gt;append('key', 'value2');
$redis-&gt;get('key');
getRange （方法不存在）
返回名称为key的string中start至end之间的字符
$redis-&gt;set('key', 'string value');
$redis-&gt;getRange('key', 0, 5);
$redis-&gt;getRange('key', -5, -1);
setRange （方法不存在）
改变key的string中start至end之间的字符为value
$redis-&gt;set('key', 'Hello world');
$redis-&gt;setRange('key', 6, "redis");
$redis-&gt;get('key');
strlen
得到key的string的长度
$redis-&gt;strlen('key');
getBit/setBit
返回2进制信息
**zset****（****sorted set****）操作相关**
zAdd**(_key, score, member_)**：向名称为key的zset中添加元素member，score用于排序。如果该元素已经存在，则根据score更新该元素的顺序。
$redis-&gt;zAdd('key', 1, 'val1');
$redis-&gt;zAdd('key', 0, 'val0');
$redis-&gt;zAdd('key', 5, 'val5');
$redis-&gt;zRange('key', 0, -1); // array(val0, val1, val5)
zRange**(_key, start, end_,**_withscores_**)**：返回名称为key的zset（元素已按score从小到大排序）中的index从start到end的所有元素
$redis-&gt;zAdd('key1', 0, 'val0');
$redis-&gt;zAdd('key1', 2, 'val2');
$redis-&gt;zAdd('key1', 10, 'val10');
$redis-&gt;zRange('key1', 0, -1); // with scores $redis-&gt;zRange('key1', 0, -1, true);
zDelete, zRem
zRem**(_key, member_)** ：删除名称为key的zset中的元素member
$redis-&gt;zAdd('key', 0, 'val0');
$redis-&gt;zAdd('key', 2, 'val2');
$redis-&gt;zAdd('key', 10, 'val10');
$redis-&gt;zDelete('key', 'val2');
$redis-&gt;zRange('key', 0, -1);
zRevRange**(_key, start, end_,**_withscores_**)**：返回名称为key的zset（元素已按score从大到小排序）中的index从start到end的所有元素._withscores_: 是否输出socre的值，默认false，不输出
$redis-&gt;zAdd('key', 0, 'val0');
$redis-&gt;zAdd('key', 2, 'val2');
$redis-&gt;zAdd('key', 10, 'val10');
$redis-&gt;zRevRange('key', 0, -1); // with scores $redis-&gt;zRevRange('key', 0, -1, true);
zRangeByScore, zRevRangeByScore
$redis-&gt;zRangeByScore(key, star, end, array(withscores， limit ));
返回名称为key的zset中score &gt;= star且score &lt;= end的所有元素
zCount
$redis-&gt;zCount(key, star, end);
返回名称为key的zset中score &gt;= star且score &lt;= end的所有元素的个数
zRemRangeByScore, zDeleteRangeByScore
$redis-&gt;zRemRangeByScore('key', star, end);
删除名称为key的zset中score &gt;= star且score &lt;= end的所有元素，返回删除个数
zSize, zCard
返回名称为key的zset的所有元素的个数
zScore
$redis-&gt;zScore(key, val2);
返回名称为key的zset中元素val2的score
zRank, zRevRank
$redis-&gt;zRevRank(key, val);
返回名称为key的zset（元素已按score从小到大排序）中val元素的rank（即index，从0开始），若没有val元素，返回“null”。zRevRank 是从大到小排序
zIncrBy
$redis-&gt;zIncrBy('key', increment, 'member');
如果在名称为key的zset中已经存在元素member，则该元素的score增加increment；否则向集合中添加该元素，其score的值为increment
zUnion/zInter_
参数__
keyOutput__
arrayZSetKeys__
arrayWeights__
aggregateFunction_ Either "SUM", "MIN", or "MAX": defines the behaviour to use on duplicate entries during the zUnion.
对N个zset求并集和交集，并将最后的集合保存在dstkeyN中。对于集合中每一个元素的score，在进行AGGREGATE运算前，都要乘以对于的WEIGHT参数。如果没有提供WEIGHT，默认为1。默认的AGGREGATE是SUM，即结果集合中元素的score是所有集合对应元素进行SUM运算的值，而MIN和MAX是指，结果集合中元素的score是所有集合对应元素中最小值和最大值。
**Hash****操作**
hSet
$redis-&gt;hSet('h', 'key1', 'hello');
向名称为h的hash中添加元素key1—&gt;hello
hGet
$redis-&gt;hGet('h', 'key1');
返回名称为h的hash中key1对应的value（hello）
hLen
$redis-&gt;hLen('h');
返回名称为h的hash中元素个数
hDel
$redis-&gt;hDel('h', 'key1');
删除名称为h的hash中键为key1的域
hKeys
$redis-&gt;hKeys('h');
返回名称为key的hash中所有键
hVals
$redis-&gt;hVals('h')
返回名称为h的hash中所有键对应的value
hGetAll
$redis-&gt;hGetAll('h');
返回名称为h的hash中所有的键（field）及其对应的value
hExists
$redis-&gt;hExists('h', 'a');
名称为h的hash中是否存在键名字为a的域
hIncrBy
$redis-&gt;hIncrBy('h', 'x', 2);
将名称为h的hash中x的value增加2
hMset
$redis-&gt;hMset('user:1', array('name' =&gt; 'Joe', 'salary' =&gt; 2000));
向名称为key的hash中批量添加元素
hMGet
$redis-&gt;hmGet('h', array('field1', 'field2'));
返回名称为h的hash中field1,field2对应的value
redis 操作相关
flushDB
清空当前数据库

flushAll
清空所有数据库

randomKey
随机返回key空间的一个key
$key = $redis-&gt;randomKey();

select
选择一个数据库
move
转移一个key到另外一个数据库
$redis-&gt;select(0); // switch to DB 0
$redis-&gt;set('x', '42'); // write 42 to x
$redis-&gt;move('x', 1); // move to DB 1
$redis-&gt;select(1); // switch to DB 1
$redis-&gt;get('x'); // will return 42

rename, renameKey
给key重命名
$redis-&gt;set('x', '42');
$redis-&gt;rename('x', 'y');
$redis-&gt;get('y'); // → 42
$redis-&gt;get('x'); // → `FALSE`

renameNx
与remane类似，但是，如果重新命名的名字已经存在，不会替换成功

setTimeout, expire
设定一个key的活动时间（s）
$redis-&gt;setTimeout('x', 3);

expireAt
key存活到一个unix时间戳时间
$redis-&gt;expireAt('x', time() + 3);

keys, getKeys
返回满足给定pattern的所有key
$keyWithUserPrefix = $redis-&gt;keys('user*');

dbSize
查看现在数据库有多少key
$count = $redis-&gt;dbSize();

auth
密码认证
$redis-&gt;auth('foobared');

bgrewriteaof
使用aof来进行数据库持久化
$redis-&gt;bgrewriteaof();

slaveof
选择从服务器
$redis-&gt;slaveof('10.0.1.7', 6379);

save
将数据同步保存到磁盘

bgsave
将数据异步保存到磁盘

lastSave
返回上次成功将数据保存到磁盘的Unix时戳

info
返回redis的版本信息等详情

type
返回key的类型值
string: Redis::REDIS_STRING
set: Redis::REDIS_SET
list: Redis::REDIS_LIST
zset: Redis::REDIS_ZSET
has