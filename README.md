# [TIL] 5-2. 백엔드 - Express : DELETE, PUT

## 💬 Map 을 객체로 변환하는 다른 방법
객체 ‘형태’ 이기만 한 Map 을 res body 에 전달하는 방법은 여러가지가 있다. 예를 들어 forEach문을 사용하는 방법이다. forEach문을 사용하면 객체를 하나씩 꺼내서 쓸 수 있다.
```
app.get("/channels", (req, res) => {
  var channels = {};
  db.forEach((value, key) => {
    channels[key] = value;
  });
  res.json(channels);
});
```
channles 라는 빈 객체를 하나 만들고, forEach문을 사용해 Map 의 value 값을 하나씩 꺼내서 직접 key: value 쌍의 객체 형태로 channels 에 집어넣는 방법이다.
<img width="622" alt="image" src="https://github.com/user-attachments/assets/82b4bc7e-68a1-4c23-a606-b58b2fc74b71" /><br />
그리고 포스트맨에서 send 해보면 res body 에 정상적으로 전달되는 것을 확인할 수 있다.

Map 을 객체로 변환한다는 목적은 같지만 단순히 Object.fromEntries 라는 객체 변환 함수를 사용하느냐, forEach문으로 하나씩 꺼내서 직접 객체를 만들어 주느냐의 차이이다.

### forEach문
forEach문을 한 번 살펴보자.

보통 forEach문은 향상된 for문 이라고도 한다. 주로 배열에서 인덱스를 돌면서 값을 꺼내 쓸 때 사용하는것이 for문인데, 인덱스를 돌면서 값을 하나씩 꺼내는 것을 한 방에 해결하기 위한 것이 forEach문이다.
```
const arr = [1, 2, 3, 4, 5];

arr.forEach(function () {

})
```
forEach문은 콜백함수와 사용되는데, 이 콜백함수는 배열 혹은 객체에서 요소를 하나 꺼낸 다음, 매개변수로 그 요소를 전달하여 호출된다. 콜백함수의 매개변수는 3개까지 받을 수 있는데, 어떤 값을 받는지 확인해보자.
```
const arr = [1, 2, 3, 4, 5];

arr.forEach(function (a, b, c) {
  console.log(`a: ${a}, b: ${b}, c: ${c}`);
});

//  a: 1, b: 0, c: 1,2,3,4,5
//  a: 2, b: 1, c: 1,2,3,4,5
//  a: 3, b: 2, c: 1,2,3,4,5
//  a: 4, b: 3, c: 1,2,3,4,5
//  a: 5, b: 4, c: 1,2,3,4,5
```
첫번째 매개변수는 배열 안의 값을, 두번째 매개변수는 그 값의 인덱스를, 세번째 매개변수는 전체 값을 받고 있음을 알 수 있다.

객체 형태인 Map 도 마찬가지이다.
```
let map = new Map();
map.set(7, "seven");
map.set(9, "nine");
map.set(8, "eight");

map.forEach(function (a, b, c) {
  console.log(`a: ${a}, b: ${b}, c: ${c}`);
});

// a: seven, b: 7, c: [object Map]
// a: nine, b: 9, c: [object Map]
// a: eight, b: 8, c: [object Map]
```

따라서 위의 Map 을 객체로 바꾸던 코드에서, `db.forEach((value, key)` 는, 콜백함수의 첫번째 매개변수가 값이고 두번째는 인덱스 였기 때문에 value, key 순서로 적은 것이다.

map 함수도 살펴보자.
```
const arr = [1, 2, 3, 4, 5];

arr.forEach(function (a, b, c) {
  console.log(`a: ${a}, b: ${b}, c: ${c}`);
});

//  a: 1, b: 0, c: 1,2,3,4,5
//  a: 2, b: 1, c: 1,2,3,4,5
//  a: 3, b: 2, c: 1,2,3,4,5
//  a: 4, b: 3, c: 1,2,3,4,5
//  a: 5, b: 4, c: 1,2,3,4,5

arr.map(function (a, b, c) {
  console.log(`a: ${a}, b: ${b}, c: ${c}`);
});

//  a: 1, b: 0, c: 1,2,3,4,5
//  a: 2, b: 1, c: 1,2,3,4,5
//  a: 3, b: 2, c: 1,2,3,4,5
//  a: 4, b: 3, c: 1,2,3,4,5
//  a: 5, b: 4, c: 1,2,3,4,5
```

forEach를 썼을 때와 map을 썼을 때 콘솔창을 찍어보면 결과값이 똑같다. 그럼 뭐가 다를까? 각 메서드가 무엇을 반환하는지를 살펴보면 알 수 있다.
```
const arr = [1, 2, 3, 4, 5];

const foreachArr = arr.forEach(function (a, b, c) {
  return a * 2;
});

const mapArr = arr.map(function (a, b, c) {
  return a * 2;
});

console.log(foreachArr);
//  undefined

console.log(mapArr);
//  [ 2, 4, 6, 8, 10 ]
```
각 메서드의 첫번째 매개변수에 2씩을 곱한 값을 리턴하여 변수에 저장하고 콘솔창에 찍은 결과이다.

forEach 는 값을 반환하지 않아 undefined 가 찍힌다. map 은 새로운 배열을 반환하는 함수이기 때문에, 원래 배열인 arr 의 각 값에 2를 곱한 값을 새로운 배열로 만들어 반환한다.


# ✅ DELETE
다시 돌아가서, 등록한 객체 ‘DELETE’ 도 연습해보자.

## 💬 개별삭제
먼저 개별삭제이다. 메서드는 당연히 DELETE 이고, URL은 /channels/:id 로 각 id 값을 받는다. URL이 개별조회할 때와 같아도, 메서드가 다르기 때문에 상관 없다.

개별조회 때와 마찬가지로 params.id 를 받아온다. 그 후 삭제했을 때 res body 에 전달될 메세지를 작성해준다.
```
app.delete("/channnels/:id", (req, res) => {
	let { id } = req.params;
	id = parseInt(id);
	db.delete(id);
	res.json({
		message: `채널이 삭제되었습니다.`;
	});
})
```
삭제를 한다는 것은 Map 에 존재하는 객체 데이터, 즉 id 에 해당하는 key 값의 데이터를 삭제한다는 것이다. 때문에 `db.delete(id)` 라는 코드가 필요하다.

만약 메세지에 어떤 채널이 삭제되었는지 채널의 이름까지 띄우고 싶다면, 코드를 다음과 같이 작성해줄 수 있을 것이다.
```
app.delete("/channnels/:id", (req, res) => {
	let { id } = req.params;
	id = parseInt(id);
	const channelTitle = db.get(id).channelTitle;
	db.delete(id);
	res.json({
		message: `'${channelTitle}'채널이 삭제되었습니다.`;
	});
})
```
db 에서 데이터가 삭제되기 전에 채널 이름을 변수에 저장해두면 된다.
<img width="621" alt="image" src="https://github.com/user-attachments/assets/bb3c1f2f-fa5a-43d3-a3e5-41f13aa9806e" /><br />
<img width="620" alt="image" src="https://github.com/user-attachments/assets/80a9baea-e548-4946-b66e-db5f1ef0d483" /><br />




## 💬 예외 처리
그런데 DELETE 메서드를 사용할 때, 존재하지 않는 채널을 삭제하려고 하면 오류가 뜬다. 당연하다. 없는데 삭제를 할 수 없다. 이런 예외 처리를 해보자.
```
app.delete("/channels/:id", (req, res) => {
  let { id } = req.params;
  id = parseInt(id);
  const channel = db.get(id);

  if (channel == undefined) {
    res.json({
      message: `요청하신 ${id} 경로에 존재하는 채널이 없습니다. 경로를 다시 한 번 확인해주세요.`,
    });
  } else {
    const channelTitle = channel.channelTitle;
    db.delete(id);
    res.json({
      message: `'${channelTitle}' 채널이 삭제되었습니다.`,
    });
  }
});
```
key 값을 가져오는 db.get(id) 를 여러번 사용하기 때문에 아예 변수에 저장을 해둔다. 그리고 이 id 가 undefined 일 때 전달할 메세지를 작성하고, 정상적으로 처리될 때 실행되는 함수를 else 코드 블록 안에 넣어준다.
<img width="620" alt="image" src="https://github.com/user-attachments/assets/688fc9e7-9eff-4af6-9708-9c8a60df178c" /><br />



## 💬 전체삭제
개별삭제를 했다면, 전체삭제도 해보자.

메서드는 DELETE, URL은 /channles 가 될 것이다. req 는 딱히 받아올 게 없다. res 는 전체 채널이 삭제되었다는 메세지가 뿌려지게 전달하면 될 것이다.
```
app.delete("/channels", (req, res) => {
	db.clear();
	db.json({
		message: "모든 채널이 삭제되었습니다.",
	});	
});
```
`db.clear` 에서 clear 함수는 모든 key와 value 를 삭제하는 함수이다. 특정 key의 value 를 삭제할 때는 delete 를 쓰고, 모든 데이터를 한 번에 삭제할 때는 clear 를 쓴다.

그런데 만약 삭제할 게 아무것도 없을 때 전체삭제를 시도한다면?

이 때 사용하는 프로퍼티가 size 이다. 삭제할 것이 없다는 것은 db 의 size 가 0이라는 것이다. 이를 조건문을 이용해서 작성할 수 있다.
```
app.delete("/channels", (req, res) => {
  if (db.size >= 1) {
    db.clear();
    res.json({
      message: "채널이 일괄 삭제되었습니다.",
    });
  } else if (db.size < 1) {
    res.json({
      message: "삭제할 채널이 없습니다.",
    });
  }
});
```
여기에서 코드를 실행한 결과를 res body 에 메세지로 날려줄 거라면, 코드를 이렇게 작성할 수도 있다.
```
app.delete("/channels", (req, res) => {
  var msg = "";
  if (db.size >= 1) {
    db.clear();
    msg = "채널이 일괄 삭제되었습니다.";
  } else if (db.size < 1) {
    msg = "삭제할 채널이 없습니다.";
  }
  res.json({
    message: msg,
  });
});
```
메세지를 넣을 빈 변수를 하나 선언해두고, 각 실행 코드 블록에 어떤 메세지를 전달할 건지 저장해둔 뒤에 마지막에 msg 만 전달하는 방식이다.
<img width="623" alt="image" src="https://github.com/user-attachments/assets/aca6584b-0409-43cf-88c2-0203ce47a8f5" /><br />
<img width="623" alt="image" src="https://github.com/user-attachments/assets/dfa8dc13-9866-4846-8d9b-37ca8ef78c85" /><br />
<img width="622" alt="image" src="https://github.com/user-attachments/assets/145baa9c-a969-470f-8345-1582856dad24" /><br />



# ✅ PUT
이제 수정을 할 수 있는 PUT 메서드를 사용해보자. 개별 수정을 할 것이기 때문에, URL은 /channels/:id 이다. req 는 params.id 를 받아온다. 그런데 뭘 수정할 지는 어떻게 알 수 있을까?

현재 사용 중인 db 의 프로퍼티는 채널 이름, 구독자 수, 비디오 개수인데 사용자가 수정할 수 있는 정보는 채널 이름 뿐이다. 구독자 수와 비디오 개수를 임의로 수정할 수가 없다. 그래서 params.id 와 함께 채널 이름도 받아와야 하는데, 채널 이름을 body 에 받아왔기 때문에 body 까지 함께 받아와야 한다.

그렇다면 res 에는 “(이전 채널명)이 (수정 채널명)으로 변경되었습니다.” 라는 메세지를 전달해보도록 해보자.
```
app.put("/channels/:id", (req, res) => {
  let { id } = req.params;
  id = parseInt(id);
  var channel = db.get(id);
  var oldTitle = channel.channelTitle;

  if (channel == undefined) {
    res.json({
      message: `요청하신 ${id} 경로에 존재하는 채널이 없습니다. 경로를 다시 한 번 확인해 주세요.`,
    });
  } else {
    var newTitle = req.body.channelTitle;
    channel.channelTitle = newTitle;
    db.set(id, channel);
    res.json({
      message: `'${oldTitle}'이(가) '${newTitle}'(으)로 변경되었습니다.`,
    });
  }
});
```
개별수정이기 때문에 역시 개별조회, 개별삭제와 비슷한 코드로 진행된다. 먼저 if문을 사용해 수정하려는 path가 undefined 일 시의 예외 사항을 처리해준다. 수정하려는 채널명은 채널을 등록할 때처럼 포스트맨의 req body 에 직접 작성한 raw json 으로 가져온 후, 변수에 저장해둔다. 그리고 db 의 해당 key 값의 프로퍼티에 접근하여 수정한 채널명을 새로 넣어준다. PUT 자체가 덮어씌우는 방식으로 수정을 진행하기 때문에, Map 의 set 함수를 사용해서 수정한 사항을 다시 set 해 주면 된다. 이렇게 하면 이미 채널 이름이 바뀐 상태이기 때문에, 이전 채널명은 코드 블록 밖에서 var 변수를 선언에 저장해준 뒤 불러온다.
<img width="622" alt="image" src="https://github.com/user-attachments/assets/ff452e8c-0791-441b-a80f-231f353aa244" /><br />
<img width="623" alt="image" src="https://github.com/user-attachments/assets/9ff98ca5-2abc-4bde-956f-5d8711e1c8f8" /><br />



# ✅ HTTP 상태 코드
- **2xx - 성공**
    - `200` : 조회, 수정, 삭제 성공. 서버가 요청을 제대로 처리함. 서버가 요청한 페이지를 제공함.
    - `201` : 등록 성공. 성공적으로 요청되었으며 서버가 새 리소스를 작성함.
- **4xx - 요청 오류**
    - `400` : 잘못된 요청. 서버가 요청의 구문을 인식하지 못함.
    - `403` : Forbidden, 금지됨. 서버가 요청을 거부함. 사용자가 리소스에 대한 필요 권한을 갖고 있지 않음.
    - `404` : Not Found, 찾을 수 없음. 서버가 요청한 페이지 리소스를 찾을 수 없음. url에 해당하는 api 없음.
- **5xx - 서버 오류**
    - `500`: 내부 서버 오류. 서버에 오류가 발생하여 요청을 수행할 수 없음. 서버가 죽었다(서버가 크리티컬한 오류를 맞음).
 

# ✅ 리팩토링
리팩토링이란? 기존에 작성한 코드를 좀 더 이해하기 쉽게 수정하거나 성능 혹은 안정성을 향상시키기 위해 변경하는 것이다. 내 코드에서 나쁜 부분이 있다면 리팩토링을 함으로써 클린 코드를 작성할 수 있다.

그렇다면 리팩토링은 언제 해야 할까? 리팩토링을 해야 하는 시점은 에러 및 문제점이 여러번 발견되었을 때이다. 이 횟수는 기준을 정해두면 된다. 반대로 리팩토링을 하면서 에러나 문제점을 발견할 수도 있다. 물론 에러만을 위해서가 아니라, 어떤 기능을 추가하기 전에 리팩토링을 해줄 수도 있다. 기존의 코드가 잘 작동을 하는지, 기능을 추가한다면 코드가 바뀔 일은 없는지 등을 확인하는 작업을 하는 것이다. 이 작업을 하면 기능이 추가되어도 코드가 좀 더 안정적으로 작동할 수 있다.

하지만 절대로 코드 수정이 있어서는 안 되는 시점이 있는데, 바로 배포 혹은 운영 직전이다.
