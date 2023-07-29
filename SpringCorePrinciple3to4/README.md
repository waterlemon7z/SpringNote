# 스프링 핵심 원리 3~4 정리
## 새로운 할인 정책
기존에 있던 할인 정책을 이용하지 않고 새롭게 적용할 경우다.   
이의 경우에는 새로운 클래스를 만드는 것이 아닌, 기존에 사용 하고 있던 할인 정책의 interface만 implement받아 정의하면 된다.   
그러면, 코드를 대부분 수정하지 않고 할인 정책만 변경할 수 있는 것이다.   
```java
public class RateDiscountPolicy implements DiscountPolicy {}
```
객체지향의 장점으로 볼 수 있다.   
하지만 여기서도 문제가 발생한다.   
새로운 RateDiscountPolicy로 변경해줘야 한다는 점이다.   
그러면 코드를 수정해야 하므로 적절하지 않다. 의존관계를 생각하면 쉽다.   
그래서 상위 객체에서 interface에만 의존하면 된다는 점이다.   
## 관심사의 분리
그래서 우리는 역할을 분리 해야한다.   
어떤한 역할이 있는건 알지만, 그건 누가하던지 상관 없다는 이야기다.   
예를 들어, 누군가 연기를 하는데 시키고 보니 조정석이었다는 이야기다.   
그래서 배역을 나누는 자, pd, 작가와 같이 역할을 분리해야 한다.   
AppConfig class에서 우리는 한번에 인스턴트를 생성하여 주입을 한번에 한다.   
각 클래스에서 사용하는 인스턴트를 생성자를 통해 입력받으면, 의존관계를 해결된다.   
그러한 AppConfig는 main에서 한번만 실행하면 appconfig.member를 통해 불러올 수 있다.   
```java
public static void main(String[] args) {
          AppConfig appConfig = new AppConfig();
          MemberService memberService = 	appConfig.memberService();
}
```
Appconfig가 최상위 관리자라고 보면 된다.   
그러면 위에서 구성한 새로운 할인 정책을 appconfig에서만 변경하면 된다는 점이다.   
* 새로운 할인 정책 개발
* 새로운 할인 정책 적용과 문제점 관심사의 분리
* AppConfig 리팩터링
* 새로운 구조와 할인 정책 적용   

## 스프링 컨테이너 생성
```java
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
```
스프링 컨테이너를 불러오는 과정이다.   
컨테이너는 애노테이션 기반의 자바 클래스로 만들 수 있다.   
### 스프링 컨테이너 생성 과정
* 스프링 컨테이너 생성
* 스프링 빈 등록
* 스프링 빈 의존관계 설정   
## 스프링 컨테이너 조회
```java
String[] beanDefinitionNames = ac.getBeanDefinitionNames();
ac.getBean(); //빈 이름으로 조회하기
```
모든 스프링 빈의 이름을 가져올 수 있다.   
## 스프링 빈 기본적인 조회
* ac.getBean(이름, 타입)
* ac.getBean(타입)   
으로 가능하다.
만약에 해당하는 빈이 없다면, 예외를 발생시킨다.   
동일한 타입이 두개 이상이라면, 빈 이름을 지정하여 출동을 피할 수 있다.   


