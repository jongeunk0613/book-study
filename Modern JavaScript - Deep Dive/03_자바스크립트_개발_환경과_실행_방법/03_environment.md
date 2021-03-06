# 자바스크립트 실행 환경

__자바스크립트는__ 자바스크립트 `엔진을 내장하고 있는` 브라우저 환경과 Node.js `환경에서 실행이 가능하다`. 하지만 둘의 용도가 다르다.  
브라우저와 Node.js 둘다 `자바스크립트 코어인 ECMAScript를 실행할 수 있지만` 브라우저와 Node.js에서 ECMAScript `이외에 추가로 제공하는 기능은 호환되지 않는다`. 

### 브라우저
HTML, CSS, JS 실행해 `웹페이지를 브라우저 화면에 렌더링` 하는 것이 주된 목적이다.  
파싱된 HTML 요소를 선택하거나 조작하는 기능의 집합인 `DOM API`를 기본적으로 제공한다.   

### Node.js
`브라우저 외부에서 자바스크립트 실행 환경을 제공`하는 것이 주된 목적이다.  
파일을 생성하고 수정할 수 있는 `파일 시스템`을 기본으로 제공한다.  

<br/>

# 웹 브라우저
ex) Google Chrome

### 개발자 도구
웹 개발에 유용한 다양한 기능을 제공한다.
![image](https://user-images.githubusercontent.com/43084680/166237398-243f9d30-8c37-4d36-a90c-47dc9c45c0d0.png)

### 콘솔
자바스크립트 코드에서 `발생한 에러를 확인`할 수 있다.  
에러가 발생했을 경우 어떤 파일, 어떤 부분에 발생했는지 확인할 수 있고 디버깅 기능을 이용해서 에러 발생 시점의 특정 값을 확인할 수 있다.  
구현 단계에서 디버깅을 위해 사용한 `console.log를 콘솔에서 확인`할 수 있다.  
__REPL(Read Eval Print Loop)__ &rarr; 자바스크립트 `코드를 직접 입력해 결과를 확인`할 수 있는 환경으로 사용할 수도 있다.  
<br/>

# Node.js
크롬 V8 자바스크립트 엔진으로 빌드된 `자바스크립트 런타임 환경`이다.  
__npm(node package manager)를__ 이용해서 사용할 수 있는 모듈들을 패키지화해서 모아둔 저장소 역할과 패키지 설치 및 관리를 위한 `CLI(Command Line Interface)를 제공`한다.  
Node.js의 __REPL를__ 이용해서 간단한 자바스크립트 `코드를 실행해 결과를 확인`해 볼 수 있다.  
<br/>

# Visual Studio Code 
코드 에디터를 사용하면 `코드 자동 완성, 문법 오류 감지, 디버깅, Git 연동` 등 강력하고 편리한 기능을 활용할 수 있다.  
`내장 터미널`을 이용해서 코드를 실행할 수 있다.  

### 확장 플러그인
- **Code Runner** &rarr; 단축키를 사용해 현재 표시 중인 자바스크립트 파일을 실행할 수 있다. 
- **Live Server** &rarr; 소스코드를 수정할 때마다 수정 사항을 브라우저에 자동으로 반영해준다.
