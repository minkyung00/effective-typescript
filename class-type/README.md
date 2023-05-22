## Class

> 클래스를 사용하면 서버에서 응답 받은 데이터를 **화면에서 보기 쉽도록 가공**할 수 있다.

서버에서 아래와 같은 데이터를 전송해준다고 가정하자.

``` ts
interface PersonData {
  name: string;
  age: number;
  gender: "man" | "woman";
  birth_date: Date;
}
```

위 데이터를 직접 사용할 경우 아래와 같은 문제점이 발생할 수 있다.

- `age` 값을 활용해 새로운 데이터를 생성해야한다. 
- 프론트에서는 camelCase를 사용하지만 서버에서는 snake_case를 사용한다. 
- `birth_date`는 Date 객체이기 때문에 화면에 바로 값을 보여줄 수 없다. 

<br />

클래스를 사용해서 위의 문제점을 해결해보자.
``` ts
class Person {
  /* 타입 정의 */
  name: string;
  age: number;
  gender: 'man' | 'woman';
  birthDate: string;
  adult: boolean;
  
  /* constructor 내부에서 값을 가공할 수 있다. */ 
  constructor(data: PersonData) {
    this.name = data.name;
    this.age = data.age;
    this.gender = data.gender;
    this.birthDate = this.formatBirthDate(data.birth_date);
    this.adult = this.checkAdult(data.age);
  }
  
  /* getter, setter를 사용해 원하는 값을 만들어낼 수 있다. */
  get thisMan() {
    return this.gender === 'man';
  }
  
  /* 클래스 외부에서는 접근할 수 없도록 private로 설정할 수 있다 */
  private formatBirthDate(date: Date) {
    // ...
    return 'yyyy-mm-dd';
  }
  
  private checkAdult(age: number) {
    return age > 19;
  }
}
```

- 화면에서 바로 보여줄 수 있도록 데이터를 가공할 수 있다.
- 별도의 레이어나 컴포넌트 내부에서 데이터 관련된 로직을 처리하지 않아도 된다.
- 즉, 클래스를 사용하여 관심사를 분리할 수 있다. 

``` ts
const person = new Person(data); // Type: Person
```
- 타입이자 값으로 클래스를 활용할 수 있다.

<br />

### BFF(Backend For Frontend)
화면에 필요한 데이터만 받는 방식으로 클래스 뿐만 아니라 **프론트엔드를 위한 중간서버인 BFF**가 있다.

- 서버가 여러대인 경우, 여러 도메인을 하나의 도메인으로 일치시켜줄 수 있다. 
- 서버로부터 API 응답 값을 BFF에서 받아서 프론트엔드에 필요한 데이터로 변경해준다.
  - 즉, 화면에 필요한 데이터만 받을 수 있다. 

#### 일반 API

``` ts
// GET/user/get_profile

{
  message: "성공"
  profile: {
    uid: 1234,
    nickname: "치즈",
    email: "cheese@test.com",
    create_dt: "1995-01-31 00:00:00" // 실제 화면에는 필요하지 않은 값
    user_id: "5678" // 실제 화면에는 필요하지 않은 값
  }
  response_time: "2022-03-03 17:49:39" // 실제 화면에는 필요하지 않은 값
  result_code: 0
}
```

#### BFF

``` ts
// user.resolver

const resolvers = {
  user: async () => {
    const response = await kakaopageApi.getProfile();
    // createUserWithUserVo 에서 원하는 User 타입에 맞게 가공해줍니다.
    return createUserWithUserVo(response.profile);
  },
};
```

<br />

- Ref: [카카오페이지는 BFF(Backend For Frontend)를 어떻게 적용했을까?](https://fe-developers.kakaoent.com/2022/220310-kakaopage-bff/)
