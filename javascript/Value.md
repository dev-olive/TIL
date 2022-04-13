## 값

### 배열

자바스크립트 배열은 어떤 타입의 값이라도 담을 수 있는 그릇이다.

```javascript
var a = [1, "2", [3]];
a.length; //3
a[0]===1; //true
a[2][0]===3; //true
```

배열 크기는 미리 정하지 않고도 선언할 수 있으며 원하는 값을 추가하면 된다.

(빈/빠진 슬롯이 있는) '구멍 난'배열을 다룰 때는 조심해야 한다. a[1] 슬롯 값은 undefiend가 될 것 같지만, 명시적으로 a[1]=undefined 세팅한 것과 똑같지는 않다.

```javascript
var a = [];
a[0] = 1;
a[2] = [3]; //a[1] 슬롯을 건너뜀

a[2]; //undefined
a.length; //3
```

배열 인덱스는 숫자인데, 배열 자체도 하나의 객체여서 키/프로퍼티 문자열을 추가할 수 있다. 하지만 배열의 길이는 증가하지 않는다.

```javascript
var a = [];
a[0] = 1;
a["foobar"] = 2;

a.length; //1
a["foobar"]; //2
a.foobar; //2
```

그런데 키로 넣은 문자열의 값이 표준 10진수 숫자로 타입이 바뀌면, 문자열 키가 아닌 숫자 키를 사용한 것과 같은 결과가 초래된다.

```javascript
var a = [];
a["13"] = 42;
a.length; //14
a // [비어있음x13, 42]
```

배열에 문자열 타입의 키/프로퍼티를 두는 건 피하자.

#### 유사 배열

DOM 쿼리 작업을 수행하면 배열은 아니지만 변환 용도로는 충분한, 유사 배열 형태의 DOM 원소 리스트가 반환된다. 다른 예로, 함수에서 arguments 객체를 사용하여 인자를 가져오는 것도 마찬가지다.

```javascript
function foo(){
	var arr = Array.prototype.slice.call(arguments);
  arr.push("bam");
  console.log(arr);
}
foo("bar", "baz"); //["bar", "baz", "bam"]
```

slice()함수에 인자가 없으면 기본 인자 값으로 구성된 배열을 복사한다.

ES6부터는 기본 내장 함수 `Array.from()`이 이 일을 대신한다.



### 문자열

자바스크립트 문자열은 생김새만 비슷할 뿐 문자 배열과 같지 않다. 그렇지만 문자열은 불변 값이고, 배열은 가변 값이다.

```javascript
var a = "foo";
var b = ["f", "o", "o"];

a[1] = "0"; //a.charAt(1)로 접근하는 것이 맞다.
b[1] = "0";

a; // "foo"
b; // ["f", "0", "o"];
```

문자열은 불변 값이므로 문자열 메서드는 그 내용을 바로 변경하지 않고 항상 새로운 문자열을 생성한 후 반환한다. 반면에 대부분의 배열 메서드는 그 자리에서 곧바로 원소를 수정한다.

```javascript
c = a.toUpperCase();
a === c; // false
a; // "foo"
c; // "FOO"

b.push("!");
b; ["f", "0", "o", "!"]
```

문자열의 순서를 거꾸로 뒤집는 경우, 배열에는 reverse()라는 가변 메서드가 있지만 문자열은 사용이 불가함

```javascript
a.reverse; // undefined
b.reverse(); //["!", "o", "0", "f"]
```

배열의 가변 메서드는 통하지 않고 빌려 쓰는 것 또한 안된다. 

```javascript
//Hack
var c = a.split("") // 'a'를 문자의 배열로 분할함
				 .reverse() // 문자 배열의 순서를 거꾸로 뒤집음
				 .join(""); // 문자 배열을 합쳐 다시 문자열로 되돌림
```



### 숫자

자바스크립트의 숫자 타입은 number가 유일하며, integer, functional decimal number를 모두 아우른다.

자바스크립트의 정수는 부동 소수점 값이 없는 값이다. (예: 42.0은 정수 42와 같음)

#### 숫자 구문

대부분의 숫자는 10진수가 디폴트고 소수점 이하 0은 뗸다.

```javascript
var a = 42.3000;
var b = 42.0;

a; //42.3
b; //42
```

아주 크거나 아주 작은 숫자는 지수형으로 표시하며, toExponential() 메서드의 결과값과 같다.

```javascript
var a = 5E10;
a; // 50000000000
a.toExponential(); // "5e+10"
```

숫자 값은 Number 객체 래퍼로 박싱할 수 있기 때문에 Number.prototype 메서드로 접근할 수 있다.

##### toFixed() 메서드

```javascript
var a = 42.59;
a.toFixed(0); //"43"
a.toFixed(2); //"42.59"
a.toFixed(3); //"42.590"
```

실제로는 숫자 값을 문자열 형태로 반환한다.

##### toPrecision() 메서드

```javascript
var a = 42.59;
a.toPrecision(1); //"4e+1"
a.toPrecision(2); //"43"
a.toPrecision(5); //"43.590"
```

두 메서드는 숫자 리터럴에서 바로 접근할 수 있다. 하지만 .이 소수점일 경우 프로퍼티 접근자가 아닌 숫자 리터럴의 일부로 해석된다.

```javascript
42.toFixed(3); //SyntaxError
(42).toFixed(3); //"42.000"
0.42.toFixed(3); //"0.420"
42..toFixed(3); //"42.000"
```

2진, 8진, 16진 등 다른 진법

```javascript
0o363 // 243의 8진수
0b1110011; // 243의 2진수
```



#### 작은 소수 값

```javascript
0.1 + 0.2 === 0.3 // false
```

이진 부동 소수점 숫자의 부작용 문제로 위와 같은 문제가 생긴다. 이진 부동 소수점으로 나타낸 0.1과 0.2는 원래의 숫자와 일치하지 않는다.

미세한 '반올림 오차'를 혀용 공차로 처리하는 방법이 있다. 이렇게 미세한 오차를 '머신 입실론'이라고 하는데 자바스크립트 숫자의 머신 입실론은 2^-52다.

```javascript
function numbersCloseEnoughToEqual(n1,n2){
  return Math.abs(n1-n2) < Number.EPSILON;
}
var a = 0.1+0.2;
var b = 0.3;
numbersCloseEnoughToEqual(a,b); //true
//ES6부터는 머신 입실론이 Number.EPSILON으로 미리 정의되어 있다.
```



#### 정수인지 확인

ES6부터는 Number.isInteger()로 어떤 값의 정수 여부를 확인한다.

```javascript
Number.isInteger(42); //true
Number.isInteger(42.00); //true
Number.isInteger(42.3); //false
```

### 특수 값

#### 값 아닌 값

undefined 타입의 값은 undefined 밖에 없다. null 타입도 값은 null 뿐이다. 그래서 이 둘은 타입과 값이 항상 같다. 

undefined와 null은 종종 '빈'값과 '값 아닌'값을 나타낸다. undefined와 null의 의미를 어떻게 정의하여 쓰든지, null은 식별자가 아닌 특별한 키워드이므로 null이라는 변수에 뭔가 할당할 수 없다. 그런데 undefined는 식별자로 쓸 수 있다.

#### Undefined

느슨한 모드에서는 전역 스코프에서 undefined란 식별자에 값을 할당할 수는 있지만 추천하지 않는다.(정말 굳이..)

##### void 연산자

undefined는 내장 식별자로, 값은 undefined지만, 이 값은 void 연산자로도 얻을 수 있다. 표현식 void __는 어떤 값이든 '무효로 만들어', 항상 결과값을 undefined로 만든다. 기존 값은 건드리지 않고, 연산 후 값은 복구할 수 없다.

```javascript
var a = 42;
console.log(void a, a); //undefined 42
```

void 연산자는 값이 존재하는 곳에서 그 값이 undefined가 되어야 좋을 경우에만 사용하자. (아마도 그렇게 해아 할 경우도 거의 없고 극히 제한적으로 쓰이겠지만)

#### 특수 문자

##### The not number, number

NaN은 글자 그대로 '숫자 아님(Not a Number)'이다.

```javascript
var a = 2/"foo"; //NaN
typeof a === "number" //true
a == NaN //false
a === NaN //false
```

'숫자 아님의 typeof는 숫자다', 어떤 변수값이 특수한 실패 숫자(NaN)인지 여부를 확인할 때 NaN과 직접 비교는 불가하다. (NaN !== NaN이다.) 비교 불능이라면 NaN여부는 `isNaN(a)`으로 확인 가능하다. 하지만 isNaN은 '인자 값이 숫자인지 여부를 평가'하는 기능이 전부이기 때문에 아래와 같은 버그가 있다.

```javascript
var b = "foo";
window.isNaN(b); //true
```

"foo"는 당연히 숫자가 아니지만, 그렇다고 NaN도 아니다. ES6부터는 Number.isNaN을 사용하면 NaN여부를 체크할 수 있다.

##### 무한대

자바스크립트에서는 0으로 나누기 연산이 잘 정의되어 있어 에러 없이 Infinity라는 결과 값이 나온다.

```javascript
var a = 1/0 //Infinity
var b = -1/0 //-Infinity
```

자바스크립트는 유한 숫자 표현식을 사용하므로 덧셈, 뺄셈 같은 연산 결과가 +무한대/-무한대가 될 수 있다. 연산 결과가 너무 커서 표현하기 곤란할 때 '가장 가까운 수로 반올림'모드가 결과값을 정한다.

무한을 무한으로 나누면, '정의되지 않은 연산'이며 결과값은 NaN이다.

##### 0

자바스크립트엔 +0, -0이 존재한다. 음의 영은 표기만 -0으로 하는 것이 아니라 특정 수식의 연산 결과 또한 -0으로 떨어진다. 그러나 -0을 문자열화하면 항상 "0"이다. 반대로 문자열에서 숫자로 바꾸면 있는 그대로 보여준다.

#### 특이한 동등 비교

NaN이나 -0의 동등 비교는 독특하기 때문에, 동등비교를 하기 위해서는 Object.is()를 사용하여 비교를 한다.



### 값 vs 레퍼런스

자바스크립트레서는 어떤 변수가 다른 변수를 참조할 수 없다. 그냥 안됨. 자바스크립트에는 값의 타입만으로 값-복사, 레퍼런스-복사 둘 중 한쪽이 결정된다.

```javascript
var a = 2;
var b = a; //'b'는 언제나 'a'에서 값을 복사
b++;
a; //2
b; //3
var c = [1,2,3];
var d = c; //'d'는 공유된 [1,2,3]값의 레퍼런스
d.push(4);
c; // [1,2,3,4];
d; // [1,2,3,4];
```

null, undefined, strine, number, boolean, symbol 같은 단순 값은 값-복사 방식으로 할당/전달됨

객체나 함수 등 합성 값은 할당/전달 시 반드시 레퍼런스 사본을 생성함. 따라서 레퍼런스로 실제 공유한 배열 값이 변경되면 이 공유 값 한 군데에만 영향을 미치므로 두 레퍼런스는 갱신된 값을 동시에 바라보게 된다.

레퍼런스는 변수가 아닌 값 자체를 가리키므로 A 레퍼런스로 B 레퍼런스가 가리키는 대상을 변경할 수는 없다.

합성 값을 값-복사에 의해 효과적으로 전달하려면 손수 값의 사본을 만들어 전달한 레퍼런스가 원본을 가리키지 않게 하면 된다. 

```javascript
foo(a.slice())
```

인자 없이 slice()를 호출하면 전혀 새로운 배열의(얕은 복사에 의한)사본을 만든다. 이렇게 복사한 사본만을 가리키는 레퍼런스를 전달하니 foo()는 a의 내용을 건드릴 수 없다. 반대로 스칼라 원시 값을 레퍼런스처럼 바뀐 값이 바로바로 반영되도록 넘기려면 원시 값을 다른 합성 값으로 감싸야 한다.

```javascript
function foo(wrapper){
	wrapper.a = 42;
}
var obj = {
	a: 2
};
foo(obj);
obj.a; //42
```

