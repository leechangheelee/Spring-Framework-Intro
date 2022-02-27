## **Inversion of Control**
  * IoC 소개
    * "내가 쓸 놈은 내가 만들어 쓸께..." (일반적인 의존성에 대한제어권)
      ```java
      class OwnerController {
          private OwnerRepository repository = new OwnerRepository();
      }
      ```
    * "내가 쓸 놈은 이 놈인데... 누군가 알아서 주겠지..." (IoC)
      * 내가 쓸 놈의 타입만 맞으면 어떤거든 상관없지 뭐..
      * 그래야 내 코드 테스트 하기도 편하지.
      ```java
      class OwnerController {
          private OwnerRepository repo;
          
          public OwnerController(OwnerRepository repo) {
              this.repo = repo;
          }
          
          // repo를 사용합니다.
      }
      ```
      ```java
      class OwnerControllerTest {
          @Test
          public void create() {
            OwnerRepository repo = new OwnerRepository();
            OwnerController controller = new OwnerController(repo);
          }
      }
      ```
***
  * IoC (Inversion of Control) 컨테이너
