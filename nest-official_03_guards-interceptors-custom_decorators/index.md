# Nest Official_03_(Guards, Interceptors, Custom_decorators)


​	

## Guards

> 가드(Guard)는 라우트 핸들러 메서드의 실행 여부를 결정하는 역할을 합니다. 특히, 가드는 특정 조건이 충족되지 않으면 요청을 처리하기 전에 실행을 막습니다. 이는 권한 검사, 인증, 혹은 다양한 비즈니스 규칙을 적용할 때 유용합니다.
>
> 간단한 예로, 사용자의 권한을 확인하는 가드를 상상해볼 수 있습니다. 특정 엔드포인트에 접근하려는 사용자의 역할이나 권한이 충족되지 않으면 가드가 요청을 차단할 수 있습니다. 이를 통해 요청이 핸들러 메서드에 도달하기 전에 필요한 조건을 검사하고 처리할 수 있습니다.

가드는 `@Injectable()` 데코레이터로 주석이 달린 클래스로, `CanActivate` 인터페이스를 구현합니다.

가드(Guards)는 **단일 책임**을 가지고 있습니다. <u>특정 조건(권한, 역할, ACL(access control list; 접근 제어 목록) 등)이 실행 시점에 존재하는지 여부에 따라 주어진 요청이 라우트 핸들러에 의해 처리될지 여부를 결정합니다.</u> 이는 일반적으로 <b>인가(authorization)</b>로 언급되며 종종 <b>인증(authentication)</b>과 함께 작동합니다. 전통적인 Express 애플리케이션에서는 [middleware](https://docs.nestjs.com/middleware)를 통해 일반적으로 이러한 작업이 처리되었습니다. 미들웨어는 토큰 유효성 검사 및 `request` 객체에 속성을 추가하는 것과 같은 작업이 특정 라우트 컨텍스트(및 해당 메타데이터)와 강하게 연결되어 있지 않기 때문에 인증에 대한 좋은 선택입니다.

그러나 미들웨어는 본질적으로 무지합니다. `next()` 함수를 호출한 후에 어떤 핸들러가 실행될지를 알지 못합니다. 반면에 **가드**는 `ExecutionContext` 인스턴스에 액세스할 수 있으며 따라서 정확히 다음에 실행될 것을 알고 있습니다. 이들은 예외 필터, 파이프, 인터셉터와 마찬가지로 요청/응답 주기에서 처리 로직을 정확한 지점에 삽입하고 선언적으로 수행할 수 있도록 설계되었습니다. 이는 코드를 DRY(Don't Repeat Yourself)하고 선언적으로 유지하는 데 도움이 됩니다.

> **HINT**
>
> Guards are executed **after** all middleware, but **before** any interceptor or pipe.

​	

### Authorization guard[#](https://docs.nestjs.com/guards#authorization-guard)

언급한 대로 **권한** 부여는 가드의 좋은 사용 사례입니다. 특정 라우트는 호출자(일반적으로 특정 인증된 사용자)가 충분한 권한을 가질 때만 사용 가능해야 합니다. 이제 구축할 `AuthGuard`는 인증된 사용자를 가정하고 (따라서 토큰이 요청 헤더에 첨부되어 있다고 가정합니다) 토큰을 추출하고 유효성을 검사한 다음 추출된 정보를 사용하여 요청이 계속 진행할 수 있는지 여부를 결정합니다.

```typescript
//auth.guard.ts

import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class AuthGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    const request = context.switchToHttp().getRequest();
    return validateRequest(request);
  }
}
```

> **HINT**
>
> 만약 애플리케이션에 인증 메커니즘을 구현하는 실제 예제를 찾고 있다면, [이 장](https://docs.nestjs.com/security/authentication)을 참조하세요. 마찬가지로 더 정교한(sophisticated ) 권한 부여 예제를 원한다면 [이 페이지](https://docs.nestjs.com/security/authorization)를 확인하세요.

`validateRequest()` 함수 내부의 로직은 필요에 따라 간단하거나 복잡할 수 있습니다. 이 예제의 주요 포인트는 가드가 요청/응답 주기에 어떻게 맞아 떨어지는지를 보여주는 것입니다.

모든 가드는 `canActivate()` 함수를 구현해야 합니다. 이 함수는 현재 요청이 허용되는지 여부를 나타내는 boolean 값을 반환해야 합니다. 이 함수는 동기적으로 또는 비동기적으로 (Promise나 Observable을 통해) 응답을 반환할 수 있습니다. Nest는 반환된 값을 사용하여 다음 동작을 제어합니다:

- true를 반환하면 요청이 처리됩니다.
- false를 반환하면 Nest는 요청을 거부합니다.

​	

### Execution context[#](https://docs.nestjs.com/guards#execution-context)

`canActivate()` 함수는 단일 인수인 `ExecutionContext` 인스턴스를 받습니다. `ExecutionContext`는 `ArgumentsHost`에서 상속됩니다. 이전에 예외 필터 챕터에서 `ArgumentsHost`를 보았습니다. 위의 예제에서는 이전에 사용한 것과 동일한 `ArgumentsHost`에서 정의된 도우미 메서드를 사용하여 `Request` 객체에 대한 참조를 가져 오고 있습니다. 이 주제에 대한 자세한 내용은 [exception filters](https://docs.nestjs.com/exception-filters#arguments-host) 챕터의 **Arguments host** 섹션을 참조할 수 있습니다.

`ExecutionContext`는 `ArgumentsHost`를 확장함으로써 현재 실행 프로세스에 대한 추가적인 세부 정보를 제공하는 여러 개의 새로운 도우미 메서드를 추가합니다. 이러한 세부 정보는 더 일반적인 가드를 작성할 때 도움이 될 수 있으며 이 가드는 다양한 컨트롤러, 메서드 및 실행 컨텍스트 집합에서 작동할 수 있습니다. `ExecutionContext`에 대한 자세한 내용은 [여기](https://docs.nestjs.com/fundamentals/execution-context)에서 확인할 수 있습니다.

​	

### Role-based authentication[#](https://docs.nestjs.com/guards#role-based-authentication)

특정 역할을 가진 사용자만 액세스를 허용하는 더 기능적인 가드를 만들어보겠습니다. 기본 가드 템플릿에서 시작하고 이후 섹션에서 이를 확장해 나갈 것입니다. 현재는 모든 요청을 허용하는 기본적인 구조입니다:

```typescript
//roles.guard.ts

import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Observable } from 'rxjs';

@Injectable()
export class RolesGuard implements CanActivate {
  canActivate(
    context: ExecutionContext,
  ): boolean | Promise<boolean> | Observable<boolean> {
    return true;
  }
}
```

​	

### Binding guards[#](https://docs.nestjs.com/guards#binding-guards)

파이프와 예외 필터와 마찬가지로 가드는 **컨트롤러 범위(controller-scoped)**, 메서드 범위, 또는 전역 범위로 설정할 수 있습니다. 아래에서는 `@UseGuards()` 데코레이터를 사용하여 컨트롤러 범위의 가드를 설정합니다. 이 데코레이터는 단일 인수나 쉼표로 구분된 인수 목록을 받을 수 있습니다. 이를 통해 하나의 선언으로 적절한 가드 집합을 쉽게 적용할 수 있습니다.

```typescript
@Controller('cats')
@UseGuards(RolesGuard)
export class CatsController {}
```

> **HINT**
>
> The `@UseGuards()` decorator is imported from the `@nestjs/common` package.

위에서는 인스턴스 대신 `RolesGuard` 클래스를 전달하여 인스턴스화 책임을 프레임워크에게 맡기고 의존성 주입을 활성화했습니다. 파이프와 예외 필터와 마찬가지로 인스턴스를 직접 전달할 수도 있습니다:

```typescript
@Controller('cats')
@UseGuards(new RolesGuard())
export class CatsController {}
```

위의 구성은 이 컨트롤러에서 선언된 모든 핸들러에 가드를 연결합니다. 가드를 하나의 메서드에만 적용하려면 `@UseGuards()` 데코레이터를 **메서드 레벨**에서 적용합니다.

전역 가드를 설정하려면 Nest 애플리케이션 인스턴스의 `useGlobalGuards()` 메서드를 사용하세요:

```typescript
//main.ts

const app = await NestFactory.create(AppModule);
app.useGlobalGuards(new RolesGuard());
```

> **NOTICE**
>
> 하이브리드 앱의 경우 `useGlobalGuards()` 메서드는 기본적으로 게이트웨이 및 마이크로 서비스에 가드를 설정하지 않습니다. (이 동작을 변경하는 방법에 대한 정보는 [Hybrid application](https://docs.nestjs.com/faq/hybrid-application)을 참조하세요.) "표준" (비-하이브리드) 마이크로서비스 앱의 경우 `useGlobalGuards()`는 가드를 전역으로 마운트합니다.

전역 가드는 전체 애플리케이션, 모든 컨트롤러 및 모든 라우트 핸들러에 걸쳐 사용됩니다. 의존성 주입 측면에서 모듈 외부에서 등록된 전역 가드 (`useGlobalGuards()`를 사용한 예제와 같이)는 어떤 모듈의 컨텍스트 외부에서 수행되므로 종속성을 주입할 수 없습니다. 이 문제를 해결하려면 다음 구성을 사용하여 모듈에서 직접 가드를 설정할 수 있습니다:

```typescript
//app.module.ts

import { Module } from '@nestjs/common';
import { APP_GUARD } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_GUARD,
      useClass: RolesGuard,
    },
  ],
})
export class AppModule {}
```

> **HINT**
>
> 이 방법을 사용하여 가드에 대한 종속성 주입을 수행할 때 어떤 모듈에서 이 구성을 사용하든지 간에 이 가드는 사실상 전역입니다. 어디에서 이 작업을 수행해야 할까요? 가드 (`RolesGuard`는 위의 예제에서와 같이)가 정의된 모듈을 선택하세요. 또한, `useClass`는 사용자 정의 공급자 등록을 다루는 유일한 방법이 아닙니다. [여기](https://docs.nestjs.com/fundamentals/custom-providers)에서 자세히 알아보세요.

​	

### Setting roles per handler[#](https://docs.nestjs.com/guards#setting-roles-per-handler)

`RolesGuard`가 작동하지만 아직 충분히 스마트하지 않습니다. 가장 중요한 가드 기능 중 하나인 [실행 컨텍스트](https://docs.nestjs.com/fundamentals/execution-context)를 아직 활용하고 있지 않습니다. 아직 역할이나 각 핸들러에 대한 허용된 역할에 대한 정보를 알지 못합니다. 예를 들어 `CatsController`는 다른 라우트에 대해 다른 권한 체계를 가질 수 있습니다. 어떤 라우트는 관리자 사용자만 사용할 수 있고 다른 라우트는 모두에게 열려 있을 수 있습니다. 어떻게 역할을 유연하고 재사용 가능한 방식으로 라우트에 매핑할 수 있을까요?

여기서 **사용자 정의 메타데이터**가 등장합니다 ([여기](https://docs.nestjs.com/fundamentals/execution-context#reflection-and-metadata)에서 더 알아보세요). Nest는 `Reflector#createDecorator` 정적 메서드를 통해 생성된 데코레이터 또는 내장된 `@SetMetadata()` 데코레이터를 통해 라우트 핸들러에 사용자 정의 **메타데이터**를 첨부할 수 있는 기능을 제공합니다.

예를 들어 `Reflector#createDecorator` 메서드를 사용하여 핸들러에 메타데이터를 첨부할 `@Roles()` 데코레이터를 만들어봅시다. `Reflector`는 프레임워크에서 기본적으로 제공되며 `@nestjs/core` 패키지에서 노출됩니다.

```typescript
//roles.decorator.ts

import { Reflector } from '@nestjs/core';

export const Roles = Reflector.createDecorator<string[]>();
```

The `Roles` decorator here is a function that takes a single argument of type `string[]`.

Now, to use this decorator, we simply annotate the handler with it:

```typescript
//cats.controller.ts

@Post()
@Roles(['admin'])
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

여기서는 `create()` 메서드에 `Roles` 데코레이터 메타데이터를 첨부했습니다. 이는 해당 라우트에는 `admin` 역할을 가진 사용자만 액세스할 수 있어야 함을 나타냅니다.

대신에 `Reflector#createDecorator` 메서드 대신 내장된 `@SetMetadata()` 데코레이터를 사용할 수도 있습니다. [여기](https://docs.nestjs.com/fundamentals/execution-context#low-level-approach)에서 더 자세히 알아보세요.

​	

### Putting it all together[#](https://docs.nestjs.com/guards#putting-it-all-together)

이제 `RolesGuard`와 함께 이를 연결해 보겠습니다. 현재 `RolesGuard`는 모든 경우에 `true`를 반환하여 모든 요청을 진행시킵니다. 우리는 현재 사용자에게 할당된 **역할을 현재 처리 중인 라우트에서 필요로 하는 실제 역할과 비교하여** 반환 값을 조건부로 만들고 싶습니다. 라우트의 역할(사용자 정의 메타데이터)에 액세스하려면 다시 `Reflector` 도우미 클래스를 사용하겠습니다. 아래와 같이:

```typescript
//roles.guard.ts

import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { Reflector } from '@nestjs/core';
import { Roles } from './roles.decorator';

@Injectable()
export class RolesGuard implements CanActivate {
  constructor(private reflector: Reflector) {}

  canActivate(context: ExecutionContext): boolean {
    const roles = this.reflector.get(Roles, context.getHandler());
    if (!roles) {
      return true;
    }
    const request = context.switchToHttp().getRequest();
    const user = request.user;
    return matchRoles(roles, user.roles);
  }
}
```

> **HINT**
>
> Node.js 세계에서는 흔히 권한이 부여된 사용자를 `request` 객체에 첨부하는 것이 일반적입니다. 따라서 위의 샘플 코드에서는 `request.user`가 사용자 인스턴스와 허용된 역할을 포함하고 있다고 가정하고 있습니다. 여러분의 앱에서는 이러한 연관성을 사용자 정의 **인증 가드**나 미들웨어에서 설정할 것으로 예상됩니다. 이 주제에 대한 자세한 정보는 [이 장](https://docs.nestjs.com/security/authentication)을 참조하세요.

{{< admonition warning "WARNING" true >}}
matchRoles() 함수 내의 로직은 필요에 따라 간단하거나 복잡하게 구성할 수 있습니다. 이 예제의 주요 목적은 가드가 요청/응답 주기에 어떻게 적합한지를 보여주는 것입니다.
{{< /admonition >}}

`Reflector`를 컨텍스트에 맞게 활용하는 방법에 대한 자세한 내용은 **Execution context** 장의 [Reflection and metadata](https://docs.nestjs.com/fundamentals/execution-context#reflection-and-metadata) 섹션을 참조하십시오.

권한이 부족한 사용자가 엔드포인트를 요청하면 Nest는 자동으로 다음과 같은 응답을 반환합니다:

```json
{
  "statusCode": 403,
  "message": "Forbidden resource",
  "error": "Forbidden"
}
```

Note that behind the scenes, 가드가 `false`를 반환하면 프레임워크가 `ForbiddenException`을 throw합니다. 다른 오류 응답을 반환하려면 자신만의 특정 예외를 던져야 합니다. 예를 들면:

```typescript
throw new UnauthorizedException();
```

가드에서 throw된 예외는 [예외 레이어](https://docs.nestjs.com/exception-filters) 에서 처리됩니다.(전역 예외 필터 및 현재 컨텍스트에 적용된 모든 예외 필터)

> **HINT**
> If you are looking for a real-world example on how to implement authorization, check [this chapter](https://docs.nestjs.com/security/authorization).

​	

​	

## Interceptors

> 인터셉터는 요청과 응답 처리의 중간에 위치하여, 해당 요청 및 응답을 수정하거나 변형할 수 있는 클래스입니다. 인터셉터는 특정 기능을 추상화하고 재사용 가능한 비즈니스 로직을 캡슐화하는 데 사용됩니다.
>
> 인터셉터는 주로 다음과 같은 작업을 수행할 수 있습니다:
>
> 1. **전처리 및 후처리 작업**: 요청이나 응답을 처리하기 전이나 후에 특정 작업을 수행할 수 있습니다. 예를 들어, 데이터 로깅, 트래킹, 또는 요청/응답 수정 등이 가능합니다.
> 2. **예외 처리**: 요청 또는 응답 중에 예외가 발생하면 인터셉터에서 해당 예외를 처리하고 특정한 형태로 응답을 조작할 수 있습니다.
> 3. **캐싱**: 일부 요청의 결과를 캐싱하여 성능을 향상시킬 수 있습니다.
> 4. **트랜스포메이션**: 요청이나 응답을 필요에 따라 변환하거나 가공할 수 있습니다.
> 5. **권한 검사**: 특정한 권한을 가진 사용자만이 특정한 요청에 접근할 수 있도록 제어할 수 있습니다.
>
> 인터셉터는 모듈, 컨트롤러, 메서드 수준에서 적용될 수 있으며, 각각의 인터셉터는 특정한 용도에 맞게 설계될 수 있습니다. 

인터셉터는 `@Injectable()` 데코레이터로 주석이 달린 클래스이며 `NestInterceptor` 인터페이스를 구현하는 클래스입니다.


인터셉터는 [Aspect Oriented Programming](https://en.wikipedia.org/wiki/Aspect-oriented_programming) (AOP) 기법에서 영감을 받은 유용한 기능을 제공합니다. 이를 통해 다음과 같은 작업이 가능해집니다:

- 메서드 실행 전/후에 추가 로직 바인딩
- 함수에서 반환된 결과를 변환
- 함수에서 발생한 예외를 변환
- 기본 함수 동작 확장
- 특정 조건에 따라 함수 완전히 재정의 (예: 캐싱 목적)

​	

### Basics[#](https://docs.nestjs.com/interceptors#basics)

각 인터셉터는 두 개의 인자를 받는 `intercept()` 메소드를 구현합니다. 첫 번째 인자는 `ExecutionContext` 인스턴스로 [가드](https://docs.nestjs.com/guards)에서 사용되는 것과 동일한 객체입니다. `ExecutionContext`는 `ArgumentsHost`에서 상속됩니다. 우리는 이전에 예외 필터 챕터에서 `ArgumentsHost`를 보았습니다. 거기에서 원래 핸들러에 전달된 인자를 래핑하고 응용 프로그램의 유형에 따라 다른 인자 배열을 포함한다는 것을 알았습니다. 이 주제에 대한 자세한 내용은 [예외 필터](https://docs.nestjs.com/exception-filters#arguments-host)를 참조할 수 있습니다.

​	

### Execution context[#](https://docs.nestjs.com/interceptors#execution-context)

`ExecutionContext`는 `ArgumentsHost`를 확장함으로써 현재 실행 프로세스에 대한 추가적인 세부 정보를 제공하는 여러 개의 헬퍼 메서드를 추가합니다. 이러한 세부 정보는 더 일반적인 인터셉터를 작성할 때 도움이 될 수 있으며, 이 인터셉터는 다양한 컨트롤러, 메서드 및 실행 컨텍스트에서 작동할 수 있습니다. `ExecutionContext`에 대한 더 자세한 내용은 [여기](https://docs.nestjs.com/fundamentals/execution-context)에서 확인할 수 있습니다.

​	

### Call handler[#](https://docs.nestjs.com/interceptors#call-handler)

두 번째 인수는 `CallHandler`입니다. `CallHandler` 인터페이스는 `interceptor`에서 라우트 핸들러 메서드를 어느 시점에서든 호출할 수 있도록 `handle()` 메서드를 구현합니다. `intercept()` 메서드에서 `handle()` 메서드를 호출하지 않으면 라우트 핸들러 메서드가 전혀 실행되지 않습니다.

이 접근 방식은 `intercept()` 메서드가 요청/응답 스트림을 효과적으로 **랩**한다는 것을 의미합니다. 그리고 최종 라우트 핸들러 실행 **전후**에 사용자 정의 로직을 사용하고 싶을 수도 있습니다. `intercept()` 메서드에서 `handle()`을 호출하기 **전에** 코드를 작성하는 것은 명확하지만, 이후에 어떻게 영향을 미칠 수 있을까요? `handle()` 메서드는 `Observable`을 반환하므로 [RxJS](https://github.com/ReactiveX/rxjs) 연산자를 사용하여 응답을 추가로 조작할 수 있습니다. **Aspect Oriented Programming** 용어를 사용하면 라우트 핸들러를 호출하는 것(즉, `handle()`을 호출하는 것)은 [Pointcut](https://en.wikipedia.org/wiki/Pointcut)이라고 하며, 여기에 추가로직이 삽입되는 지점을 나타냅니다.

예를 들어, 들어오는 `POST /cats` 요청이 있습니다. 이 요청은 `CatsController` 내에 정의된 `create()` 핸들러를 대상으로 합니다. `handle()` 메서드를 호출하지 않는 인터셉터가 코드 중간 어디에서든(along the way) 사용되면 `create()` 메서드가 실행되지 않습니다. `handle()`이 호출되면 (그리고 그 `Observable`이 반환된 후에), `create()` 핸들러가 트리거됩니다. 그리고 `Observable`을 통해 수신한 응답 스트림을 사용하여 추가 작업을 수행하고 최종 결과를 호출자에게 반환할 수 있습니다.

​	

### Aspect interception[#](https://docs.nestjs.com/interceptors#aspect-interception)

첫 번째 사용 사례는 인터셉터를 사용하여 사용자 상호 작용을 기록하는 것입니다. 예를 들어 사용자 호출을 저장하거나 이벤트를 비동기적으로 디스패치하거나 타임스탬프를 계산하는 등의 상호 작용을 기록하는 간단한 `LoggingInterceptor`를 아래에 표시합니다:

```typescript
//logging.interceptor.ts

import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { tap } from 'rxjs/operators';

@Injectable()
export class LoggingInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    console.log('Before...');

    const now = Date.now();
    return next
      .handle()
      .pipe(
        tap(() => console.log(`After... ${Date.now() - now}ms`)),
      );
  }
}
```

> **HINT**
>
> The `NestInterceptor<T, R>` is a generic interface in which `T` indicates the type of an `Observable<T>` (supporting the response stream), and `R` is the type of the value wrapped by `Observable<R>`.

> **NOTICE**
>
> 인터셉터는 컨트롤러, 프로바이더, 가드 등과 마찬가지로 `constructor`를 통해 **의존성을 주입**할 수 있습니다.

`handle()`가 RxJS `Observable`을 반환하기 때문에 우리는 스트림을 조작하기 위해 다양한 오퍼레이터를 선택할 수 있습니다. 위의 예제에서는 `tap()` 오퍼레이터를 사용했는데, 이는 우리의 익명 로깅 함수를 옵저버블 스트림이 정상적이거나 예외적으로 종료될 때 호출하지만 그 외에는 응답 주기에 간섭하지 않습니다.

​	

### Binding interceptors[#](https://docs.nestjs.com/interceptors#binding-interceptors)

인터셉터를 설정하기 위해서는 `@nestjs/common` 패키지에서 가져온 `@UseInterceptors()` 데코레이터를 사용합니다. [파이프](https://docs.nestjs.com/pipes)와 [가드](https://docs.nestjs.com/guards)와 마찬가지로 인터셉터는 컨트롤러 범위, 메서드 범위 또는 글로벌 범위로 설정할 수 있습니다.

```typescript
//cats.controller.ts

@UseInterceptors(LoggingInterceptor)
export class CatsController {}
```

> **HINT**
>
> The `@UseInterceptors()` decorator is imported from the `@nestjs/common` package.

위의 구조를 사용하면 `CatsController`에 정의된 각 라우트 핸들러는 `LoggingInterceptor`를 사용합니다. 누군가 `GET /cats` 엔드포인트를 호출하면 표준 출력에서 다음과 같은 출력을 볼 수 있습니다:

```cmd
Before...
After... 1ms
```

앞서 언급한 대로 `LoggingInterceptor` 타입을 전달하여 (인스턴스 대신에) 프레임워크에 인스턴스화 책임을 맡기고 의존성 주입을 활성화했습니다. 파이프, 가드, 예외 필터와 마찬가지로 현장에서 직접 인스턴스를 전달할 수도 있습니다:

```typescript
//cats.controller.ts

@UseInterceptors(new LoggingInterceptor())
export class CatsController {}
```

앞서 언급했듯이 위의 구성은 이 인터셉터를 이 컨트롤러에서 선언된 모든 핸들러에 첨부합니다. 인터셉터의 범위를 단일 메서드로 제한하려면 데코레이터를 **메서드 레벨**에서 적용하면 됩니다.

전역 인터셉터를 설정하려면 Nest 애플리케이션 인스턴스의 `useGlobalInterceptors()` 메서드를 사용합니다:

```typescript
//main.ts

const app = await NestFactory.create(AppModule);
app.useGlobalInterceptors(new LoggingInterceptor());
```

전역 인터셉터는 전체 애플리케이션에서 모든 컨트롤러와 라우트 핸들러에 사용됩니다. 의존성 주입 측면에서 위 예제와 같이 외부에서 `useGlobalInterceptors()`를 사용하여 등록한 전역 인터셉터는 어떤 모듈의 컨텍스트에서도 의존성을 주입할 수 없습니다. 이 문제를 해결하기 위해 다음 구성을 사용하여 **모듈에서 직접** 인터셉터를 설정할 수 있습니다:

```typescript
//app.module.ts

import { Module } from '@nestjs/common';
import { APP_INTERCEPTOR } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_INTERCEPTOR,
      useClass: LoggingInterceptor,
    },
  ],
})
export class AppModule {}
```

> **HINT**
>
> 이 접근 방식을 사용하여 인터셉터에 대한 의존성 주입을 수행할 때, 이 구성이 적용된 모듈과 관계없이 해당 인터셉터는 사실상 전역적입니다. 어디에서 이 작업을 수행해야 할까요? 위의 예제에서 나온대로 인터셉터가 정의된 모듈을 선택하세요. 또한, `useClass`는 사용자 정의 제공자 등록을 다루는 유일한 방법이 아닙니다. 더 자세한 내용은 [여기](https://docs.nestjs.com/fundamentals/custom-providers)에서 알아보세요.

​	

### Response mapping[#](https://docs.nestjs.com/interceptors#response-mapping)

우리는 이미 `handle()`이 Observable을 반환한다는 것을 알고 있습니다. 이 스트림에는 route handler에서 반환된 값이 포함되어 있으므로 RxJS의 `map()` 연산자를 사용하여 쉽게 변형할 수 있습니다.

{{< admonition warning "WARNING" true >}}
매핑 기능은 라이브러리별 응답 전략과 함께 작동하지 않습니다 (`@Res()` 객체를 직접 사용하는 것은 금지).
{{< /admonition >}}

다음은 `TransformInterceptor`를 만들어보겠습니다. 이 인터셉터는 간단한 방식으로 각 응답을 수정하여 프로세스를 보여줍니다. RxJS의 `map()` 연산자를 사용하여 응답 객체를 새로 생성된 객체의 `data` 속성에 할당하고, 이 새로운 객체를 클라이언트에 반환합니다.

```typescript
//transform.interceptor.ts

import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

export interface Response<T> {
  data: T;
}

@Injectable()
export class TransformInterceptor<T> implements NestInterceptor<T, Response<T>> {
  intercept(context: ExecutionContext, next: CallHandler): Observable<Response<T>> {
    return next.handle().pipe(map(data => ({ data })));
  }
}
```

> **HINT**
>
> Nest 인터셉터는 동기 및 비동기 `intercept()` 메서드와 함께 작동합니다. 필요한 경우 메서드를 `async`로 전환할 수 있습니다.

위의 구성으로 누군가 `GET /cats` 엔드포인트를 호출하면, 응답은 다음과 같을 것입니다 (루트 핸들러가 빈 배열 `[]`을 반환하는 경우를 가정합니다):

```json
{
  "data": []
}
```

인터셉터는 전체 애플리케이션에 걸쳐 발생하는 요구 사항에 대한 재사용 가능한 솔루션을 만드는 데 큰 가치가 있습니다. 예를 들어 `null` 값을 빈 문자열 `''`로 변환해야 하는 경우를 상상해보세요. 한 줄의 코드로 이를 수행하고 인터셉터를 전역으로 바인딩하여 각 등록된 핸들러에서 자동으로 사용하도록 설정할 수 있습니다.

```typescript
import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable } from 'rxjs';
import { map } from 'rxjs/operators';

@Injectable()
export class ExcludeNullInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next
      .handle()
      .pipe(map(value => value === null ? '' : value ));
  }
}
```

​	

### Exception mapping[#](https://docs.nestjs.com/interceptors#exception-mapping)

또 다른 흥미로운 사용 사례는 RxJS의 `catchError()` 연산자를 활용하여 던져진 예외를 재정의하는 것입니다.

```typescript
//errors.interceptor.ts

import {
  Injectable,
  NestInterceptor,
  ExecutionContext,
  BadGatewayException,
  CallHandler,
} from '@nestjs/common';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable()
export class ErrorsInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next
      .handle()
      .pipe(
        catchError(err => throwError(() => new BadGatewayException())),
      );
  }
}
```

​	

### Stream overriding[#](https://docs.nestjs.com/interceptors#stream-overriding)

핸들러 호출을 완전히 방지하고 대신에 다른 값을 반환하는 이유가 몇 가지 있습니다. 명백한 예로 응답 시간을 개선하기 위해 캐시를 구현하는 것이 있습니다. 캐시에서 응답을 반환하는 간단한 **캐시 인터셉터**를 살펴보겠습니다. 현실적인 예제에서는 TTL, 캐시 무효화, 캐시 크기 등과 같은 다른 요소도 고려해야 합니다. 하지만 이것은 이 토론의 범위를 벗어납니다. 여기서는 주요 컨셉을 보여주는 기본적인 예제를 제공하겠습니다.

```typescript
//cache.interceptor.ts

import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
import { Observable, of } from 'rxjs';

@Injectable()
export class CacheInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    const isCached = true;
    if (isCached) {
      return of([]);
    }
    return next.handle();
  }
}
```

우리의 `CacheInterceptor`에는 하드코딩된 `isCached` 변수와 하드코딩된 응답 `[]`이 있습니다. 주목해야 할 중요한 점은 RxJS의 `of()` 연산자에 의해 여기서 생성된 새로운 스트림을 반환하므로 라우트 핸들러는 **전혀 호출되지 않을 것**입니다. `CacheInterceptor`를 사용하는 엔드포인트를 호출하면 응답(하드코딩된 빈 배열)이 즉시 반환됩니다. 일반적인 솔루션을 만들려면 `Reflector`를 활용하여 사용자 정의 데코레이터를 만들 수 있습니다. `Reflector`는 [guards](https://docs.nestjs.com/guards) 챕터에서 잘 설명되어 있습니다.

​	

### More operators[#](https://docs.nestjs.com/interceptors#more-operators)

RxJS 연산자를 사용하여 스트림을 조작할 수 있는 가능성은 많은 기능을 제공합니다. 또 다른 일반적인 사용 사례를 살펴보겠습니다. 엔드포인트가 일정 시간이 지나서 아무것도 반환하지 않는 경우에, 에러를 반환 하고 싶을 것입니다. 다음 구성은 이를 가능하게 합니다.

```typescript
//timeout.interceptor.ts

import { Injectable, NestInterceptor, ExecutionContext, CallHandler, RequestTimeoutException } from '@nestjs/common';
import { Observable, throwError, TimeoutError } from 'rxjs';
import { catchError, timeout } from 'rxjs/operators';

@Injectable()
export class TimeoutInterceptor implements NestInterceptor {
  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
    return next.handle().pipe(
      timeout(5000),
      catchError(err => {
        if (err instanceof TimeoutError) {
          return throwError(() => new RequestTimeoutException());
        }
        return throwError(() => err);
      }),
    );
  };
};
```

5초 후에 요청 처리가 취소됩니다. `RequestTimeoutException`을 throw하기 전에 사용자 지정 논리를 추가할 수도 있습니다(예: 리소스 해제).

​	

​	

## Custom route decorators

Nest는 **데코레이터**라는 언어 기능을 중심으로 구축되었습니다. 데코레이터는 많은 일반적으로 사용되는 프로그래밍 언어에서 잘 알려진 개념이지만 JavaScript 세계에서는 여전히 비교적 새로운 개념입니다. 데코레이터 작동 방식을 더 잘 이해하기 위해 [이 기사](https://medium.com/google-developers/exploring-es7-decorators-76ecb65fb841)를 읽는 것이 좋습니다. 여기에 간단한 정의가 있습니다:

> ES2016(ES7) 데코레이터는 함수를 반환하는 표현식으로, 대상(target), 이름(name), 및 속성 기술자(property descriptor)를 인수로 받을 수 있습니다. 데코레이터를 적용하려면 데코레이터를 `@` 문자로 접두사로 붙이고 이를 꾸며주려는 대상의 맨 위에 배치하면 됩니다. 데코레이터는 클래스, 메서드 또는 속성에 대해 정의할 수 있습니다.

​	

### Param decorators[#](https://docs.nestjs.com/custom-decorators#param-decorators)

Nest는 HTTP 라우트 핸들러와 함께 사용할 수 있는 유용한 **파라미터 데코레이터** 세트를 제공합니다. 아래는 제공되는 데코레이터와 해당하는 일반적인 Express (또는 Fastify) 객체의 목록입니다.

|                            |                                      |
| -------------------------- | ------------------------------------ |
| `@Request(), @Req()`       | `req`                                |
| `@Response(), @Res()`      | `res`                                |
| `@Next()`                  | `next`                               |
| `@Session()`               | `req.session`                        |
| `@Param(param?: string)`   | `req.params` / `req.params[param]`   |
| `@Body(param?: string)`    | `req.body` / `req.body[param]`       |
| `@Query(param?: string)`   | `req.query` / `req.query[param]`     |
| `@Headers(param?: string)` | `req.headers` / `req.headers[param]` |
| `@Ip()`                    | `req.ip`                             |
| `@HostParam()`             | `req.hosts`                          |

추가로 사용자는 자신만의 **커스텀 데코레이터**를 만들 수도 있습니다. 왜 이것이 유용할까요?

node.js 세상에서는 요청 개체에 속성을 첨부하는 것이 일반적인 방법입니다. 그런 다음 각 라우터 핸들러에서 다음과 같은 코드를 사용하여 속성을 수동으로 추출합니다:

```typescript
const user = req.user;
```

코드를 더 읽기 쉽고 깔끔하게 만들기 위해 @User() 데코레이터를 만들고 모든 컨트롤러에서 코드를 재사용할 수 있습니다.

```typescript
//user.decorator.ts

import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const User = createParamDecorator(
  (data: unknown, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    return request.user;
  },
);
```

Then, you can simply use it wherever it fits your requirements.

```typescript
@Get()
async findOne(@User() user: UserEntity) {
  console.log(user);
}
```

​	

### Passing data[#](https://docs.nestjs.com/custom-decorators#passing-data)

데코레이터의 동작이 어떤 조건에 따라 달라질 때 `data` 매개변수를 사용하여 데코레이터 팩토리 함수에 인수를 전달할 수 있습니다. 이러한 경우 중 하나는 요청 객체에서 키별로 속성을 추출하는 사용자 정의 데코레이터입니다. 예를 들어 [인증 레이어](https://docs.nestjs.com/techniques/authentication#implementing-passport-strategies)가 요청을 유효성 검사하고 사용자 엔터티를 요청 객체에 첨부하는 경우, 인증된 요청에 대한 사용자 엔터티는 다음과 같을 수 있습니다:

````json
{
  "id": 101,
  "firstName": "Alan",
  "lastName": "Turing",
  "email": "alan@email.com",
  "roles": ["admin"]
}
````

해당 예제에서는 키로 속성 이름을 받아 해당 값이 있으면 반환하고 (없으면 `undefined` 반환하거나 `user` 객체가 생성되지 않은 경우도 해당), 사용자 엔터티의 특정 속성을 추출하는 데코레이터를 정의합니다.

```typescript
//user.decorator.ts

import { createParamDecorator, ExecutionContext } from '@nestjs/common';

export const User = createParamDecorator(
  (data: string, ctx: ExecutionContext) => {
    const request = ctx.switchToHttp().getRequest();
    const user = request.user;

    return data ? user?.[data] : user;
  },
);
```

다음은 컨트롤러에서 `@User()` 데코레이터를 사용하여 특정 속성에 액세스하는 방법의 예시입니다:

```typescript
@Get()
async findOne(@User('firstName') firstName: string) {
  console.log(`Hello ${firstName}`);
}
```

같은 데코레이터를 다른 키와 함께 사용하여 다양한 속성에 액세스할 수 있습니다. `user` 객체가 깊거나 복잡한 경우, 이렇게 함으로써 더 쉽고 가독성 있는 요청 핸들러를 구현할 수 있습니다.

> **HINT**
>
> TypeScript 사용자들을 위해 `createParamDecorator<T>()`가 제네릭임을 주목하세요. 이는 명시적으로 타입 안전성을 강제할 수 있음을 의미합니다. 예를 들어 `createParamDecorator<string>((data, ctx) => ...)`와 같이 사용할 수 있습니다. 또는 팩토리 함수에서 매개변수 타입을 지정할 수도 있습니다. 예를 들어 `createParamDecorator((data: string, ctx) => ...)`와 같이 사용할 수 있습니다. 둘 다 생략하는 경우 `data`의 타입은 `any`가 됩니다.

​	

### Working with pipes[#](https://docs.nestjs.com/custom-decorators#working-with-pipes)

Nest는 사용자 정의된 매개 변수 데코레이터를 내장된 것들 (`@Body()`, `@Param()` 및 `@Query()`)과 동일한 방식으로 처리합니다. 이는 사용자 정의 주석이 달린 매개 변수에 대해서도 파이프가 실행된다는 것을 의미합니다 (우리의 예제에서는 `user` 인자). 더 나아가 직접 사용자 정의 데코레이터에 파이프를 적용할 수도 있습니다:

```typescript
@Get()
async findOne(
  @User(new ValidationPipe({ validateCustomDecorators: true }))
  user: UserEntity,
) {
  console.log(user);
}
```

> **HINT**
>
> 주의: `validateCustomDecorators` 옵션을 true로 설정해야 합니다. `ValidationPipe`은 기본적으로 사용자 정의 데코레이터로 주석이 달린 인수를 유효성 검사하지 않습니다.

​	

### Decorator composition[#](https://docs.nestjs.com/custom-decorators#decorator-composition)

Nest는 여러 데코레이터를 결합하는 데 사용할 수 있는 도우미 메서드를 제공합니다. 예를 들어 인증과 관련된 모든 데코레이터를 하나의 데코레이터로 결합하려면 다음 구조를 사용할 수 있습니다:

```typescript
//auth.decorator.ts

import { applyDecorators } from '@nestjs/common';

export function Auth(...roles: Role[]) {
  return applyDecorators(
    SetMetadata('roles', roles),
    UseGuards(AuthGuard, RolesGuard),
    ApiBearerAuth(),
    ApiUnauthorizedResponse({ description: 'Unauthorized' }),
  );
}
```

You can then use this custom `@Auth()` decorator as follows:

```typescript
@Get('users')
@Auth('admin')
findAllUsers() {}
```

이렇게 하면 한 번의 선언으로 네 개의 데코레이터가 모두 적용됩니다.

> **WARNING**
>
> `@nestjs/swagger` 패키지의 `@ApiHideProperty()` 데코레이터는 합성 가능하지 않으며 `applyDecorators` 함수와 제대로 작동하지 않을 것입니다.

​	

​	

​	

​	

​	






