## **Portable Service Abstraction**
  * PSA 소개
    * 스프링이 제공하는 대부분의 API가 PSA임.
    * 잘 만든 인터페이스 (추상화)
      ```
      나의 코드
      확장성이 좋지 못한 코드 or 기술에 특화되어있는 코드
      ```
      ```
      나의 코드
      잘 만든 인터페이스 (PSA)
      확장성이 좋지 못한 코드 or 기술에 특화되어있는 코드
      ```
***
  * (추상화 예시) 스프링 트랜잭션
    * PlatformTransactionManager
      * https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html
      * JpaTransactionManager | DatasourceTransactionManager | HibernateTransactionManager
      * @Transactional 을 처리하는 이 Aspect 는 PlatformTransactionManager 인터페이스를 사용해서 코딩했기 때문에 빈이 바뀌어도 (JpaTransactionManager → DatasourceTransactionManager) 트랜잭션을 처리하는 Aspect 코드는 바뀌지가 않음. 그게 추상화의 장점임.
      ```
      Aspect 코드 (나의 코드) ← 바뀌지 않음
      @Transactional
      ```
***
  * (추상화 예시) 캐시
    * CacheManager
      * https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/cache/CacheManager.html
      * JCacheManager | ConcurrentMapCacheManager | EhCacheCacheManager | ...
      ```
      Aspect 코드 (나의 코드)
      @Cacheable | @CacheEvict | ...
      ```
***
  * (추상화 예시) 웹 MVC
    * @Controller 와 @RequestMapping
      ```
      나의 코드
      @Controller | @RequestMapping | ...
      Servlet | Reactive
      톰캣, 제티, 네티, 언더토우
      ```
    * 대부분의 코드는 추상화 되었기 때문에 밑단에 있는 기술을 바꾸더라도 나의 코드가 바뀌지 않음 → 이게 PSA의 가장 큰 목표이자 장점. 그렇기 때문에 테스트하기도 더 쉬움.
