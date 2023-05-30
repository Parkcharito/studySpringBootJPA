-------------------------------------------------------------------------------------------------------------------------------------------


### Lombok 어노테이션 정리


- @ToString : toString 메소드를 자동으로 생성해주는 어노테이션이다

        (System.out.println(">>>" + user.toString()); 가능하게 해줌)

- @Getter : 해당 클래스의 필드값들의 getter메소드를 자동으로 만들어주는 어노테이션이다. (데이터의 캡슐화 이유로 필수) 

- @Setter : 해당 클래스의 필드값들의 setter메소드를 자동으로 만들어주는 어노테이션이다. (데이터의 캡슐화 이유로 필수) 

- @NoArgsConstructor : 아무값도 존재하지 않는 생성자를 만들수 있게 만드는 어노테이션이다. (기본 생성자를 만들어줌) 

      ex) 
      
        User user = new User();

- @AllArgsConstructor : 전체의 값을 넣는 생성자를 만들수 있게 만드는 어노테이션이다. (여기에 필드에 쓴 모든생성자만 만들어줌) 

      ex) 
      
        User user1 = new User("martin", "martin@nate.com", LocalDateTime.now(), LocalDateTime.now());

- @RequiredArgsConstructor : 초기화 되지않은 final 필드나, @NonNull 이 붙은 필드에 대해 생성자를 생성해 줍니다. @NonNull을 필드값위에 붙인다

      ex) 
           @NonNull
           private String name;
           @NonNull
           private String email;
           
- @Data : @ToString, @Getter, @Setter, @RequiredArgsConstructor, @EqualsAndHashCode 를 합쳐놓은 기능이다

- @Builder : 빌더 기능을 사용 가능하게 하는 어노테이션이다.

        ex) 

        User user3 = User.builder().name("martin").email("martin@nate.com").build();
        
        
-------------------------------------------------------------------------------------------------------------------------------------------

## 인티티 설계시 주의점

### 엔티티에는 가급적 Setter를 사용하지 말자

Setter가 모두 열려있으면 변경포인트가 너무 많아서, 유지보수가 어렵다.

