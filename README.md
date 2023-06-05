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

- Setter가 모두 열려있으면 변경포인트가 너무 많아서, 유지보수가 어렵다.

### 모든 연관관계는 지연로딩으로 설정!

- 즉시로딩('EAGER')은 예측이 어렵고, 어떤 SQL이 실행될지 추적하기 어렵다. 특히 JPQL을 실행할 때 N+1 문제가 자주 발생한다.
- 실무에서 모든 연관관계는 지연로딩('LAZY')으로 설정해야 한다.
- 연관된 엔티티를 함께 DB에서 조회해야 하면, fetch join 또는 엔티티 그래프 기능을 사용한다.
- @XToOne(OneToOne, ManyToOne)관계는 기본이 즉시로딩이므로 직접 지연로딩으로 설정해야한다.

### 컬렉션은 필드에서 초기화 하자.

- 컬렉션은 필드에서 바로 초기화 하는 것이 안전하다.
- null 문제에서 안전하다.

ex)
        
![image](https://github.com/Parkcharito/studySpringBootJPA/assets/100402443/26292a4a-04cb-4c75-962a-5bb056ea33e5)


