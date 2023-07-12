# 스프링 입문 정리
## 스프링은 무엇인가 
자바 기반의 웹 프레임워크이다. -> 스프링에서 이미 기능들을 만들어 놓아서 우리가 사용하기만 하면 된다.   
순수 자바 객체를 통해 프로젝트를 구성한다.   

## 프로젝트 구성
1. **controller**는 웹으로부터 들어오는 주소를 파악하여, 어떤 페이지를 보여주고, 어떤 메소드를 불러올 지 결정하는 요소.
2. **service**는 핵심 비지니스 로직을 구현하는 요소
3. **repository**는 데이터베이스에 접근하고, 유저의 객체(도메인)을 db에 저장하고 관리하는 요소.
그리고 **domain**은 요구되는 유저정보를 가지고 있는 객체.

![](%E1%84%89%E1%85%B3%E1%84%91%E1%85%B3%E1%84%85%E1%85%B5%E1%86%BC%20%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%86%E1%85%AE%E1%86%AB%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202023-07-12%2013.58.47.png)
## 서비스 개발
repository를 구현하였는데, 여기에 유저에 관한 정보를 저장할 수 있다.   
이 repository에 접근하고 불러오는 메소드를 구현하면 된다.
```java
public Long join(Member member) {
	validateDuplicateMember(member); //중복 회원 검증
	memberRepository.save(member);
	return member.getId(); 
} 
```
## 스프링 빈
**컴포넌드 스캔**   
@Controller, @Service, @Repository, @Component, @Autowired와 같이 애노테이션을 써서 등록할 수 있다.
**직접 등록**   
Config파일을 등록해야하고, @Configuration을 class위에 쓴다.   
그리고 각각 등록할 객체 위에 @Bean이라 쓰면 완료된다.
## DB 접근 방식
h2 데이터베이스를 이용해서 db를 관리한다.
1. **순수 Jdbc**는 과거 기술이다. 뭐 하나 하기 힘들다는 것을 보여준다.
2. **스프링 JdbcTemplate**는 그나마 반복적인 작업을 줄여준다. 하지만 여전히 SQL은 작성해야 한다.
3. **JPA**는 SQL도 작성할 필요가 없다. 생산성 증가.
4. **스프링 데이터 JPA**는 그냥 안일하게 인터페이스만 만들어도 사용이 가능하다!
```java
    public interface SpringDataJpaMemberRepository extends JpaRepository<Member, Long>, MemberRepository {
        Optional<Member> findByName(String name);
    }
```
이것만 하면 findByName을 사용할 수 있다. (헉)
## AOP
메소드의 시간 측정이 필요할 때 사용된다.   
하나하나씩 다 추가할 수 없기에 AOP를 이용해야한다.   
AOP는 관심 사항이라는게 있는데, 공통 관심 사항을 적용해서, 메소드에 직접 추가할 필요 없이 알아서 다 추가해줍니다. (이는 프록시라는 기술입니다.)   

