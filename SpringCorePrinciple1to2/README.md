# 스프링 핵심 원리 1~2 정리
객체 지향과 관련된 내용은 생략한다.   

## 프로젝트 생성
[https://start.spring.io](https://start.spring.io)사이트에서 쉽게 생성 가능하다.   
스프링 부트 버전을 2.7.14, 자바 버전을 11으로 설정하고 나머지는 그대로 둔다.   
## 회원 도메인 설계
이 프로젝트에서는 회원에 관한 내용을 설계한다.   
아래의 내용이 필요하다.   
* 회원 가입 및 조회
* 회원 등급 - VIP, Basic
* 회원 정보 데이터 - 자체 DB 또는 외부 서버와 연동
## 회원 도메인 개발
```java
public Member(Long id, String name, Grade grade) {
		this.id = id;
      this.name = name;
      this.grade = grade;
}
```
Member 클래스의 Constructor 부분이다.   
private의 id, name, grade가 있고, 이는 getter, setter으로 접근가능하다.   
## 회원 저장소 개발
여기서는 회원 정보를 저장하고 불러온다.   
```java
public interface MemberRepository {
      void save(Member member);
      Member findById(Long memberId);
} 
```
Member 클래스의 정보를 저장하는 Repository의 interface이다.   
구현은 Impl에서 구현체를 만든다.   
```java
public class MemoryMemberRepository implements MemberRepository {
	private static Map<Long, Member> store = new HashMap<>();
	@Override
	public void save(Member member) {
		store.put(member.getId(), member);
	}
	@Override
	public Member findById(Long memberId) {
		return store.get(memberId);
	}
} 
```
구현체이다. 이를 보면 Map의 ADT에 회원정보를 저정한다.   
## 회원 서비스 개발
여기서는 회원 가입에 대한 기능을 제공한다.   
```java
public interface MemberService {
	void join(Member member);
	Member findMember(Long memberId);
} 
```
MemberService를 구성하는 interface이다.   
Impl에서 구현체를 만든다. 코드는 생략한다.   
## 테스트 적극 활용하기
```java
@Test
void test()
{
//given
//when
//then
}
```
이와 같은 구성으로 테스트 케이스를 만들 수 있다.   
assertTaht().isEqualto()으로 검증 가능하다.   
## 코드 설계가 잘 되었는가
* 다른 저장소로 변경할 때 OCP를 준수할까
* DIP를 잘 지키고 있는가
* DI문제는 없는가
역할과 구현을 분리하였기에 우리는 구현 객체는 자유롭게 적용 가능하다.   
예를 들어, 할인 정책이 존재하는데, 고정 1000원 할인에서 10% 할인으로 바뀌었을 때, DiscountPolicy라는 interface아래에서 구현체만 변경 되기에, 코드의 수정이 크지 않을 것이다.   
### Cont’d
프로젝트를 살펴보면, 의존관계와 구조가 좋은 상황은 아니다.   
따라서 객체 지향을 따르는 원칙을 적용하고 스프링으로 전환하는 과정을 할 것이다. 