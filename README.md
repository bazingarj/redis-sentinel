# redis-sentinel
redis-sentinel client for php based on phpredis extension.

## examples
Get Redis master address and create Redis object:
```php
$sentinel = new \bazingarj\RedisSentinel\Sentinel();
$sentinel->connect('127.0.0.1', 6379);
$address = $sentinel->getMasterAddrByName('mymaster');

$redis = new Redis();
$redis->connect($address['ip'], $address['port']);
$info = $redis->info();
print_r($info);
```

Create redis-sentinel pool and create Redis object:
```php
$sentinel_pool = new \bazingarj\RedisSentinel\SentinelPool();
$sentinel_pool->addSentinel('127.0.0.1', 26379);
$sentinel_pool->addSentinel('127.0.0.1', 26380);

$address = $sentinel_pool->master('mymaster');
print_r($address);

$redis = $sentinel_pool->getRedis('mymaster');
$info = $redis->info();
print_r($info);
```

In order to prevent redis/sentinel to wait too long for connections in case of 
issues with the Redis backend it's advisable to use a timeout (in seconds):

```php
$sentinel_pool->addSentinel('127.0.0.1', 26380, 1.0); # 1 second timeout
```

Forked From: https://github.com/huyanping/redis-sentinel, https://github.com/RobinUS2/redis-sentinel
