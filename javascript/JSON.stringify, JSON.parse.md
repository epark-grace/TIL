# JSON.stringify(value[, replacer, space])

## value

-   직렬화할 값
-   적용할 수 있는 자료형 : 객체, 배열, 문자, 숫자, boolean, null
-   무시되는 프로퍼티 : 함수, 심볼형, undefined

## replacer

-   원하는 프로퍼티만 직렬화하고 싶은 경우, 특히 순환참조 프로퍼티가 있는 경우 직렬화가 불가능하기 때문에 순환참조하는 프로퍼티만 제외하고 싶을 때 사용
-   직열화할 프로퍼티만 담은 **배열** 또는 기존 프로퍼티 값을 대신하여 쓸 값을 반환하는 **함수**

## space

-   전송 목적이 아닌 로깅 목적인 경우, 가독성을 높이기 위해 중첩 객체를 별도의 행에 들여쓰기하여 직렬화
-   space로 들여쓸 space 크기를 지정

```javascript
let customer = {
  name: 'John Doe',
}

let provider = {
  name: 'John Roe',
  goods: ['computer', 'keyboard', 'mouse'],
  customer: customer
}

customer.provider = provider;

let result = JSON.stringify(provider, function(key, value) {
  if (key === "provider") return undefined; //프로퍼티 값을 undefined로 반환하므로 직렬화하지 않고 무시됨
  return value;
}, 2);

console.log(result);
/*
{
  "name": "John Roe",
  "goods": [
    "computer",
    "keyboard",
    "mouse"
  ],
  "customer": {
    "name": "John Doe"
  }
}
*/
```

---

# JSON.parse(string[, reviver])

## string

역직렬화할 JSON 형식의 문자열

## reviver

특정 프로퍼티 값을 변경하거나 제외하여 역직렬화하고 싶을 경우 사용

```javascript
let json = '{"name":"홍길동", "birthday":"1980-10-28"}'

let person = JSON.parse(json, function(key, value) {
  if (key === "birthday") {
    return new Date(value);
  }
  return value;
});

person.age = new Date().getFullYear() - person.birthday.getFullYear() + 1;
console.log(person.age);
```
