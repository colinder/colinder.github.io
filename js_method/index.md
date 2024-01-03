# JS(TS) 함수선언방식과 차이


​	

# Javascript(TS) 함수 선언 방식과 차이

TypeScript는 JavaScript를 기반으로 하는 언어이기 때문에 JavaScript의 함수 선언 방식을 따르면서 몇 가지 추가적인 기능을 제공합니다. 주요 함수 선언 방식은 다음과 같습니다.

​		

1. **함수 선언문 (Function Declaration):**

   ```typescript
   function add(a: number, b: number): number {
       return a + b;
   }
   ```

   - TypeScript에서도 JavaScript와 마찬가지로 함수 선언문을 사용할 수 있습니다.

   - 함수 선언문은 `function` 키워드로 시작하며, 함수 이름이 바로 뒤에 나옵니다.

   - 호이스팅(hoisting)에 영향을 받습니다. 즉, 선언 전에 호출할 수 있습니다.

   - 함수의 매개변수와 반환값에 타입을 명시할 수 있습니다.

     ​		

2. **함수 표현식 (Function Expression):**

   ```typescript
   const multiply = function(x: number, y: number): number {
       return x * y;
   };
   ```

   - TypeScript에서도 JavaScript의 함수 표현식을 그대로 사용할 수 있습니다.

   - 함수 표현식은 변수에 함수를 할당하는 형태입니다.

   - 호이스팅에 영향을 받습니다. 변수 선언은 호이스팅되지만 함수는 그렇지 않습니다.

   - 변수에 함수를 할당하고 타입을 명시할 수 있습니다.

     ​	

3. **화살표 함수 (Arrow Function):**

   ```typescript
   const divide = (x: number, y: number): number => x / y;
   ```

   - TypeScript에서도 JavaScript의 화살표 함수를 그대로 사용할 수 있습니다.

   - 매개변수와 반환값에 타입을 명시할 수 있습니다.

   - ES6에서 도입된 화살표 함수는 간결하고 익명 함수를 간편하게 작성할 수 있는 형태입니다.

   - 함수 내부의 `this`는 화살표 함수를 감싸고 있는 가장 가까운 함수의 `this`를 가져옵니다.

     ​	

4. **메서드 (Method):**

   ```typescript
   class Calculator {
       add(a: number, b: number): number {
           return a + b;
       }
   }
   ```

   - TypeScript에서는 클래스 내에 메서드를 정의할 수 있습니다.

   - 메서드는 클래스에 속하며, 인스턴스 또는 클래스 자체에 호출할 수 있습니다.

     ​			

5. **생성자 함수 (Constructor Function):**

   ```typescript
   class Person {
       constructor(public name: string, public age: number) {}
   }
   
   const person1 = new Person("John", 30);
   ```

   - TypeScript에서도 JavaScript와 동일하게 클래스를 이용하여 생성자 함수를 만들 수 있습니다.

   - 생성자 함수로 객체를 생성할 때 매개변수의 타입과 속성을 명시할 수 있습니다.

   - `new` 키워드를 사용하여 객체를 생성하는 함수입니다.

   - 생성된 객체의 프로토타입은 해당 생성자 함수의 프로토타입과 연결됩니다.

     ​	


TypeScript의 주요 특징은 정적 타입 검사를 통한 안정성을 제공하는 것이므로, 함수의 매개변수와 반환값에 명시적으로 타입을 지정하여 코드의 가독성과 유지보수성을 향상시킬 수 있습니다.

​	

​	

​	

​	

​	

