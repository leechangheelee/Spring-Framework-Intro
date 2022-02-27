## **Aspect Oriented Programming**
  * AOP 소개
    * 흩어진 코드를 한 곳으로 모아
    * 흩어진 AAAA와 BBBB
      ```java
      class A {
          method a () {
              AAAA
              오늘은 7월 4일 미국 독립 기념일이래요.
              BBBB
          }
          
          method b () {
              AAAA
              저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
              BBBB
          }
      }
      ```
      ```java
      class B {
          method c() {
              AAAA
              점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
              BBBB
          }
      }
      ```
    * 모아 놓은 AAAA와 BBBB
      ```java
      class A {
          method a () {
              오늘은 7월 4일 미국 독립 기념일이래요.
          }
          
          method b () {
              저는 아침에 운동을 다녀와서 밥먹고 빨래를 했습니다.
          }
      }
      ```
      ```java
      class B {
          method c() {
              점심은 이거 찍느라 못먹었는데 저녁엔 제육볶음을 먹고 싶네요.
          }
      }
      ```
      ```java
      class AAAABBBB {
          method aaaabbbb(JoinPoint point) {
              AAAA
              point.execute()
              BBBB
          }
      }
      ```
      * Spring AOP 는 PROXY 패턴을 이용하여 구현함
