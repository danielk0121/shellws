# 레디스 자주 쓰는 명령어

```
# 벤치마크
# -d 단위 데이터 크기
# -q 테스트 결과만 출력하기(진행 상황 x)
# -t 테스트 메소드
redis-benchmark -d 15000 -q -t get,set,lpush,lpop,sadd,spop
```

# jedis 학습코드, pool, get, set, close
```
@Test
public void testRedisConnect(){
	JedisPoolConfig jedisPoolConfig = new JedisPoolConfig();
	JedisPool pool = new JedisPool(jedisPoolConfig, "localhost", "8011", 1000, "password");

	Jedis jedis = pool.getResource();

	jedis.set("foo", "bar");
	String value = jedis.get("foo");
	assertTrue( value != null && value.equals("bar") );

	jedis.del("foo");
	value = jedis.get("foo");
	assertTrue( value == null );

	if( jedis != null ){
		jedis.close();
	}
}
```

# 