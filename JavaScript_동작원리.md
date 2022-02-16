# API를 이해하기 위한 JavaScript 동작원리

### API와 같이 시간이 오래 걸리는 작업을 원활하게 하기 위해서는 JavaScript의 동작원리를 알아야 한다.

#



JavaScript는 1개의 쓰레드(Stack)을 가져, 동기적으로 일을 처리한다.<br>
예를 들어, 다음과 같은 코드가 있다.

    console.log(1);
    console.log(2);
    console.log(3);

위 코드의 결과값은 다음과 같다.

    1
    2
    3

하지만 아래 코드의 결과는 어떻게 될 것인가?

    console.log(1);
    setTimeout(()=>{
        console.log(2);
    },5*1000)
    console.log(3);

신기하게도 결과값은 다음과 같다.
```javascript
1
3
//5초가 지난 뒤
2   
```

![javascript runtime](images/javascript/runtime.png)

이는 서두에 얘기한대로, Javascript는 한 번에 하나의 일만 처리할 수 있는 싱글쓰레드(Single Thread)이기 때문이다. 위 그림과 같이 JavaScript 코드가 실행되면 스택에 새로운 프레임이 들어오고(그림속 function1, function2) 완료되면 바로 나간다.<br><br>
이 때 setTimeout, Event handler(EventListener 등), Ajax 등 처리 시간이 오래걸리는 Web APIs로부터 Tesk를 받으면 큐(Queue)에 저장한다.
### Web APIs
 - 자바스크립트의 스택이 아닌, 별도의 스택에서 작업이 이루어진다.
 - setTimeOut(함수,시간): 시간만큼 코드를 딜레이시키고 함수를 실행한다. 시간은 참고로 마이크로세컨드 단위이기 때문에 1초는 1000ms이다
 - Ajax, Axios, fetch: 클라이언트와 서버간에 데이터를 주고받는 기술
 - Event Handler : 클릭과같은 이벤트를 핸들하는 함수들
<br><br>

![queue](/images/javascript/큐.png)
: Queue는 눈치를 많이 보는 애라, Event Loop를 통해 Stack이 비었는지 확인하고, 비어있다면 Event Loop를 통해 아이템을 꺼내 Stack에 이동시킨다. Event Loop는 스택과 큐 사이에서 흐름을 제어한다고 볼 수 있다.


### 위 예제에서 console.log(2)를 실행해야만 console.log(3)가 실행될 수 있다면, console.log(3)은 2가 실행될 5초를 함께 기다려야 한다. 이것처럼 API호출 이후 일어나야할 Stack은 'async','await','fetch'로 미룰 수 있다.