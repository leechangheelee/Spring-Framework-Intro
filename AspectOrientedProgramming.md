## **Aspect Oriented Programming**
  * AOP 소개
    * 흩어진 코드를 한 곳으로 모아
    * 로깅, transactional 처리, 성능측정, 인증, 권한 확인 기능 등에서 사용가능
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
***
  * AOP 적용 예제
    * @LogExecutionTime 으로 메소드 처리 시간 로깅하기
      * @LogExecutionTime 애노테이션 (어디에 적용할지 표시해두는 용도)
        ```java
        /* aspect/LogExecutionTime.java */
        ...
        @Target(ElementType.METHOD)
        @Retention(RetentionPolicy.RUNTIME)
        // 애노테이션을 사용한 그 코드를 언제까지 유지할 것이냐
        // 런타임 까지 유지를 해야 스프링이 애노테이션 붙어있는곳 런타임에 찾아서 적용해주니까
        public @interface LogExecutionTime {
        }
        ```
        * @LogExecutionTime 애노테이션으로 적용할 곳 표시
          ```java
          /* SampleController.java */
          ...
          @LogExecutionTime
          @GetMapping("/context")
          public String context() {
              return "hello " + changhee;
          }
          ...
          ```
          ```java
          /* OwnerController.class */
          ...
          @LogExecutionTime
          @GetMapping("/owners/new")
          public String initCreationForm(Map<String, Object> model) {
              Owner owner = new Owner();
              model.put("owner", owner);
              return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
          }
          ...
          @LogExecutionTime
          @PostMapping("/owners/new")
          public String processCreationForm(@Valid Owner owner, BindingResult result) {
              if (result.hasErrors()) {
                  return VIEWS_OWNER_CREATE_OR_UPDATE_FORM;
              }
              else {
                  this.owners.save(owner);
                  return "redirect:/owners/" + owner.getId();
              }
          }
          ...
          @LogExecutionTime
          @GetMapping("/owners/find")
          public String initFindForm(Map<String, Object> model) {
              model.put("owner", new Owner());
              return "owners/findOwners";
          }
          ...
          @LogExecutionTime
          @GetMapping("/owners")
          public String processFindForm(@RequestParam(defaultValue = "1") int page, Owner owner, BindingResult result, Model model) {
          ...
          ```
      * 실제 Aspect (@LogExecutionTime 애노테이션 달린곳에 적용)
        ```java
        /* aspect/LogAspect.java */
        ...
        // 스프링 빈만 Aspect 가 될 수 있음
        @Component
        @Aspect
        public class LogAspect {

            Logger logger = LoggerFactory.getLogger(LogAspect.class);

            @Around("@annotation(LogExecutionTime)")
            public Object logExecutionTime(ProceedingJoinPoint joinPoint) throws Throwable {
                StopWatch stopWatch = new StopWatch();
                stopWatch.start();

                Object ret = joinPoint.proceed();

                stopWatch.stop();
                logger.info(stopWatch.prettyPrint());

                return ret;
            }
        }
        ```
