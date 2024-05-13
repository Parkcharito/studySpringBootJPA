

------------------------------------------------------------------------------------------------------------------------------------------





## 기본 Lombok 어노테이션 정리



#### @ToString 

toString 메소드를 자동으로 생성해주는 어노테이션이다


        (System.out.println(">>>" + user.toString()); 가능하게 해줌)



#### @Getter 


해당 클래스의 필드값들의 getter메소드를 자동으로 만들어주는 어노테이션이다. (데이터의 캡슐화 이유로 필수) 

#### @Setter 

해당 클래스의 필드값들의 setter메소드를 자동으로 만들어주는 어노테이션이다. (데이터의 캡슐화 이유로 필수) 

#### @NoArgsConstructor 

아무값도 존재하지 않는 생성자를 만들수 있게 만드는 어노테이션이다. (기본 생성자를 만들어줌) 

 ex) 
      
        User user = new User();



#### @AllArgsConstructor 

전체의 값을 넣는 생성자를 만들수 있게 만드는 어노테이션이다. (여기에 필드에 쓴 모든생성자만 만들어줌) 

 ex) 
      
        User user1 = new User("martin", "martin@nate.com", LocalDateTime.now(), LocalDateTime.now());

#### @RequiredArgsConstructor

초기화 되지않은 final 필드나, @NonNull 이 붙은 필드에 대해 생성자를 생성해 줍니다. @NonNull을 필드값위에 붙인다

 ex) 
           @NonNull
           private String name;
           @NonNull
           private String email;
           
#### @Data 

@ToString, @Getter, @Setter, @RequiredArgsConstructor, @EqualsAndHashCode 를 합쳐놓은 기능이다

#### @Builder 

빌더 기능을 사용 가능하게 하는 어노테이션이다.

 ex) 

        User user3 = User.builder().name("martin").email("martin@nate.com").build();
        

        
 #### @Transactional
  
데이터베이스를 다룰 때 트랜잭션을 적용하면 데이터 추가, 갱신, 삭제 등으로 이루어진 작업을 처리하던 중 오류가 발생했을 때 모든 작업들을 원상태로 되돌릴 수 있다. 모든 작업들이 성공해야만 최종적으로 데이터베이스에 반영하도록 한다. 일련의 작업들을 묶어서 하나의 단위로 처리하고 싶다면 @Transactional을 쓰자

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


-------------------------------------------------------------------------------------------------------------------------------------------


# 데이터베이스

### 데이터 마트의 정의

데이터 마트는 단일 주제 또는 LOB에 초점을 맞춘 단순한 형태의 데이터 웨어하우스입니다. 귀사의 팀은 데이터 마트를 사용해 데이터에 빠르게 액세스하고, 인사이트를 신속하게 얻을 수 있습니다. 복잡한 데이터 웨어하우스 또는 다양한 소스로부터 수동으로 집계된 데이터 내에서 원하는 데이터를 탐색하느라 시간을 낭비할 필요가 없기 때문이죠.


### 데이터 마트를 만드는 이유

데이터 마트는 기업 내 특정 팀 또는 LOB가 요청한 데이터에 대한 보다 손쉬운 액세스를 제공합니다. 예를 들어 마케팅 팀이 휴가 시즌 캠페인의 성과 개선에 도움이 될 데이터를 찾고 있다면, 여러 시스템에 흩어져 있는 데이터를 솎아내고 결합하는 작업이 시간, 정확도, 무엇보다도 비용 측면에서 더 큰 이익을 안겨줄 것입니다.

여러 소스에 흩어진 데이터 위치를 파악하는 일을 하는 팀의 경우, 데이터를 공유하고 협업하는 일에 스프레드시트를 주로 활용할 것입니다. 이와 같은 작업은 보통 인적 오류, 혼란, 복잡한 조정, 여러 개의 소스 저장소 등의 문제를 유발해 소위 말하는 '스프레드시트 악몽'을 초래하죠. 데이터 마트는 필요한 데이터가 보고서, 대시보드 및 시각화 자료로 생성되기 전에 수집 및 정리되는 중앙화된 공간으로 널리 활용되고 있습니다.

### 데이터 웨어하우스

데이터 웨어하우스는 기업 전체에 대한 비즈니스 인텔리전스 및 분석을 지원하도록 설계된 데이터 관리 시스템입니다. 데이터 웨어하우스에는 보통 기록 데이터를 포함한 방대한 데이터가 담겨있죠. 일반적으로 데이터 웨어하우스 내에 저장된 데이터는 애플리케이션 로그 파일, 트랜잭션 애플리케이션 등 광범위한 소스로부터 추출된 것들입니다. 데이터 웨어하우스는 보통의 경우 그 목적이 명확히 정의된, 구조화된 데이터를 보관합니다.
데이터 마트는 영업, 재무, 마케팅 등 단일 주제 또는 LOB에 중점을 둔 단순한 형태의 데이터 웨어하우스입니다.
그렇기 때문에 데이터 마트는 데이터 웨어하우스보다 적은 소스로부터 데이터를 수집합니다. 데이터 마트의 소스에는 내부 운영체제, 중앙 데이터 웨어하우스, 외부 데이터가 포함됩니다.

### 데이터 마트의 이점

팀 또는 특정 LOB 전용 데이터 마트는 다음과 같은 여러 이점을 제공합니다:

- SSOT. 데이터 마트의 중앙화된 특성은 부서 또는 기업의 모든 구성원이 동일한 데이터를 기반으로 의사결정을 내릴 수 있게 해줍니다. 이와 같은 특징이 주요 이점인 이유는 데이터 및 해당 데이터를 기반으로 한 예측을 신뢰할 수 있고, 덕분에 이해관계자들은 데이터 자체에 대한 논쟁을 벌이는 대신 의사결정 도출과 조치 실행에 집중할 수 있기 때문입니다.
  
- 데이터 액세스 가속화. 특정 비즈니스 팀과 사용자들은 기업 데이터 웨어하우스 내 필요한 데이터의 하위 집합에 신속하게 액세스할 수 있습니다. 그리고 이를 다양한 소스로부터 수집한 데이터와 결합할 수 있죠. 원하는 데이터 소스에 대한 연결이 설정되면, 이들은 주기적인 데이터 추출을 위해 IT 팀을 방문할 필요 없이 필요할 때마다 데이터 마트로부터 라이브 데이터를 얻을 수 있습니다. 그 결과 귀사의 비즈니스 및 IT 팀 모두 생산성 향상이라는 성과를 얻게 되죠.

- 빠른 의사결정을 가능케 하는 빠른 인사이트. 데이터 웨어하우스가 엔터프라이즈급 의사결정을 가능하게 한다면, 데이터 마트는 부서 수준의 데이터 분석을 가능하게 합니다. 분석가들은 재무, HR 등의 영역이 직면한 특정 도전과 기회에 집중해, 데이터에서 인사이트를 더욱 신속하게 도출합니다. 덕분에 더욱 현명하고 빠른 의사결정을 내릴 수 있죠.
 
- 보다 단순하고 신속한 구현. 기업 전체의 요구사항을 충족시킬 기업 데이터 웨어하우스를 구축하는 일은 상당한 시간과 노력이 필요한 일입니다. 반면 데이터 마트는 몇 개의 데이터 세트에 대한 액세스만 요구하는 특정 비즈니스 팀의 요구사항에 중점을 둘 수 있죠. 따라서 구현도 훨씬 간편하고 신속합니다.
민첩하고 확장 가능한 데이터 관리 구현. 데이터 마트는 과거 프로젝트에서 수집한 정보를 현재 작업을 지원하는 데 활용할 수 있게 하는 등 비즈니스 요구 사항에 맞는 민첩한 데이터 관리 시스템을 제공합니다. 팀은 신규 분석 프로젝트 및 진행 중인 분석 프로젝트를 기반으로 데이터 마트를 업데이트 및 변경할 수 있죠.

- 임시 분석. 일부 데이터 분석 프로젝트는 단기간 진행됩니다. 예를 들어, 팀 미팅에 앞서 2주간의 프로모션을 위한 특정 온라인 판매 분석을 완료하는 프로젝트가 포함되죠. 팀은 이와 같은 프로젝트 수행을 위해 신속하게 데이터 마트를 설정할 수 있습니다.



