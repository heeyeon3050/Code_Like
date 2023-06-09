## **Title: [2Week] 김희연**

### **미션 요구사항 분석 & 체크리스트**

---

### 필수 미션
호감표시 할 때 예외처리 케이스 3가지를 추가로 처리

- 현재 호감표시기능이 구현되어 있고 아래와 같은 예외처리는 이미 구현되어 있다.
- 케이스 1 : 현재 로그인을 하지 않으면 호감표시를 할 수 없다.
- 케이스 2 : 현재 본인의 인스타ID를 등록하지 않은 회원은 호감표시를 할 수 없다.
- 케이스 3 : 현재 본인이 본인의 인스타ID를 호감상대로 등록할 수 없다.

**케이스 4**

: 한 명의 인스타회원이 다른 인스타회원에게 중복으로 호감표시를 할 수 없음

- [x]  중복 호감표시 테스트 코드 작성
- [x]  호감표시를 받은 사람의 인스타아이디가 중복인지 확인한다.
- [x]  중복이면 추가되지 않고, 예외 처리

**케이스 5**

: 한 명의 인스타회원이 11명 이상의 호감상대를 등록할 수 없음

- [x]  11명 이상이 등록되면 예외가 발생하는 테스트 코드 작성
- [x]  현재 로그인한 회원이 좋아하는 사람들의 수를 확인한다.
- [x]  10명이 되었는데도, 등록하려하면 예외 처리

  **케이스 6**

: 케이스 4가 발생했을 때 기존의 사유와 다른 사유로 호감을 표시하는 경우에는 성공으로 처리

- [x]  인스타 아이디가 같아도 다른 유형의 호감표시는 가능한 테스트 코드 작성
- [x]  현재 회원이 호감표시한 사람의 인스타 아이디와 등록하려는 인스타 아이디가 같은데 같은 유형의 호감표시라면 추가 불가능
- [x]  현재 회원이 호감표시한 사람의 인스타 아이디와 등록하려는 인스타 아이디가 같아도 다른 유형의 호감표시라면 호감표시 유형을 수정

### 선택 미션

**네이버 로그인 연동**

- [x] 네이버 로그인으로 가입 및 로그인 처리가 가능해야 한다. (스프링 OAuth2 클라이언트 구현)
- [x] 네이버 로그인으로 가입한 회원의 providerTypeCode : NAVER 
- [x] 클라이언트에 출력 시, 이름, 성별 같은 개인 정보는 포함되면 안된다.

### **2주차 미션 요약**

---

### **[접근 방법]**

### 필수 미션

**케이스 4**

- `likeablePersonService`에서 `like() 메소드` 활용
- 현재 로그인한 회원이 호감표시한 사람들 중에서
  `인스타 아이디`가 `추가하려는 인스타 아이디`와 같을 경우 중복으로 호감표시 할 수 없음
- `member.getInstaMember().getFromLikeablePeople()` 에 대해 반복문을 사용하여 체크

**케이스 5**

- `likeablePersonService`에서 `like() 메소드`활용
- FromLikeablePeople은 List이므로 size()를 이용하여 10명 이상인지 체크
    - `member.getInstaMember().getFromLikeablePeople().size() > 10` 일 경우, 예외 처리를 하도록 진행하였으나 11명 까지 등록이 되었음
    - 현재 `FromLikeablePeople` 의 크기에 중점을 두고 생각했어야 했음
      (내가 등록하려는 likeablePerson은 아직 등록되지 않음)
    - `member.getInstaMember().getFromLikeablePeople().size() >= 10` 으로 구현해야 함

**케이스 6**

- `likeablePersonService`에서 `like() 메소드` 활용
- 케이스 4에서 추가적으로 생각
- 현재 로그인한 회원이 호감표시한 사람들 중에서
  `인스타 아이디`가 `추가하려는 인스타 아이디`와 같을 경우 `호감표시 유형`도 같은지 체크
    - `if(lp.getAttractiveTypeCode() == attractiveTypeCode)`

      → 같을 경우, 중복으로 호감 표시할 수 없음

      → 다를 경우, 호감표시 유형만 수정

- 호감표시 유형을 수정한 후, 코드가 아닌 이름을 출력하도록 해야 함
    - `AttractiveTypeDisplayName()`을 사용

### 선택 미션

**네이버 로그인 연동**
- `네이버 developer` 가입 후 진행
- 카카오, 구글 로그인과 같은 방식으로 `application.yml`을 수정
- 네이버는 카카오, 구글과 다르게 NAVER__{id=?, nickname=?, email=?}과 같은 형식으로 출력되어서 id만 추출
    - 구글 검색을 통해 Map을 이용하여 id만 추출

### **[특이 사항]**
- 네이버 로그인 연동 시, id만 추출하기 위해 `(Map<String, Object>) oAuth2User.getAttributes().get("response");`를 사용하였는데,
    `getAttributes()`가 리턴하는 값은 `null`인데 왜 넣어주는지 궁금하다.