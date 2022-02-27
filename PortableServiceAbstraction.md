## **Portable Service Abstraction**
  * PSA 소개
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
  * (예시) 스프링 트랜잭션
    * PlatformTransactionManager
      * https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/PlatformTransactionManager.html
      * JpaTransactionManager | DatasourceTransactionManager | HibernateTransactionManager
      * @Transactional 을 처리하는 이 Aspect 는 PlatformTransactionManager 인터페이스를 사용해서 코딩했기 때문에 빈이 바뀌어도 (JpaTransactionManager → DatasourceTransactionManager) 트랜잭션을 처리하는 Aspect 코드는 바뀌지가 않음. 그게 추상화의 장점임.
      ```
      Aspect 코드 ← 바뀌지 않음
      @Transactional
      ```
