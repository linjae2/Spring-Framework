# Spring-Framework
어디까지 공부할수 있을까?



## 2023.06.19

### Redis Publish, Subscribe

```java
@Configuration
public class RedisConfiguration {
    
    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        LettuceConnectionFactory lettuceConnectionFactory = new LettuceConnectionFactory();
        return lettuceConnectionFactory;
    }
    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
        redisTemplate.setConnectionFactory(redisConnectionFactory());
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new Jackson2JsonRedisSerializer<>(DataData.class));
        return redisTemplate;
    } 
    
    @Bean
    MessageListenerAdapter messageListenerAdapter() {
        return new MessageListenerAdapter(new RedisService());
    }
    
    @Bean
    RedisMessageListenerContainer redisContainer() {
        RedisMessageListenerContainer container = new RedisMessageListenerContainer(); 
        container.setConnectionFactory(redisConnectionFactory()); 
        container.addMessageListener(messageListenerAdapter(), topic());
        return container; 
    }
    @Bean
    ChannelTopic topic() {
        return new ChannelTopic("Event");
    }
    
}
```
특정 메세지를 받기 위해서는 채널이 필요하다.

- [ ] redis-cli를 통한 publish

```
> publish Event "jeong pro"
```

출처 : [spring boot에서 redis 사용하는 방법](https://jeong-pro.tistory.com/175)


### ??

- ResponseEntity<?>
- HttpStatus.(CEEATED, OK)
- @PathVariable
- 크롬 플러그인 -> 포스트맨, 부메랑(Boomerang)
- 메인 어플리케이션 클래스에 @EnableCaching 을 선언하면 스프링 부트에서 @Cacheable과 같은 캐싱 어노테이션 사용을 인식하게 됩니다.
- Spring CacheManager 타입의 RedisCacheManager를 빈으로 등록

