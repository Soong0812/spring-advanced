# SPRING ADVANCED

DB연결 및 Secret Key 추가로 Lv.0 오류를 해결

​

Lv.1에서는 WebConfig를 추가해 resolver 목록에

커스텀 resolver인 AuthUserArgumentResolver를 추가

​

Lv.2-1 

코드 순서를 바꾸어 이메일 중복 체크를 먼저 수행하고, 중복이 아니면 비밀번호를 암호화

Early Return : 실패 가능성이 있는 조건을 먼저 처리해 실패할경우 함수를 빠르게 끝낸다

​

Lv.2-2

else를 제거

​

Lv.2-3

1) UserChangePasswordRequest에 Pattern추가

2) Controller에 @Valid 적용 : changePassword 메서드에서 Request DTO에 @Valid 적용

3) Service에 있던 비밀번호 변경시 유효성 검증 로직 삭제 : 검증 로직을 요청 DTO로 이동시켜 Service는 비즈니스 로직만 수행하도록 변경.

​

Lv.3

1) 모든 Todo를 조회할 때, 각 Todo와 연관된 데이터를 개별적으로 가져오는 경우 N+1 문제가 발생할 수 있음

2) findAllByOrderByModifiedAtDesc에 @entitygraph를 사용, attributePaths로 Todo 엔티티의 user 필드 조회

3) 이렇게 Todo마다 User를 조회하는 N+1 문제 해결



Lv.4

1) matches 메서드 순서 변경 (rawPassword가 먼저, encodedPassword가 그 다음)

2) NullPointerException이 아닌 InvalidRequestException이므로 메서드명 수정

   Todo가 없을때 Todo not found가 나와야 하는데 Manager not found로 테스트 되어 있어 Todo로 수정

4) CommentService.saveComment에서 예외 타입 확인

   예외타입을 ServerException에서 InvalidRequestException로 수정

5) Test에서 todo.getUser()가 null로 설정되어 NullPointerException이 발생했는데 전체적인 코드에서는 InvalidRequestException을 기대중

   Service에 todo.getUser()가 null인 명확한 조건을 추가
