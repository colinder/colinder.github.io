# Django vs Node.js 질문을 받았습니다.


​		

# 🎈질문을 받았습니다.

> 저희는 이전에는 django로 풀스택 개발을 했어요.
> 개발자님은 django - vue.js로 개발을 하신다고 들었는데, vue는 javaScript 언어고 django는 Python 언어인데 둘이 뭔가 통신? 데이터를 주고 받고 하는 것에 있어서 문제는 없나요?
>
> 또 같은 javaScript 언어인 node.js 라는 것으로도 Backend 개발이 가능하다고 봤는데 node.js를 Backend로 개발하는 것이, 같은 javaScript로 개발하니까 더 효율적? 좋은거 아닌가요?

​		

### 👨‍💻 하나는 명확히 답을 드릴 수 있는데, 하나는 모르겠네요. 알아보고 말씀드리겠습니다 .

​			

1. Vue는 javaScript고 django는 Python인데 둘이 뭔가 통신? 데이터를 주고 받고 하는 것에 있어서 문제는 없나요?

   => 네 없습니다. 

   - 아마 기존에 django로 풀스택 개발한 코드를 보면 JSON 아니면, XML이라는 표기법(혹은 문법)으로 frontend와 backend 간에 데이터를 주고 받았을 것입니다.
   - 저는 JSON, XML만 알고 있어서 frontend와 backend간에 데이터 통신을 하는 표기법에 대하여 조사를 해보면 더 많은 방법이 있겠지만, 요새는 거의 JSON만 사용하는 것으로 알고 있습니다.
   - python기반의 django로 웹을 개발하더라도 frontend는 html문법으로 개발하기 때문에 python을 할 줄아는 것이 무의미합니다. 
   - 결론으로 frontend와 backend 간에 데이터를 주고 받는 것은 python이 아닌 별도의 표기법(JSON or XML 등)을 따르기 때문에 문제는 없습니다. 

     ​			

2. 같은 javaScript언어를 사용하는 node.js를 활용해 Backend개발을 하는 것이 더 효율적? 좋은거 아닌가요?

   => 어느쪽이 더 좋다는 명확한 답을 드리기 어렵습니다. (솔직히 잘 모르겠습니다.)

   - 정확하게는 node.js의 웹개발 프레임워크인 express.js랑 비교해야 할 것 같은데요. 
     자료를 찾아보니 static site는 node.js가 유리하고, dynamic site는 Django가 유리하다고 합니다. 

   - 저는 dynamic site를 CRUD가 필요한 site라고 이해하고 있습니다. 사용자 반응에 따라 다른 화면이 구성되어야 하는 site를 기획한다면, Django는 이부분에 강점이 있습니다.  (ex. instargram)

   - 반면 API와 실시간 기능이 필요하다면 이는 static site고 이때는 django보다 속도가 빠른 node.js가 유리합니다. 

   - 그런데 개인적으로 본인에게 편한 언어로 개발하는 것이 제일 유리하다고 생각합니다. 이 방법이 결국에는 가장 빠르고 효율적인 개발을 할 수 있을 것이라고 생각하기 때문입니다. (저는 python이 편합니다.ㅎ)

   - 부가적으로 사용자는 이를 고려할 필요가 없습니다. 사용자는 개발자들이 서버에서 처리한 웹페이지를 보기 때문입니다. 또 static site에도 javaScript를 활용해 동적인 처리를 하는 경우도 많아, 칼 처럼 구분지을 필요도 이유도 없을 것 같습니다.

     





 

