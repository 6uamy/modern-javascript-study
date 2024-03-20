# REST API

### REST

- HTTP의 장점을 최대한 활용할 수 있는 아키텍처로서 REST가 등장
- REST의 기본 원칙을 성실히 지킨 서비스 디자인을 "RESTful"이라고 할 수 있다.

- REST: HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처
- REST API: REST를 기반으로 서비스 API를 구현한 것을 의미한다.

### REST API의 구성

- 자원(resource), 행위(verb), 표현(representations)의 3가지 요소로 구성된다.

| 구성 요소 | 내용                        | 표현 방법        |
| --------- | ----------------------------| ---------------- |
| 자원(resource) | 자원                         | URI(엔드포인트)  |
| 행위(verb) | 자원에 대한 행위              | HTTP 요청 메서드 |
| 표현(representations) | 자원에 대한 행위의 구체적 내용 | 페이로드         |

### REST API 설계 원칙

- REST에서 가장 중요한 기본적인 원칙
  
1. URI는 리소스를 표현하는데 집중한다.

    - 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용한다.
    - 이름에 get 같은 행위에 대한 표현이 들어가서는 안된다.
    ```
    # bad
    GET /getTodos/1
    GET /todos/show/1
    
    # good
    GET /todos/1
    ```

2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현한다.

    - 클라이언트가 서버에게 요청의 종류와 목적을 알리는 방법이다.
    - 주로 5가지 요청 메서드를 사용하여 CRUD를 구현한다.
  
    | HTTP 요청 메서드 | 종류 | 목적 | 페이로드 |
    | --------- | ----------------------------| ---------------- |---|
    | GET | index/retrieve | 모든/특정 리소스 취득  | X |
    | POST | create | 리소스 생성 | O |
    | PUT | replace | 리소스의 전체 교체 | O |
    | PATCH | modify | 리소스의 일부 수정 | O |
    | DELETE | delete | 모든/특정 리소스 삭제 | X |


### 📄 참고하면 좋을 자료

- [재경님 REST API](https://www.youtube.com/watch?v=JKMh3gBPHzs)
- [진환님 API 명세서](https://www.youtube.com/watch?v=3L9mhA-HH-Y)
