# Nest Official_01_(Introduction, First steps, Controllers, Providers)


​	

# Introduction

Nest.js 는 효율적이고 확장가능한 node.js SSR을 구축하기 위한 프레임워크.

Nest.js는 progressive(점진적인) JavaScript를 사용하며.<span style="font-size: 0.8em; color: gray"> (progressive JavaScript; 웹과 네이티브 엡을 모두 대응하는 등 점진적인 방법론을 javascript를 통해 개발하는 개념.)</span> TypeScript를 지원(javascript로 개발도 가능) 그리고 OOP(객체 지향 프로그래밍), FP(기능적 프로그래밍), FRP(기능적 반응 프로그래밍)의 요소들을 결합합니다.

후드 아래에서 Nest는 Express(기본값)와 같은 강력한 HTTP 서버 프레임워크를 사용하며 Fastify를 사용하도록 구성할 수도 있습니다!

Nest는 이러한 일반적인 Node.js 프레임워크(Express/Fastify) 이상의 추상화 수준을 제공하지만 개발자에게 API를 직접 노출시킵니다. 이를 통해 개발자는 기본 플랫폼에 사용할 수 있는 수많은 타사 모듈을 사용할 수 있습니다.

​		

## Philosophy

최근 몇년, node.js 덕분에 javascript는 프론트와 백엔드 애플리케이션 모두의 “lingua franca”가 되었습니다. <span style="font-size: 0.8em; color: gray">(lingua franca: 서로 다른 모어를 사용하는 화자들이 의사소통을 하기 위해 국제어)</span> 이것은 개발자의 생산성을 향상시키고 빠르고 테스트 가능하며 확장 가능한 프론트엔드 애플리케이션의 생성을 가능하게 하는 Angular, React 및 Vue와 같은 멋진 프로젝트를 탄생시켰습니다. 그러나 Node(그리고 서버측 자바스크립트)를 위한 훌륭한 라이브러리, 도우미 및 도구는 많이 존재하지만, 그 중 어느 것도 아키텍처의 주요 문제를 효과적으로 해결하지는 못합니다.

Nest는 Out-of-the-box <span style="font-size: 0.8em; color: gray">(Out-of-the-box: 해당 제품이나 서비스를 추가적인 구성이나 변경 없이도 바로 사용할 수 있는 상태)</span> 애플리케이션 아키텍처를 제공하여 개발자와 팀이 고도로 테스트 가능하고 확장 가능하며 느슨하게 결합되고 쉽게 유지 관리할 수 있는 애플리케이션을 만들 수 있습니다. 이 아키텍처는 Angular에서 많은 영감을 받았습니다.

​	

​	

# Overview

## First step

### step

설치된 NPM을 활용해 Nest CLI로 새로운 프로젝트를 실행하는 것은 쉽습니다. 

```
$ npm i -g @nestjs/cli
$ nest new project-name
```

> **HINT**
> To create a new project with TypeScript's [stricter](https://www.typescriptlang.org/tsconfig#strict) feature set, pass the `--strict` flag to the `nest new` command.



'project-name' 디렉토리가 생성되고, 노드 모듈 및 기타 몇 개의 보일러 플레이트 파일이 설치되며, 'src/' 디렉토리가 생성되고 여러 개의 코어 파일로 채워집니다.

src</br>
 └ app.controller.spec.ts</br>
 └ app.controller.ts</br>
 └ app.module.ts</br>
 └ app.service.ts</br>
 └ main.ts</br>

​	

다음은 이러한 핵심 파일에 대한 간략한 개요입니다:

|                          |                                                              |
| ------------------------ | ------------------------------------------------------------ |
| `app.controller.ts`      | A basic controller with a single route.                      |
| `app.controller.spec.ts` | The unit tests for the controller.                           |
| `app.module.ts`          | The root module of the application.                          |
| `app.service.ts`         | A basic service with a single method.                        |
| `main.ts`                | The entry file of the application which uses the core function `NestFactory` to create a Nest application instance. |

​		

main.ts에는 비동기 기능이 포함되어 있어 애플리케이션을 부트스트랩(초기화)합니다:

```typescript
// main.ts

import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

Nest 응용 프로그램 인스턴스를 생성하기 위해 코어 `NestFactory 클래스`를 사용합니다. `NestFactory`는 응용 프로그램 인스턴스를 생성할 수 있는 몇 가지 정적 메서드를 공개합니다. 

`create()` 메서드는 INestApplication 인터페이스를 충족하는 응용 프로그램 객체를 반환합니다. 이 객체는 다음 장에서 설명하는 일련의 메서드를 제공합니다. 위의 main.ts 예제에서는 간단히 HTTP 수신기를 시작하여 응용 프로그램이 인바운드 HTTP 요청을 대기하도록 합니다.

Nest CLI로 프로젝트를 생성하면 각 모듈을 별도의 디렉터리에 유지하는 규칙을 따르도록 하는 초기 프로젝트 구조가 생성됩니다.

​				

### Running the application

설치 프로세스가 완료되면 OS 명령 프롬프트에서 다음 명령을 실행하여 인바운드 HTTP 요청에 대한 응용 프로그램 수신을 시작할 수 있습니다:

```
$ npm run start
```

>  **HINT**
>  To speed up the development process (x20 times faster builds), you can use the [SWC builder](https://docs.nestjs.com/recipes/swc) by passing the `-b swc` flag to the `start` script, as follows `npm run start -- -b swc`.



여러분의 파일이 변경되는 것을 바로 확인하고 싶다면, 아래 명령어로 실행하세요

```
$ npm run start:dev
```

이 명령어는 파일의 변화를 추적할 것이며, 서버를 자동으로 재컴파일하고, 다시 실행 합니다.

​			

### Linting and formatting

CLI는 최선을 다해 규모에 따라 신뢰할 수 있는 개발 워크플로우의 기틀을 제공합니다. 
따라서, 생성된 Nest 프로젝트는 코드 라이너와 포맷터(각각 에슬린트와 더 예쁜)가 미리 설치되어 있습니다.

> **HINT**
> Not sure about the role of formatters vs linters? Learn the difference [here](https://prettier.io/docs/en/comparison.html).

최대한의 안정성과 확장성을 보장하기 위해 기본 에슬린트와 더 예쁜 클리 패키지를 사용합니다. 이 설정을 통해 디자인에 의한 공식 확장 기능과 깔끔한 IDE 통합이 가능합니다.

IDE와 관련이 없는 헤드리스 환경(Continuous Integration, Git hook 등)의 경우 Nest 프로젝트는 즉시 사용할 수 있는 스크립트와 함께 제공됩니다.

```typescript
// Lint and autofix with eslint
$ npm run lint

// Format with prettier
$ npm run format
```

​		

## Controllers

> 요청을 처리하고 클라이언트에 대한 응답을 반환하는 역할을 합니다.

컨트롤러의 목적은 애플리케이션에 대한 특정 요청을 수신하는 것입니다. 라우팅 메커니즘은 어떤 컨트롤러가 어떤 요청을 수신하는지를 제어합니다. 종종, 각각의 컨트롤러는 하나 이상의 루트를 가지며, 다른 루트는 다른 동작을 수행할 수 있습니다.

기본 컨트롤러를 만들기 위해 클래스와 데코레이터를 사용합니다. 데코레이터는 클래스를 필요한 메타데이터와 연결하고 Nest가 라우팅 맵(해당 컨트롤러에 요청을 연결)을 만들 수 있도록 합니다.

​	

​		

### Routing	

아래 예시에서 기본 컨트롤러를 정의하는 데 *필수인* `@Controller()` 데코레이터를 사용할 것입니다. cat을 route path의 prefix로 지정합니다. `@Controller()` 데코레이터에서 path prefix를 사용하면 관련된 일련의 경로를 쉽게 그룹화할 수 있고 반복되는 코드를 최소화할 수 있습니다.
예를 들어, 우리는 경로 `/cats` 아래에서 cat 엔티티와의 상호 작용을 관리하는 path 집합을 그룹화할 수 있습니다. 해당 경우, 우리는 파일의 각 경로에 대해 경로의 그 부분을 반복할 필요가 없도록 `@Controller()` 데코레이터에서 path perfix를 `'cats'`으로 지정할 수 있습니다.1️⃣

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('cats') //1️⃣
export class CatsController {
  @Get()
  findAll(): string { //2️⃣
    return 'This action returns all cats';
  }
}
```

> **HINT**
> CLI를 사용해서 컨드롤러를 생성하려 한다면, 간단히 `$ nest g controller [name]` 명령어를 실행하면 됩니다.

findAll() 메서드 앞에 있는 `@Get()` HTTP request method decorator는 Nest에게 HTTP request에 대한 특정 엔드포인트에 대한 핸들러를 작성하도록 지시합니다.
엔드포인트는 HTTP 요청 방식(이 경우 GET) 및 `route path`에 해당합니다.

​			

그런데 `route path`는 무엇일까요?

핸들러(연결자)인 route path는 컨트롤러에 의해 선언된 perfix(선택사항 임)와 동작할 함수 데코레이터에 지정된 경로를 연결하여 완성됩니다. (아래 예시 글을 보며 이해해봅시다.)

route를 'cats'로 선언했고 이후에 다른 route 정보를 데코레이터에 선언하지 않았기 때문에, Nest는 `/cats`으로 접수된 GET 요청을 연결합니다. 언급한 바와 같이 path는 개발자가 의도적으로 작성한 perfix 경로 컨트롤러와 함수 데코레이터에 선언된 임의의 경로 문자열을 모두 포함(동일한 것으로 간주)합니다.

예를 들어, @Controller('cats')안에 데코레이터 @Get('breed')를 같이 사용하면  `/cats/breed`와 같은 GET 요청에 대한 경로 매핑을 생성합니다.

```typescript
import { Controller, Get } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
  @Get('bread')
  OMG(): string {
    return 'This breadbreadbreadbread';
  }
}
```

위의 예제에서 이 엔드포인트에 GET 요청이 있을 때 Nest는 요청을 사용자 정의 findAll() 메서드로 라우팅합니다. 2️⃣

여기서 우리가 선택하는 메서드 이름(findAll(), OMG()...)은 완전히 임의적입니다. 경로를 바인딩할 메서드를 분명히 선언해야 하지만 Nest는 선택한 메서드 이름에 아무런 의미를 부여하지 않습니다. (그냥 만들고 싶은 이름으로 만들면 됩니다.)

​			

이 함수는 HTTP response 200 state를 반환하는데, 단순히 문자열에 불과합니다. 왜 이렇게 될까요? 설명하기 위해 Nest의 두 가지 다른 옵션을 사용하는 개념을 소개합니다.

|                        |                                                              |
| ---------------------- | ------------------------------------------------------------ |
| Standard (recommended) | 내장 함수를 사용하는 경우 핸들러는 자바스크립트의 객체 또는 배열을 반환하는데 이는 자동으로 JSON으로 직렬화 됩니다. 그러나 자바스크립트의 원시타입(e.g., 문자열, 숫자형, 참거짓)을 반환하는 경우, Nest는 직렬화하지 않고 값만 반환합니다. 이렇게 하면 응답 관리가 간단해집니다: 간단히 값만 반환하면되고, 나머지는 Nest가 처리합니다. (e.g.,상태 코드) <br />또 상태 코드의 응답은 기본적으로 200이 default입니다. POST는 201을 사용합니다. 상태 코드를 바꿔서 반환(응답)하고 싶다면, '@HttpCode(...)' 데코레이터를 사용하면 됩니다. |
| Library-specific       | 우리는 method handler signature 기술인 `@Res() 데코레이터`를 사용하여 라이브러리별 (e.g., Express) 응답 객체를 사용할 수도 있습니다. (e.g., findAll(@Res() response))<br />이 방법을 사용하면 해당 객체에서 노출된 기본 응답 처리 메서드를 사용할 수 있습니다. 예를 들어 Express를 사용하면 response.status(200).send()와 같은 코드를 사용하여 응답을 구성할 수 있습니다. |

> **WARNING**
> Nest detects when the handler is using either `@Res()` or `@Next()`, indicating you have chosen the library-specific option. If both approaches are used at the same time, the `Standard approach is automatically disabled` for this single route and will no longer work as expected. To use both approaches at the same time (for example, by injecting the response object to only set cookies/headers but still leave the rest to the framework), you must set the `passthrough` option to `true` in the `@Res({ passthrough: true })` decorator.

​		

### Request object

핸들러는 클라이언트 요청에 대한 상세한 정보에 접근할 필요가 있는 경우가 많습니다. Nest는 기본 플랫폼의 요청 객체에 대한 접근을 제공합니다(Express by default). 핸들러의 시그니처에 @Req() 데코레이터를 추가하여 Nest에 지시함으로써 요청 객체에 접근할 수 있습니다.

```typescript
import { Controller, Get, Req } from '@nestjs/common';
import { Request } from 'express';

@Controller('cats')
export class CatsController {
  @Get()
  findAll(@Req() request: Request): string {
    console.log(request)   // 👈
    return 'This action returns all cats';
  }
}
```

> **HINT**
> In order to take advantage of `express` typings (as in the `request: Request` parameter example above), install `@types/express` package.

요청 개체는 HTTP 요청을 나타내며 요청 쿼리 문자열, 매개 변수, HTTP 헤더 및 본문에 대한 속성을 가집니다(read more [here](https://expressjs.com/en/api.html#req)). 대부분의 경우, 이런 속성들을 수동적으로 확인하는 것은 필요하지 않습니다. 우리는 특정 데코레이터를 사용할 수 있습니다. 예를 들어  `@Body()` or `@Query()`, 같은 것들을 별도의 설치 없이(out of the box) 사용할 수 있습니다. 아래 리스트들은 Nest에서 제공하는 데코레이터이며, 간단하게 사용 가능한 것들 입니다.

|                           |                                     |
| ------------------------- | ----------------------------------- |
| `@Request(), @Req()`      | `req`                               |
| `@Response(), @Res()`     | `res`                               |
| `@Next()`                 | `next`                              |
| `@Session()`              | `req.session`                       |
| `@Param(key?: string)`    | `req.params` / `req.params[key]`    |
| `@Body(key?: string)`     | `req.body` / `req.body[key]`        |
| `@Query(key?: string)`    | `req.query` / `req.query[key]`      |
| `@Headers(name?: string)` | `req.headers` / `req.headers[name]` |
| `@Ip()`                   | `req.ip`                            |
| `@HostParam()`            | `req.hosts`                         |

HTTP 플랫폼(e.g., Express and Fastify) 간의 typings 호환성을 위해, Nest는 `@Res()` and `@Response()` 데코레이터를 제공합니다. `@Res()`는 `@Response()`의 별칭일 뿐입니다. 이 두 데코레이터는 개발자에게 HTTP 플랫폼의 기본 응답 객체에 직접 접근할 수 있게 해줍니다. 이것들을 사용할 때 모든 이점을 얻기 위해서는 당신은 라이브러리를 import해서 사용해야 합니다. 

`@Res()` 또는 `@Response()`를 메서드 핸들러에 주입할 때는 해당 핸들러에 대해 Nest를 라이브러리별 모드로 전환하고 응답을 관리하는 책임이 당신에게 있음을 주의하세요. 이렇게 할 때는 'response' 개체(e.g., `res.json(...)` or `res.send(...)`)를 호출하여 어떤 종류의 응답을 발행해야 합니다. 그렇지 않으면 HTTP 서버가 중단됩니다.

> **HINT**
> To learn how to create your own custom decorators, visit [this](https://docs.nestjs.com/custom-decorators) chapter.

​		

### Resources

앞서 우리는 cats 리소스를 가져올 엔드포인트(**GET** route)에 대하여 정의 했습니다. 또한 일반적으로 새 레코드를 생성하는 엔드포인트를 제공하고자 합니다. 이를 위해 **POST** 핸들러를 만들어 보겠습니다:

```typescript
// cats.controller.ts

import { Controller, Get, Post } from '@nestjs/common';

@Controller('cats')
export class CatsController {
  @Post()
  create(): string {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(): string {
    return 'This action returns all cats';
  }
}
```

간단하죠. Nest는 모든 기본 HTTP 메서드를 위한 데코레이터를 제공합니다. `@Get()`, `@Post()`, `@Put()`, `@Delete()`, `@Patch()`, `@Options()`, and `@Head()`. 또 `@All()`은 이 모든 데코레이터를 처리하는 엔드포인트 입니다.

> 예를 들어 
> /cats 에 대하여 @Get(), @Post() 두 메서드만 정의했고 @All()을 추가로 정의한 경우, 
> /cats으로 `@Put()`, `@Delete()`, `@Patch()`, `@Options()`, and `@Head()` 요청을 보내면 `@All()`메서드가 응답합니다.

​		

### Route wildcards[#](https://docs.nestjs.com/controllers#route-wildcards)

패턴에 기반한 routes도 지원합니다. 예를 들어 asterisk(*)는 와일드카드로 사용되며 모든 문자 조합과 일치합니다.

```typescript
@Get('ab*cd')
findAll() {
  return 'This route uses a wildcard';
}
```

`'ab*cd'` route path는 `abcd`, `ab_cd`, `abecd` 등과 연결됩니다. `?`, `+`, `*` 및 `()` 문자는 route path에 사용될 수 있으며 정규식 대응의 하위 집합입니다. 하이픈(`-`)과 점(`.`)은 문자열 기반 경로로 문자 그대로 해석됩니다.

> **WARNING**
> A wildcard in the middle of the route is only supported by express.

​			

### Status code[#](https://docs.nestjs.com/controllers#status-code)

이전에 언급했듯이, status code는 기본적으로 항상 200이 기본값입니다. POST는 예외적으로 201입니다. 이는 간단히 바꿀 수 있는데 핸들러 작성 단계에서 `@HttpCode(...)`를 추가 하면 됩니다. 

```typescript
import { Post, HttpCode } from '@nestjs/common';

@Post()
@HttpCode(204)
create() {
  return 'This action adds a new cat';
}
```

> **HINT**
> Import `HttpCode` from the `@nestjs/common` package.

> **WARNING**
> `@All()` 이후 `@Post()`를 작성하면 호이스팅이 되지 않는다!  우선 순위를 고려하면 개발해야함.

​			

### Headers[#](https://docs.nestjs.com/controllers#headers)

특별히 응답 header를 커스텀하고 싶다면, `@Header()`데코레이터나, 라이브러리별 응답 개체를 사용하고  `res.header()`를 직접 호출하면 됩니다.

```typescript
import { Post, Header } from '@nestjs/common';

@Post()
@Header('Cache-Control', 'none')
create() {
  return 'This action adds a new cat';
}
```

​		

### Redirection[#](https://docs.nestjs.com/controllers#redirection)

특정 URL로 리다이렉션 하기 위해서는 `@Redirect()`데코레이터나, 라이브러리별 응답 객체를 사용하고, `res.redirect()`를 직접 호출하면 됩니다.

`@Redirect()` 는 두개의 인자가 필요합니다., `url`과 `statusCode`, 둘 다 필수값은 아니며, 생략할 경우 기본 status code는 302입니다.

```typescript
@Get()
@Redirect('https://nestjs.com', 301)
```

> **HINT**
> 가끔씩 동적으로 HTTP 상태 코드나 리디렉션 URL을 보내고 싶을 때가 있을 것입니다. 이럴 땐, (@nestjs/common의) HttpRedirectResponse 인터페이스를 따르는 객체를 반환하면 됩니다.

메서드가 return 값(e.g, URL)을 반환하면, 이 값은 데코레이터에 전달된 기존의 인수를 오버라이드(덮어쓰기)한다. 예를 들어

```typescript
@Get('docs')
@Redirect('https://docs.nestjs.com', 302)
getDocs() {  
  return { url: 'https://naver.com' };
}
```

해당 경우 응답이 naver로 덮어써진다.

​			

### Route parameters[#](https://docs.nestjs.com/controllers#route-parameters)

요청의 일부로 동적 데이터를 수신할 필요가 있을 때,정적  path의 routes는 동작하지 않습니다. (e.g., `GET /cats/1` to get cat with id `1`). 매개변수가 있는 routes를 정의하기 위해, routes에 매개변수 `tokens`을 추가하여 요청 URL의 해당 위치에서 동적값을 활용할 수 있습니다. 

`@Get()`데코레이터안에 route 매개변수 token를 사용한 아래의 예시를 봅시다. route 매개변수가 아래와 같이 선언되었다면, `@Param()` 데코레이터를 사용해서 접근하고 사용할 수 있습니다. 

> **HINT**
> route 매개변수를 사용할 때, 매개변수가 포함된 route는 정적 path를 선언한 후에 작성되어야 한다. 
> 이렇게 하면, 정적 path의 트래픽이 매개변수가 담긴 path에 의해 방해받는 것을 방지할 수 있다.
>
> 예를 들어 /users/profile, /users/:id 두개의 라우트가 선언 되어 있을 때 '/users/profile' 먼저 선언되어 있어야한다. 그렇지 않으면, :id 파라미터가 "profile"이라는 값을 캡쳐하게 되어, 의도하지 않은 동작을 유발할 수 있다.

```typescript
import { Get, Param } from '@nestjs/common';

@Get(':id')
findOne(@Param() params: any): string {
  console.log(params.id);
  return `This action returns a #${params.id} cat`;
}
```

`@Param()`은 메서드 매개변수(위 예시의 'params')를 장식하는 데 사용된다. 이렇게 하면 라우트 파라미터가 메서드 함수 내에서 변수의 속성으로 사용할 수 있습니다. 위 코드에서 볼 수 있듯이, params.id를 참조하여 id 매개변수에 접근할 수 있다. 데코레이터에 특정 파라미터 토큰을 전달하면, 메서드 본문에서 route 매개변수를 이름으로 직접 참조할 수 있습니다.

```typescript
import { Get, Param, Bind } from '@nestjs/common';

@Get(':id')
@Bind(Param('id'))
findOne(id) {
  return `This action returns a #${id} cat`;
}
```

단, @Get(':id') / @Bind(Param('id')) 와 같이 @Get(`':'`)의 `:`뒤에 문자와 @Bind(Param(`''`)) `''`안의 문자가 동일해야 합니다.

또 아래와 같이 사용할 수 있습니다.

```typescript
import { Get, Param } from '@nestjs/common';

@Get(':id')
findOne(@Param('id') id: string): string {
  return `This action returns a #${id} cat`;
}
```

​			

### Sub-Domain Routing[#](https://docs.nestjs.com/controllers#sub-domain-routing)

`@Controller` 데코레이터는 호스트 옵션을 사용하여 들어오는 요청의 HTTP 호스트가 특정 값과 일치하도록 요구할 수 있습니다.

```typescript
@Controller({ host: 'admin.example.com' })
export class AdminController {
  @Get()
  index(): string {
    return 'Admin page';
  }
}
```

> **WARNING**
> Since **Fastify** lacks support for nested routers, when using sub-domain routing, the (default) Express adapter should be used instead.

route path와 마찬가지로 `hosts` 옵션은 토큰을 사용하여 호스트 이름에서 해당 위치의 동적 값을 캡처할 수 있습니다. 아래 `@Controller()` 데코레이터 예제의 호스트 매개변수 토큰은 이 사용법을 보여줍니다. 이런 방법으로 선언된 호스트 매개변수는 `@HostParam()` 데코레이터를 사용하여 접근할 수 있으며, 이 데코레이터는 메서드 시그니처에 추가되어야 합니다.

```typescript
@Controller({ host: ':account.example.com' })
export class AccountController {
  @Get()
  getInfo(@HostParam('account') account: string) {
    return account;
  }
}
```

​			

### Scopes[#](https://docs.nestjs.com/controllers#scopes)

Nest에서는 거의 모든 것이 들어오는 요청 간에 공유된다. 예를 들어 데이터베이스에 대한 연결 풀, 글로벌 상태의 singleton services(싱글톤 서비스) 등이 있습니다. Node.js는 모든 요청이 별도의 스레드에 의해 처리되는 요청/응답 다중 스레드 상태 비저장 모델을 따르지 않는다는 점을 기억하십시오. 따라서 싱글톤 인스턴스를 사용하는 것은 우리의 응용 프로그램에 완전히 안전합니다.

> singleton services
>
> 객체의 인스턴스가 오직 1개만 생성하여 사용하는 것을 의미합니다. 생성자를 여러 번 호출하더라고 실제 생서되는 각체는 하나이며 최초로 생성된 이후에 호출된 생성자는 이미 생성한 객체를 반환시키도록 만드는 것입니다. 즉, 어플리케이션이 시작될 때 어떤 클래스가 최로 한 번만 메모리를 할당(static)하고 그 메모리에 인스턴스를 만들어 사용하는 서비스입니다. 
>
> ```typescript
> var SingletonService = (function () {
>  // 1. private 변수
>  var instance;
> 
>  // 2. private 생성자
>  function SingletonService() {
>      // 객체 초기화 코드
>  }
> 
>  // 3. 싱글톤 객체 생성 또는 기존 객체 반환
>  function createInstance() {
>      if (!instance) {
>          instance = new SingletonService();
>      }
>      return instance;
>  }
> 
>  // 4. 외부에서 접근 가능한 메서드 (public method)
>  return {
>      getInstance: function () {
>          return createInstance();
>      }
>  };
> })();
> 
> // 테스트
> var singletonA = SingletonService.getInstance();
> var singletonB = SingletonService.getInstance();
> 
> console.log(singletonA === singletonB); // true, 같은 인스턴스를 공유하고 있음
> ```

​		

### Asynchronicity[#](https://docs.nestjs.com/controllers#asynchronicity) (비동기성)

우리는 최신 JavaScript를 사랑하며 데이터 추출이 대부분 **비동기식**이라는 것을 알고 있습니다. 그렇기 때문에 Nest는 **비동기** 함수를 지원하고 잘 작동합니다.

>**HINT**
>Learn more about `async / await` feature [here](https://kamilmysliwiec.com/typescript-2-1-introduction-async-await)

​				

모든 비동기 함수는 `프로미스` 타입을 반환해야 합니다. 이것은 Nest가 스스로 해결할 수 있는 연기된(deferred) 값을 반환할 수 있다는 것을 의미합니다. 이에 대한 예시를 살펴보겠습니다.

```typescript
@Get()
async findAll(): Promise<any[]> {
  return [];
}
```

위의 코드는 완전히 유효합니다. 더욱 강력한 것은 Nest 라우트 핸들러가 *<u>RxJS [observable streams](http://reactivex.io/rxjs/class/es6/Observable.js~Observable.html)</u>을 반환할 수 있다는 점입니다. Nest는 자동으로 소스에 구독하고 스트림이 완료되면 마지막으로 전달된 값을 가져옵니다.

```typescript
@Get()
findAll(): Observable<any[]> {
  return of([]);
}
```

위의 두 가지 접근 방식은 모두 작동하며 요구 사항에 맞는 모든 것을 사용할 수 있습니다.

<span style="font-size: 0.8em; color: gray">*RxJS(Reactive Extensions for JavaScript)는 자바스크립트에서 반응형 프로그래밍을 구현하기 위한 라이브러리입니다. "Observable"은 RxJS에서 중요한 개념 중 하나로, 이는 비동기적인 이벤트나 데이터 스트림을 다루는데 사용됩니다. 
Observable은 데이터나 이벤트의 스트림을 나타내는데, 이 스트림은 시간에 따라 여러 값을 방출할 수 있습니다. 이러한 값을 Observer가 관찰하고, 스트림의 상태에 따라 특정 작업을 수행할 수 있습니다. 간단히 말해, RxJS observable은 비동기적인 이벤트 또는 데이터 스트림을 나타내며, 해당 스트림에서 발생하는 변화에 대응하여 작업을 수행할 수 있는 방법을 제공합니다. Nest.js와 같은 프레임워크에서는 이러한 Observable을 사용하여 비동기적인 작업을 효율적으로 다룰 수 있습니다.</span>

​			

### Request payloads[#](https://docs.nestjs.com/controllers#request-payloads)

이전에 예를 보면 POST 요청의 처리에서 클라이언트 데이터를 받지 못했습니다. 여기에 `@Body()`데코레이터를 추가해 수정해보겠습니다.

그런데 먼저 (당신이 typescript를 사용한다면) 우리는 DTO(Data Transfer Object) 스키마를 결정해야 합니다. DTO는 데이터가 네트워크를 통해 전송되는 방식을 정의하는 개체입니다. TypeScript 인터페이스를 사용하거나 간단한 클래스를 사용하여 DTO 스키마를 결정할 수 있습니다. 흥미롭게도 여기서는 **클래스를 사용하는 것을 권장합니다. 왜인가요? **클래스는 자바스크립트 ES6 표준의 일부이므로 컴파일된 자바스크립트에서 실제 엔티티로 보존됩니다. 반면에 TypeScript 인터페이스는 컴파일 중에 제거되므로 Nest는 실행중일 때 해당 인터페이스를 참조할 수 없습니다. <u>*Pipes</u>와 같은 기능이 런타임에 변수의 메타타입에 액세스할 수 있을 때 추가적인 가능성을 지원하기 때문에 중요합니다.

<span style="font-size: 0.8em; color: gray">*Nest.js의 Pipes는 입력 데이터의 변환 또는 유효성 검사와 같은 특정 작업을 수행하는데 사용됩니다. Pipes는 메타데이터(metatype)에 접근할 수 있어야 하는 경우가 있는데, 이는 런타임(runtime)에서 변수의 유형 및 다른 정보를 알아내는 것을 의미합니다. 메타데이터를 사용하면 Pipes가 동적으로 동작하고 입력 데이터에 대해 적절한 작업을 수행할 수 있습니다. 예를 들어, 데이터 유효성 검사를 수행하는 Pipe는 입력 데이터의 유형이나 기타 속성을 알아야 합니다. 이를 위해 Pipes는 런타임에서 변수의 메타데이터를 확인할 수 있어야 합니다.</span>

Let's create the `CreateCatDto` class:

```typescript
export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}
```

It has only three basic properties. Thereafter we can use the newly created DTO inside the `CatsController`:

기본 속성은 세 가지뿐입니다. 이제 `CatsController`안에서 새로 만든 DTO를 사용할 수 있습니다:

```typescript
@Post()
async create(@Body() createCatDto: CreateCatDto) {
  return 'This action adds a new cat';
}
```

> **HINT**
> Our `ValidationPipe`는 메서드 핸들러가 수신해서는 안 되는 속성을 필터링할 수 있다. 이 경우 허용되는 속성을 화이트리스트에 추가할 수 있으며, 화이트리스트에 포함되지 않은 속성은 결과 개체에서 자동으로 제거된다. In the `CreateCatDto` example, our whitelist is the `name`, `age`, and `breed` properties. Learn more [here](https://docs.nestjs.com/techniques/validation#stripping-properties).

​		

### Handling errors[#](https://docs.nestjs.com/controllers#handling-errors)

There's a separate chapter about handling errors (i.e., working with exceptions) [here](https://docs.nestjs.com/exception-filters).

​			

### Full resource sample[#](https://docs.nestjs.com/controllers#full-resource-sample)

아래는 사용 가능한 여러 데코레이터를 사용하여 기본 컨트롤러를 만드는 예입니다. 이 컨트롤러는 내부 데이터에 접근하고 조작할 수 있는 몇 가지 방법을 공개합니다.

```typescript
import { Controller, Get, Query, Post, Body, Put, Param, Delete } from '@nestjs/common';
import { CreateCatDto, UpdateCatDto, ListAllEntities } from './dto';

@Controller('cats')
export class CatsController {
  @Post()
  create(@Body() createCatDto: CreateCatDto) {
    return 'This action adds a new cat';
  }

  @Get()
  findAll(@Query() query: ListAllEntities) {
    return `This action returns all cats (limit: ${query.limit} items)`;
  }

  @Get(':id')
  findOne(@Param('id') id: string) {
    return `This action returns a #${id} cat`;
  }

  @Put(':id')
  update(@Param('id') id: string, @Body() updateCatDto: UpdateCatDto) {
    return `This action updates a #${id} cat`;
  }

  @Delete(':id')
  remove(@Param('id') id: string) {
    return `This action removes a #${id} cat`;
  }
}
```

> **HINT**
> Nest CLI는 모든 보일러 플레이트 코드를 자동으로 생성하는 발전기(도형)를 제공하여 이 모든 것을 방지하고 개발자가 훨씬 쉽게 경험할 수 있도록 도와줍니다. 이 기능에 대한 자세한 내용은  [here](https://docs.nestjs.com/recipes/crud-generator)에서 확인하십시오.

​			

### Getting up and running[#](https://docs.nestjs.com/controllers#getting-up-and-running)

위 컨트롤러가 완전히 정의된 상태에서 Nest는 여전히 CatsController가 존재한다는 것을 알지 못하며 결과적으로 이 클래스의 인스턴스를 만들지 않습니다.

컨트롤러는 항상 모듈에 속하기 때문에, 이는 왜 우리가  `@Module()`안에 `controllers`들을 넣어야 하는 가의 답입니다. 우리는`AppModule`을 제외한 어떤 모듈도 정의하지 않았기 때문에,  `CatsController`를 사용해서 정의해보겠습니다.

```typescript
// app.module.ts

import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';

@Module({
  controllers: [CatsController],
})
export class AppModule {}
```

`@Module()` 데코레이터를 이용하여 모듈 클래스에 메타데이터를 연결했으며, Nest는 탑재해야 하는 컨트롤러를 쉽게 반영할 수 있게 되었습니다.

​	

### Library-specific approach[#](https://docs.nestjs.com/controllers#library-specific-approach)

지금까지 우리는 Nest 표준 응답 조작 장법에 대하여 이야기했습니다. 응답을 다루는 두 번째 방법은 라이브러리 별 응답 개체(library-specific [response object](https://expressjs.com/en/api.html#res))를 사용하는 것입니다. 특정 응답 객체를 주입하기 위해서는 `@Res()`데코레이터를 사용하면 됩니다. 

차이점을 보이기 위해 `CatsController` 를 다시 작성해보겠습니다.

```typescript
import { Controller, Get, Post, Res, HttpStatus } from '@nestjs/common';
import { Response } from 'express';

@Controller('cats')
export class CatsController {
  @Post()
  create(@Res() res: Response) {
    res.status(HttpStatus.CREATED).send();
  }

  @Get()
  findAll(@Res() res: Response) {
     res.status(HttpStatus.OK).json([]);
  }
}
```

비록 이 방식이 작동하고 실제로 응답 객체에 대한 완전한 제어를 제공하여 헤더 조작, 라이브러리별 기능 등에서 어느 정도의 유연성을 허용하지만, 이는 주의해서 사용해야 합니다.

일반적으로 이 방식은 훨씬 명확하지 않으며 몇 가지 단점이 있습니다. 주요 단점은 코드가 플랫폼에 의존적이 되며(기본 라이브러리에서 응답 객체에 대한 API가 다를 수 있음), 테스트하기 어려워진다는 것입니다(응답 객체를 목업해야 할 필요가 있습니다 등).

또한 위의 예제에서는 Nest 표준 응답 처리에 의존하는 Interceptor 및 `@HttpCode()` / `@Header()` 데코레이터와 같은 Nest 기능과의 호환성이 손실됩니다. 이를 해결하려면 다음과 같이 `passthrough` 옵션을 `true`로 설정할 수 있습니다:

```typescript
@Get()
findAll(@Res({ passthrough: true }) res: Response) {
  res.status(HttpStatus.OK);
  return [];
}
```

이제 특정 조건에 따라 쿠키나 헤더를 설정하는 등 네이티브 응답 객체와 상호 작용할 수 있지만, 나머지는 프레임워크에 맡겨둘 수 있습니다.

​	

​	

## Providers

프로바이더는 Nest에서의 기본적인 개념 중 하나입니다. 많은 기본 Nest 클래스는 프로바이더로 취급될 수 있습니다 - 서비스, 리포지토리, 팩토리, 헬퍼 등입니다. 프로바이더의 주요 아이디어는 이를 의존성으로 주입할 수 있다는 것입니다. 이는 객체들이 서로 다양한 관계를 생성할 수 있고, 이러한 객체들을 '연결'하는 기능을 대부분 Nest 런타임 시스템에 위임할 수 있다는 것을 의미합니다.

이전 장에서 우리는 간단한 CatsController를 만들었습니다. 컨트롤러는 HTTP 요청을 처리하고 더 복잡한 작업을 프로바이더에게 위임해야 합니다. 프로바이더는 모듈에서 프로바이더로 선언된 일반적인 JavaScript 클래스입니다.

> **HINT**
> Nest는 의존성을 더 객체지향적인 방식으로 설계하고 구성할 수 있는 기능을 제공하므로, [SOLID](https://en.wikipedia.org/wiki/SOLID) 원칙을 따르는 것을 강력히 권장합니다.

SOLID 원칙은 다음과 같습니다.

1. **단일 책임 원칙 (Single-responsibility principle):** '한 클래스가 변경되어야 하는 이유는 오직 하나뿐이어야 합니다.' 다시 말해, 모든 클래스는 하나의 책임만 가져야 합니다.
2. **개방/폐쇄 원칙 (Open–closed principle):** '소프트웨어 엔터티는 확장에는 열려있어야 하지만 수정에는 닫혀 있어야 합니다.' 즉, 기존의 코드를 수정하지 않고도 새로운 기능을 추가할 수 있어야 합니다.
3. **리스코프 치환 원칙 (Liskov substitution principle):** '기본 클래스의 포인터나 참조를 사용하는 함수는 그것들을 모르고도 파생 클래스의 객체를 사용할 수 있어야 합니다.' 계약에 따른 설계도 참고하세요.
4. **인터페이스 분리 원칙 (Interface segregation principle):** '클라이언트는 사용하지 않는 인터페이스에 의존하지 않아야 합니다.'
5. **의존성 역전 원칙 (Dependency inversion principle):** '추상화에 의존하되 구체화에 의존하지 마십시오.'

​			

### Services[#](https://docs.nestjs.com/providers#services)

간단한 `CatsService`를 만들어 보겠습니다. 이 서비스는 데이터의 저장 및 검색을 담당하며, `CatsController`에서 사용될 것으로 예상되므로 프로바이더로 정의하기에 좋은 후보입니다.

```typescript
// cats.service.ts

import { Injectable } from '@nestjs/common';
import { Cat } from './interfaces/cat.interface';

@Injectable()
export class CatsService {
  private readonly cats: Cat[] = [];

  create(cat: Cat) {
    this.cats.push(cat);
  }

  findAll(): Cat[] {
    return this.cats;
  }
}
```

> **HINT**
> To create a service using the CLI, simply execute the `$ nest g service cats` command.

우리의 `CatsService`는 하나의 속성과 두 개의 메서드를 가진 기본적인 클래스입니다. 유일하게 새로운 기능은 `@Injectable()` 데코레이터를 사용한다는 것입니다. `@Injectable()` 데코레이터는 `CatsService`가 Nest <u>*IoC</u> 컨테이너에서 관리될 수 있는 클래스임을 선언하는 메타데이터를 첨부합니다. 이 예제에서는 아마도 다음과 같이 보이는 `Cat` 인터페이스도 사용하고 있습니다.

​			

*IoC는 "Inversion of Control"의 약어로, 한국어로는 "제어의 역전"이라고 번역됩니다. IoC는 소프트웨어 디자인에서 중요한 개념 중 하나로, 일반적으로 의존성 주입(Dependency Injection)과 관련이 있습니다.

기존에는 개발자가 코드의 제어 흐름을 직접 제어하고 구성했습니다. 그러나 IoC에서는 제어의 주도권이 프레임워크나 컨테이너로 넘어가게 됩니다. 이는 개발자가 코드를 작성할 때 일부 제어를 외부에서 받아들이는 개념입니다. 

일반적으로 IoC는 다음과 같은 특징을 포함합니다:

1. **의존성 주입(Dependency Injection):** 객체가 필요로 하는 의존성을 직접 생성하는 대신, 외부에서 의존성을 주입받는 방식입니다. 이를 통해 코드의 결합도를 낮추고 유연성을 높일 수 있습니다.
2. **제어의 역전:** 개발자가 코드의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 제어 흐름이 주어지는 것을 의미합니다. 일반적으로는 컨테이너나 프레임워크가 코드의 실행을 주도하는 구조를 말합니다.

```typescript
// interfaces/cat.interface.ts

export interface Cat {
  name: string;
  age: number;
  breed: string;
}
```

이제 고양이를 검색하는 서비스 클래스가 있으니, 이를 `CatsController`내에서 사용해보겠습니다:

```typescript
// cats.controller.ts

import { Controller, Get, Post, Body } from '@nestjs/common';
import { CreateCatDto } from './dto/create-cat.dto';
import { CatsService } from './cats.service';
import { Cat } from './interfaces/cat.interface';

@Controller('cats')
export class CatsController {
  constructor(private catsService: CatsService) {}

  @Post()
  async create(@Body() createCatDto: CreateCatDto) {
    this.catsService.create(createCatDto);
  }

  @Get()
  async findAll(): Promise<Cat[]> {
    return this.catsService.findAll();
  }
}
```

`CatsService`는 클래스 생성자를 통해 주입됩니다. `private` 구문을 사용하는 것에 주목하세요. 이 간편한 표기법을 사용하면 `catsService` 멤버를 동시에 선언하고 초기화할 수 있습니다.

​	

### Dependency injection[#](https://docs.nestjs.com/providers#dependency-injection)

Nest는 보편적으로 알려진 의존성 주입이라는 강력한 디자인 패턴을 기반으로 구축되었습니다. 이 개념에 대한 훌륭한 기사를 읽는 것을 권장합니다. official [Angular](https://angular.io/guide/dependency-injection) documentation

Nest에서는 TypeScript의 능력 덕분에 의존성을 관리하는 것이 매우 쉽습니다. 왜냐하면 TypeScript는 단순히 타입에 의해 의존성이 해결되기 때문입니다. 아래의 예제에서 Nest는 `catsService`를 해결하고 `CatsService`의 인스턴스를 생성하여 반환합니다. (또는 싱글톤의 일반적인 경우에는 이미 다른 곳에서 요청되었다면 기존 인스턴스를 반환합니다) 이 의존성은 해결되고 해당 의존성은 컨트롤러의 생성자로 전달됩니다(또는 지정된 속성에 할당됩니다):

```typescript
constructor(private catsService: CatsService) {}
```

​	

### Scopes[#](https://docs.nestjs.com/providers#scopes)

프로바이더는 일반적으로 애플리케이션 라이프사이클과 동기화된 수명(혹은 '스코프')을 갖습니다. 애플리케이션이 부트스트랩될 때 모든 의존성이 해결되어야 하며, 따라서 모든 프로바이더는 인스턴스화되어야 합니다. 마찬가지로 애플리케이션이 종료될 때마다 각 프로바이더가 소멸됩니다. 그러나 프로바이더 수명을 요청 스코프(request-scoped)로 만드는 방법도 있습니다. 이에 대한 자세한 내용은 여기([here](https://docs.nestjs.com/fundamentals/injection-scopes))에서 확인할 수 있습니다.

​	

### Custom providers[#](https://docs.nestjs.com/providers#custom-providers)

Nest는 제공자 간의 관계를 해결하는 내장형 제어의 역전('IoC') 컨테이너를 갖고 있습니다. 이 기능은 위에서 설명한 의존성 주입 기능의 기반이지만, 실제로는 여태까지 설명한 것보다 훨씬 더 강력합니다. 프로바이더를 정의하는 여러 가지 방법이 있습니다: 평범한 값, 클래스, 비동기 또는 동기적인 팩토리를 사용할 수 있습니다. 더 많은 예제는 여기([here](https://docs.nestjs.com/fundamentals/dependency-injection))에서 제공됩니다.

​	

### Optinal providers[#](https://docs.nestjs.com/providers#optional-providers)

가끔은 해결되지 않아도 괜찮은 의존성이 있을 수 있습니다. 예를 들어, 클래스가 구성 객체에 의존할 수 있지만 전달되지 않으면 기본값을 사용해야 할 수 있습니다. 이 경우 구성 프로바이더가 없어도 오류가 발생하지 않기 때문에 의존성은 선택적이 됩니다.

프로바이더가 선택적임을 나타내려면 생성자의 시그니처에서 `@Optional()`데코레이터를 사용하십시오."

```typescript
import { Injectable, Optional, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  constructor(@Optional() @Inject('HTTP_OPTIONS') private httpClient: T) {}
}
```

위의 예제에서는 `HTTP_OPTIONS` 사용자 지정 토큰을 포함하는 사용자 지정 프로바이더를 사용하고 있습니다. 이는 이전 예제에서 생성자 기반 주입을 보여주면서 생성자 내에서 클래스를 통해 의존성을 나타냈던 이유입니다. 사용자 지정 프로바이더 및 관련 토큰에 대한 자세한 내용은 여기([here](https://docs.nestjs.com/fundamentals/custom-providers))에서 확인할 수 있습니다.

​	

### Property-based injection[#](https://docs.nestjs.com/providers#property-based-injection)

지금까지 사용한 기술은 생성자 기반 주입(Constructor-based Injection)이라고 불립니다. 여기서 프로바이더는 생성자 메서드를 통해 주입됩니다. 매우 특수한 경우에는 속성 기반 주입(Property-based Injection)이 유용할 수 있습니다. 예를 들어, 최상위 클래스가 하나 이상의 프로바이더에 의존하는 경우, 서브 클래스의 생성자에서 `super()`를 호출하여 모든 프로바이더를 계속 전달하는 것은 매우 번거로울 수 있습니다. 이를 피하기 위해 속성 레벨에서 `@Inject()` 데코레이터를 사용할 수 있습니다.

```typescript
import { Injectable, Inject } from '@nestjs/common';

@Injectable()
export class HttpService<T> {
  @Inject('HTTP_OPTIONS')
  private readonly httpClient: T;
}
```

> **WARNING**
> If your class doesn't extend another class, you should always prefer using **constructor-based** injection.

​	

### Provider registration[#](https://docs.nestjs.com/providers#provider-registration)

이제 우리는 프로바이더(`CatsService`)를 정의했고, 해당 서비스를 사용하는 소비자(`CatsController`)가 있습니다. 이 서비스를 Nest에 등록하여 주입을 수행할 수 있도록 해야 합니다. 이를 위해 모듈 파일인 (`app.module.ts`)을 편집하고, 서비스를 `@Module()` 데코레이터의 `providers` 배열에 추가합니다.

```typescript
import { Module } from '@nestjs/common';
import { CatsController } from './cats/cats.controller';
import { CatsService } from './cats/cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class AppModule {}
```

Nest는 이제 CatsController 클래스의 의존성을 해결할 수 있게 될 것입니다.

​	

이제 디렉터리 구조는 다음과 같아야 합니다:

> src
>
> └ cats
>
> &nbsp;&nbsp;&nbsp;&nbsp;└ dto
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└ create-cat.dto.ts
>
> &nbsp;&nbsp;&nbsp;&nbsp;└ interfaces
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;└ cat.interface.ts
>
> &nbsp;&nbsp;&nbsp;&nbsp;└ cats.controller.ts
>
> &nbsp;&nbsp;&nbsp;&nbsp;└ cats.service.ts
>
> app.module.ts
>
> main.ts

​		

### Manual instantiation(인스턴스(객체)화)[#](https://docs.nestjs.com/providers#manual-instantiation)

지금까지 우리는 Nest가 대부분의 의존성 해결 세부 사항을 자동으로 처리하는 방법에 대해 논의했습니다. 특정 상황에서는 내장된 의존성 주입 시스템 밖으로 나가서 프로바이더를 수동으로 검색하거나 인스턴스화해야 할 수 있습니다. 아래에서 두 가지 관련 주제에 대해 간략하게 논의하겠습니다.

기존 인스턴스를 가져오거나 프로바이더를 동적으로 인스턴스화하려면 [Module reference](https://docs.nestjs.com/fundamentals/module-ref)를 사용할 수 있습니다.

`bootstrap()` 함수 내에서 프로바이더를 얻으려면(예: 컨트롤러 없이 독립적인 애플리케이션 또는 부트스트래핑 중에 구성 서비스를 활용하기 위한 경우) [Standalone applications](https://docs.nestjs.com/standalone-applications)를 참조하세요.

​	

​	

​	

​	


