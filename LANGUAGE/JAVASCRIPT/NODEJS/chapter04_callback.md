# NodeJS

<https://velopert.com/255>
<http://yubylab.tistory.com/entry/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%9D%98-%EC%BD%9C%EB%B0%B1%ED%95%A8%EC%88%98-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0>

## 콜백

### Callback함수

- 함수는 Object 타입이며 다른 일급객체와 똑같이 사용(String, Array, Number, 등등..)
- function 자체가 객체이므로 변수안에 담을 수 도 있고 인수로서 다른 함수에 전달 해 줄수도있고, 함수에서 만들어질수도있고 반환 될수도 있음
- Callback function은, 특정 함수에 매개변수로서 전달된 함수를 지칭
- Callback function은 그 함수를 전달받은 함수 안에서 호출

```js
// 보시면, click 메소드의 인수가 변수가 아니라 함수이죠?
// click 메소드의 인수가 바로 Callback 함수입니다.
$("#btn_1").click(function() {
  alert("Btn 1 Clicked");
});
//설명: click 메소드에 이름이 없는 callback function을 인수로 전달
//jQuery 안의 click 메소드에서는, 마우스 클릭이 있으면 callback function을 호출하게 설정
```

### Blocking Code

- Callback 함수가 사용되지 않는, Blocking Code
- 어떤 작업을 실행하고 기다리면서 코드가 “막히”게 됨

```txt
[input.txt]
Let's understand what is a callback function.
What the HELL is it?
```

```js
//[main.js]
var fs = require("fs");

var data = fs.readFileSync(__dirname+'input.txt');

console.log(data.toString());
console.log("Program has ended");
```

```bash
$ node main.js
Let's understand what is a callback function.
What the HELL is it?
Program has ended
```

### Non-Blocking Code

- Callback 함수가 사용된 Non-Blocking Code
- 함수가 실행될 때, 프로그램이 함수가 끝날때까지 기다리지않고 바로 그 아래에있는 코드들을 실행
- 그 다음에 함수에 있던 작업이 다 끝나면 콜백함수를 호출

```js
//[main.js]
var fs = require("fs");

fs.readFile(__dirname+'input.txt', function (err, data) {
    if (err) return console.error(err);
    console.log(data.toString());
});
console.log("Program has ended");
```

- 모든 Node 어플리케이션의 비동기식 함수에서는 첫번째 매개변수로는 error를, 마지막 매개변수로는 callback 함수 받음
- fs.readFile() 함수는 비동기식으로 파일을 읽는 함수이고, 도중에 에러가 발생하면 err 객체에 에러 내용을 담고 그렇지 않을 시에는 파일 내용을 다 읽고 출력

```bash
$ node main.js
Program has ended
Let's understand what is a callback function.
What the HELL is it?
```

- readFile() 메소드가 실행 된 후, 프로그램이 메소드가 끝날때까지 대기하지 않고 곧바로 다음 명령어로 진행하였기 때문에,프로그램이 끝났다는 메시지를 출력 한 후에, 텍스트 내용을 출력
- 그렇다고 해서 프로그램이 끝나고나서 텍스트 내용을 출력한것은 아닙니다.
- 프로그램이 실질적으로 끝난건 텍스트가 출력된 후입니다.
- 만약에 readFile() 다음에 실행되는 코드가 그냥 console.out() 이 아니라
readFile() 보다 작업시간이 오래걸리는 코드였다면 텍스트 출력


- callback 함수를 사용하여 이렇게 프로그램의 흐름을 끊지 않음으로서, Non-Blocking 코드를 사용하는 서버는 Blocking 코드를 사용하는 서버보다 더 많은 양의 요청을 빠르게 처리

-----------------------------------------------------------------------

최근 들어 node.js를 이용한 서버사이드 프로그래밍이 증가하면서 callback함수에 대한 이해도를 높일 필요가 있었습니다.
우리가 DOM을 다룰때 가장 흔히 사용하는 라이브러리인 jQuery도 대부분의 이벤트를 callback 방식으로 처리를 하고 있습니다. 아무런 생각없이 사용하던 callback 함수의 이해와 제대로된 사용을 위해 공부하던중 좋은 자료가 있어 번역 및 추가적인 내용들을 추가해 보았습니다.

출처는 : http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/ 입니다.




1 .자바스크립트의 콜백함수의 이해와 사용법 (Learn JavaScript Higher-order Functions—Callback Functions)

자바스크립트에서 함수는 일급객체입니다. 일급 객체가 되기 위해서는 몇 가지 조건을 만족하여야 합니다.
1.변수나 데이터 구조 안에 담을 수 있다.
2.파라미터로 전달 할 수 있다.
3.반환값으로 사용 할 수 있다.
4.런타임에 생성될 수 있다.

자바스크립트에서 함수가 일급객체이기 때문에 우리는 함수를 인자로 사용하는 콜백패턴을 자바스크립트에서 사용할 수가 있습니다.

이제부터 우리는 콜백함수의 모든것에 대해서 알아보겠습니다.
자바스크립트에서 콜백함수는 가장 널리 사용되는 기법이고, 이를 이용해 jquery나 각종 자바스크립트로 만들어진 라이브러리에서 사용되고 있는 것을 확인 할 수 있습니다.
만약 자바스크립트를 개발하는 입장에서는 여전히 콜백함수가 명확하게 개념이 잡혀있지 않다면 이번기회에 확실히 잡으시길 바랍니다.

콜백합수는 함수형 프로그래밍에서부터 발생한 패러다임입니다. 간단히 말해 함수형 프로그래밍은 함수를 인자처럼 사용을 하는 것인데 이는 소수의 전문가들만이 이해하고 사용하는 기법이었지만 지속적으로 누군가에 의해 설명되고 분석되어 오면서 우리가 쉽게 이해하고 사용 할 수 있는 수준이 되었습니다.
그중에서 가장 일반적인 기술 중 하나가 바로 콜백함수 입니다.
다시한번 간략하게 설명하면 함수를 인자로 넘겨 사용하겠다 입니다.

2 .콜백함수 또는 Higher-order Function이라는게 무슨말일까요? (What is a Callback or Higher-order Function?)

콜백함수 higher-order function 이라고 알려진 이 녀석은 파라메터로 함수를 전달하는 녀석입니다. 그리고 전달받은 그 함수를 함수의 내부에서 실행시킵니다.
그럼 Jquery에서 사용하는 간단한 콜백함수의 예를 볼까요?
$("#btn_1").click(function() {
     alert("Btn 1 Clicked");
});


보시는것처럼 우리는 click 함수의 인자로 함수를 전달하고 있습니다. 그리고 그 함수는 click 함수가 실행이 되면 동작을 합니다. 위와 같은 형태가 가장 전형적인 자바스크립트의 콜백함수 입니다.
또 다른 매우 전형적인 콜백함수의 예제가 있습니다.
var friends = ["Mike", "Stacy", "Andy", "Rick"];
friends.forEach(function (eachName, index){
     console.log(index + 1 + ". " + eachName); // 1. Mike, 2. Stacy, 3. Andy, 4. Rick
});


forEach라는 함수안에 익명의 함수를 넣어서 forEach 구문 내에서 동작을 하게 하고 있습니다.
일단 여기까지 우리가 얼마나 거리낌 없이 콜백함수를 사용하고 있었는지 알게 되었습니다. 그럼 실제 콜백함수가 어떤식으로 동작을 하는지에 대해 집중해 보겠습니다.

3.콜백함수는 어떻게 동작하는가? (How Callback Functions Work?)

자바스크립트에서 모든 것은 객체입니다. 심지어 함수도 객체입니다. 일급객체! 그래서 우리는 함수를 변수처럼 그리고 다름 함수의 리턴값으로 사용이 가능하게 됩니다. 함수를 콜백으로 다른 함수의 인자처럼 사용 할 경우에는 오직 합수의 정의만을 넘겨주면 됩니다.
즉, 함수의 이름만을 넘겨주면 됩니다. 굳이 함수라고 하여 끝에 () 같은 것을 붙여줄필요가 없습니다. 다음과 같은 형태입니다.
setInterval(callback, 1000)


위의 코드 처럼 함수의 인자로 전달된 함수의 경우에는 언제든 원하는 시점에 실행을 시킬수가 있습니다.
이게 매우 중요한 점인데 콜백함수는 전달 받은 즉시 바로 실행이 될 필요가 없다는 말입니다. 함수의 이름처럼 “called back” , 함수의 내부의 어느 특정시점에 실행을 합니다. 처음에 봤었던 jquery의 예제에서도 봤지만
$("#btn_1").click(function() {
     alert("Btn 1 Clicked");
});


익명의 함수는 click이라는 함수의 내부에서 나중에 호출이 됩니다. 아무런 이름이 없지만 이 함수를 담고 있는 click 이라는 함수는 언제든지 이 함수를 호출할 수가 있습니다.

4. 콜백함수는 클로저다(Callback Functions Are Closures)

우리가 다른 함수의 인자로 콜백함수를 전달할 때, 전달받은 함수의 특정시점에 그 콜백함수가 동작을 할 것입니다. 마치 전달받은 함수가 이미 콜백함수를 내부에서 정의 한 것처럼 말이지요.
이 말은 콜백은 클로저라는 말과 같습니다. 클로저에 대한 자세한 내용은 자바스크립트-변수로-자바스크립트-이해하기 에서 확인이 가능합니다.
간단하게 설명을하면 전달된 콜백함수는 콜백함수를 포함한 함수 내부의 인자에 접근이 가능하고 심지어 전역변수에도 접근이 가능한 상태가 됩니다. 즉, 기존의 함수가 가진 스코프에 새로운 내부 스코프가 추가가 되는 것입니다. 이로 인해 발생하는 이슈가 있는데 자세한 내용은 자바스크립트-변수로-자바스크립트-이해하기 에서 확인하시기 바랍니다.
우리가 알다시피 클로저는 포함하고 있는 함수의 스코프에 접근을 할수 있습니다. 그래서 콜백함수는 포함하고 있는 함수의 변수에 접근이 가능하고 심지어 전역변수에도 접근이 가능합니다.

5. 콜백함수 적용의 기본 원칙(Basic Principles When Implementing Callback Functions)

콜백함수는 복잡하지 않지만 적용하기 위한 몇가지 원칙이 있습니다.

이름이나 익명의 함수를 사용하라(Use Named OR Anonymous Functions as Callbacks)

위에서 본 jquery나 foreach함수의 예제에서 보았듯이 우리는 익명의 함수를 파라메터로 사용을 합니다. 이러한 방식이 가장 일반적인 패턴입니다. 또 다른 보편적인 방법으로는 함수를 정의해 해당 함수의 이름을 파라메터로 넘기는 방식입니다.
var allUserData = [];

// 콘솔에 결과를 찍는 함수
function logStuff (userData) {
    if ( typeof userData === "string")
    {
        console.log(userData);
    }
    else if ( typeof userData === "object")
    {
        for (var item in userData) {
            console.log(item + ": " + userData[item]);
        }

    }

}

// 두 개의 인자를 받아서 마지막에 콜백함수를 호출한다.
function getInput (options, callback) {
    allUserData.push (options);
    callback (options);

}

// getInput 함수를 호출할 때 , 우리는 logStuf이라는 함수의 이름을 인자로 넘긴다.
// 그래서 logStuff 은 콜백함수가 되어 getInput이라는 함수의 내부에서 동작을 할것이다.
getInput ({name:"Rich", speciality:"JavaScript"}, logStuff);
//  name: Rich
// speciality: JavaScript


콜백함수로 파라매터 전달(Pass Parameters to Callback Functions)

콜백함수가 실행이 될 때는 그냥 일반 함수와 동일하게 동작을 합니다. 그래서 우리는 콜백함수에 파라메터를 전달할 수가 있습니다. 우리는 파라메터로 콜백함수를 감싸고있는 함수 내부의 어떠한 프로퍼티라도 파라메터로 전달할 수가 있습니다. 아래의 예제를 실행하면 options 파라메터를 콜백함수에 전달할수 있습니다. 전역변수와 지역변수를 파라메터로 전달할 수 있습니다.
//전역변수
var generalLastName = "Clinton";

function getInput (options, callback) {
    allUserData.push (options);
// 전역변수를 콜백함수의 인자로 전달한다.
    callback (generalLastName, options);
}


콜백함수가 실행 되기 전에 함수임을 명확하게 하기(Make Sure Callback is a Function Before Executing It)

콜백함수가 인자로 전달되어 함수의 내부에서 실행이 될 때 전달받은 인자가 함수인지를 명확하게 정의 하고 실행하는 것이 좋은 습관이다.
위의 함수를 고쳐보겠습니다.
function getInput(options, callback) {
    allUserData.push(options);

    // callback 이 함수 인지를 확인합니다.
    if (typeof callback === "function") {
    // callback 이 함수인지를 확인 했으니까 함수호출합니다.
        callback(options);
    }
}


만약에 이러한 확인 작업이 없다면 callback파라메터를 넘기지 않거나 혹은 함수가 아닌 값을 넘기게 되는 경우에는 실행중에 에러가 발생하는 문제가 일어납니다.

this를 사용한 메서드를 콜백으로 사용시 문제 (Problem When Using Methods With The this Object as Callbacks)

콜백함수가 this객체를 사용하는 메서드인 경우에는 우리는 반드시 this객체의 컨택스트를 보호할 수 있도록 콜백함수를 수정해야 합니다.
예를들면 전역함수에 인자로 콜백함수가 전달된 경우에는 this객체가 window객체를 가리키게 만들거나 또는 콜백함수를 감싸고 있는 메서드를 가리키게 해야합니다.
//객체를 생성합니다.
// 다른 함수의 콜백함수로 전달한 메서드를 정의합니다.

var clientData = {
    id: 094545,
    fullName: "Not Set",
    // setUserName clientData의 메서드입니다.
       setUserName: function (firstName, lastName)  {
        // this는 clientData라는 객체를 지칭하고 있습니다.
      this.fullName = firstName + " " + lastName;
    }
}

function getUserInput(firstName, lastName, callback  )  {

    // Do other stuff to validate firstName/lastName here

    // Now save the names
    callback (firstName, lastName);
}

getUserInput ("Barack", "Obama", clientData.setUserName);

console.log (clientData.fullName);// 값에 설정되지 않음

// fullName 프로퍼티가 window object의 인자로 세팅됨
console.log (window.fullName); // Barack Obama


이 코드예제에서 clientData내의 setUserName을 실행하면 예상과 달리 clientData내의 this.fullName 의 값으로 세팅이 되지 않습니다.
대신에 window 오브젝트의 값으로 세팅이 되어버리는데 이는 getUserInput메서드가 글로벌 함수이기 때문이다. 이러한 일이 발생하는 원인은 this라는 객체가 window객체라는 글로벌한 객체를 가리키고있기 때문이다.
이게 자바스크립트가 가진 문제점 중에 하나입니다. 작성자가 생각한 this가 때로는 의도치 않게 다른 객체로 나타나게 되는 경우가 있습니다.
이러한 문제를 피하기 위해 jQuery에서는 $(this) 를 사용하거나 커피스크립트에서는 내부에서 처리를 해주고 있습니다.

Call 과 Apply를 통한 this 보호 (Use the Call or Apply Function To Preserve this )

우리는 Call과 Apply 함수를 통해 이러한 문제를 해결할 수 있습니다. 일단 모든 자바스크립트의 함수에는 Call 과 Apply라는 메서드를 가지고 있습니다. 그리고 이 함수들은 함수 내부에서 this객체를 유지하게 하고 인자들은 함수로 전달하는 역할을 합니다.
Call함수는 항상 첫번째 인자로 this 객체를 사용합니다. 그리고 나머지 인자들은 콤마로 구분하여 보이지 않게 전달을 합니다.
Apply 함수의 경우에도 첫번째 인자로 this객체를 사용합니다. 하지만 마지막 파라메터의 경우 값들이 배열형태로 존재합니다.
이게 보면 말은 매우 복잡합니다. 하지만 코드를 통해 보시면 쉽게 이해가 될것입니다. 이전의 문제가 있었던 코드를 고쳐보겠습니다. 우리는 우선 Apply함수를 사용해보겠습니다.
//callbackObj라는 파라메터를 추가했습니다.
function getUserInput(firstName, lastName, callback, callbackObj)  {
    // Do other stuff to validate name here

    // 아래의 apply함수는 callbackObj에 this객체를 매핑합니다.
    callback.apply (callbackObj, [firstName, lastName]);
}


Apply함수를 통해 this 객체를 제대로 정의 할 것입니다. 우리는 이제 실제 getUserInput을 실행을 하면 제대로 clientData의 값을 세팅하는 것을 확인할수 있습니다.

// 우리는 clientData.setUserName 메서드와 clientData 객체를 파라메터로 전달합니다. clientData객체는 Apply함수에의해 this 객체로 정의가 될 것입니다.  
getUserInput ("Barack", "Obama", clientData.setUserName, clientData);

// 제대로된 결과를 리턴합니다.
console.log (clientData.fullName); // Barack Obama


이러한 동작은 Call 함수를 통해서도 그대로 동작하게 될것입니다.

6 .다중 콜백함수(Multiple Callback Functions Allowed)

우리는 여러 개의 콜백함수를 파라매터를 전달할 수 있습니다. 아래의 코드는 전형적인 jquery의 ajax 함수 들입니다.
function successCallback() {
    // Do stuff before send
}

function successCallback() {
    // Do stuff if success message received
}

function completeCallback() {
    // Do stuff upon completion
}

function errorCallback() {
    // Do stuff if error received
}

$.ajax({
    url:"http://fiddle.jshell.net/favicon.png",
    success:successCallback,
    complete:completeCallback,
    error:errorCallback

});


7. 콜백 지옥의 문제와 해결책 (Callback Hell” Problem And Solution)

간단한 실행을 위한 비동기적인 코드는 가끔 수많은 콜백함수의 연속으로 이어지는 경우가 있습니다. 아래의 코드를 보시다시피 매우 지저분합니다. 이른바 콜백지옥입니다. 이러한 코드는 매우 가독성이 떨어집니다.
아래의 코드의 경우 node-mongodb-native 에서 가져온 예제입니다.
var p_client = new Db('integration_tests_20', new Server("127.0.0.1", 27017, {}), {'pk':CustomPKFactory});
p_client.open(function(err, p_client) {
    p_client.dropDatabase(function(err, done) {
        p_client.createCollection('test_custom_key', function(err, collection) {
            collection.insert({'a':1}, function(err, docs) {
                collection.find({'_id':new ObjectID("aaaaaaaaaaaa")}, function(err, cursor) {
                    cursor.toArray(function(err, items) {
                        test.assertEquals(1, items.length);

                        // Let's close the db
                        p_client.close();
                    });
                });
            });
        });
    });
});


이 함수를 간단하게 설명을 하면 우선 몽고디비 서버를 열어 해당 데이터 베이스의 내용들을 삭제를 시킵니다. 그리고 그 삭제된 데이터 베이스를 가지고 ‘test_custom_key’ 라는 이름의 컬렉션을 생성합니다. 생성된 컬랙션에 데이터를 삽입하고 “aaaaaaaaaa”라는 아이디를 가지는 값을 찾습니다. 이러한 처리가 끝이 나면 그 데이터를 처리하고 DB를 close 시킵니다.
아마 이러한 코드를 여러분의 코드에서는 만나지 못할 수도 있습니다. 하지만 node.js 로 서버사이드를 처리하다 보면 의도치 않게 이러한 처리들을 하게 되는 경우가 빈번하게 있습니다.
이러한 콜백지옥에서 탈출하기 위해 2가지 해결책이 있습니다.
1.메인함수에서 익명의 함수 형태로 인자가 되어 전달되게 하지말고 함수에 이름을 줘서 변수화 시켜 그 함수의 이름을 인자로 전달을 시킵니다.
2.모듈화를 시킵니다. 특정작업을 위한 모듈로 분리시켜 필요할 때 불러서 사용하는 형태로 만듭니다.
  
마지막로 정리를 하며 콜백함수를 사용할 때 주의사항을 말씀드리면



•코드를 중복하여 사용하는 형태를 피합니다.
•추상화를 제대로 시켜 더 일반적인 형태로 다양하게 사용될 수 있도록 만듭니다.(특정 형태가 아닌 제너럴한 형태로 만듭니다.)
•유지가 잘될 수 있도록 만듭니다.
•읽기 쉽게 만듭니다.
•함수는 더욱 특정화 시킵니다.
이러한 주의사항을 지킨다면 콜백함수를 제대로 사용할 수 있을것입니다.
실제 원문에는 자신만의 callback함수 만드는 내용이 있지만 개념을 설명하기에는 이걸로 충분할듯합니다. 역시 뭐든 제대로 알고 써야한다는 생각이 듭니다.


출처: http://yubylab.tistory.com/entry/자바스크립트의-콜백함수-이해하기 [Yuby's Lab.]
