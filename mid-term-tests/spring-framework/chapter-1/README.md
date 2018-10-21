# Annotations (어노테이션)

## 어노테이션이란 ?

> 어노테이션을 정의함으로써 많은 컨텍스트를 제공해주는 역할을 한다.
> 어노테이션이 나오기 전에는 많은것이 xml 로 관리되었는데, 요즘은 annotation 을 정의함으로써 Spring Framework 에서 제공하는 많은 기능들을 사용할수 있다.

### @Required

bean setter methods 로 적용된다.
필요한 속성에 대해 지정해주는 어노테이션이다.

### @Autowired

속성, 세터 메소드, 생성자에서 사용된다. 무조건적인 객체에 대한 의존성을 주입시킨다. @Autowired 를 사용할시 Spring 이 자동적으로 값을 할당한다.

### @Controller

Spring Controller 클래스를 나타낸다. Spring MVC 에서 컨트롤러들을 구분하기 위해 쓰인다.

### @Service

class 에서 쓰인다. 어떠한 서비스를 제공하는 클래스를 나타낸다. (비즈니스 로직, 외부 API 호출을 하는 목적으로 쓰이는 로직)

### @Repository

DataBase 에 접근하는 객체(DAO) 클래스에서 쓰이는 어노테이션이다. DAO 의 역할을 수행하는 클래스에서 쓰인다.

### @RequestMapping

class 와 method 에서 모두 사용된다. 웹 요청과 특정 클래스 혹은 특정 메서드를 매핑시킬때 쓴다. 메서드를 핸들링 할수 있는 URI 가 주어진다.
웹 요청 방법으로는 GET, POST, PATCH, PUT, DELETE 가 있다.

### @PathVariable

URI 에서 넘어오는 파라미터 값을 파싱할수 있게 한다.

### @RequestBody

웹 요청에서 넘어오는 request body 값들을 파싱할수 있게 한다.

### @RequestParam

? 뒤로 넘어오는 파라미터 값을 파싱해준다.

### @ResponseBody

HttpMessageConverter 를 이용하여 JSON 혹은 xml 로 요청에 응답할수 있게 해주는 어노테이션이다.

### @RestController

class 수준에서만 쓰이는 어노테이션이다. 컨트롤러 메서드에서 뷰를 반환하지 않고, 도메인(모델) 객체를 반환하는 용도로 쓰인다.
@RestController 어노테이션을 쓰는순간 따로 @ResponseBody 를 지정해주지 않아도 된다.
