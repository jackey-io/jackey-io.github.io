---
title: Introduction
layout: default
nav_order: 1
---

Jackey is [Valkey](https://github.com/valkey-io/valkey)'s Java client, derived from [Jedis](https://github.com/redis/jedis) fork, dedicated to maintaining simplicity and high performance.

# Getting started
Add the following dependencies to your `pom.xml` file:
```
<dependency>
    <groupId>io.jackey</groupId>
    <artifactId>jackey</artifactId>
    <version>5.2.0</version>
</dependency>
```

## Connect to Valkey
```java
public class JackeyTest {
    // can be static or singleton, thread safety.
    private static io.jackey.JedisPool jedisPool;
    
    public static void main(String[] args) {
        io.jackey.JedisPoolConfig config = new io.jackey.JedisPoolConfig();
        // It is recommended that you set maxTotal = maxIdle = 2*minIdle for best performance
        config.setMaxTotal(32);
        config.setMaxIdle(32);
        config.setMinIdle(16);
        jedisPool = new io.jackey.JedisPool(config, <host>, <port>, <timeout>, <password>);
        try (io.jackey.Jedis jedis = jedisPool.getResource()) {
            jedis.set("key", "value");
            System.out.println(jedis.get("key"));
        } catch (Exception e) {
            e.printStackTrace();
        }
        jedisPool.close(); // when app exit, close the resource.
    }
}
```