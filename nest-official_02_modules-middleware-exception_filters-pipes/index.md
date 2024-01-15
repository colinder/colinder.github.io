# Nest Official_02_(Modules, Middleware, Exception_filters, Pipes)


​	

## Modules

모듈은 `@Module()` 데코레이터로 주석이 달린 클래스입니다. `@Module()` 데코레이터는 Nest가 애플리케이션 구조를 구성하는 데 사용하는 메타데이터를 제공합니다.

각 어플리케이션은 최소한 하나의 모듈, **루트 모듈**을 가지고 있습니다. **루트 모듈**은 Nest가 어플리케이션 그래프를 구축하는데 사용하는 시작점입니다. **어플리케이션 그래프**는 Nest가 Modules 및 Providers 간의 관계와 의존성을 해결하는 데 사용하는 내부 데이터 구조입니다. 매우 작은 애플리케이션의 경우 이론적으로 루트 모듈만을 가질 수 있지만, 이는 일반적인 경우가 아닙니다. 모듈은 구성 요소를 효과적으로 구성하는 데 강력하게 권장되는 방법이며, 대부분의 애플리케이션에서는 각각이 밀접한 관련성을 가진 일련의 기능을 캡슐화하는 여러 모듈을 사용하는 결과로서의 아키텍처가 형성됩니다.

`@Module()` 데코레이터는 모듈을 설명하는 속성들이 담긴 단일 객체를 인자로 받습니다.

|             |                                                              |
| ----------- | :----------------------------------------------------------- |
| providers   | the providers that will be instantiated by the Nest injector and that may be shared at least across this module |
| controllers | the set of controllers defined in this module which have to be instantiated |
| imports     | the list of imported modules that export the providers which are required in this module |
| exports     | the subset of `providers` that are provided by this module and should be available in other modules which import this module. You can use either the provider itself or just its token (`provide` value) |

**기본적으로 모듈은 프로바이더를 캡슐화합니다.** **이는 현재 모듈에 직접 속하지 않거나 가져온 모듈에서 내보내지지 않은 프로바이더를 주입하는 것이 불가능하다는 것을 의미**합니다. 따라서 모듈에서 내보내는 프로바이더를 모듈의 공개 인터페이스 또는 API로 간주할 수 있습니다.

​	

### Feature modules[#](https://docs.nestjs.com/modules#feature-modules)

 `CatsController`와 `CatsService`는 동일한 애플리케이션 도메인에 속합니다. 서로 밀접하게 관련되어 있기 때문에 이들을 **feature module**로 이동하는 것이 합리적입니다. **feature module**은 단순히 특정 기능에 관련된 코드를 조직하는 역할을 합니다. 코드를 조직하고 명확한 경계를 정의함으로써 복잡성을 관리하고 애플리케이션 또는 팀의 규모가 커짐에 따라 SOLID 원칙에 따라 개발하는 데 도움이 됩니다.

이를 구현하기 위해  `CatsModule`을 만듭니다.

```typescript
//cats/cats.module.ts

import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {}
```

> **HINT**
>
> To create a module using the CLI, simply execute the `$ nest g module cats` command.

위에 우리는  `cats.module.ts`파일 안에  `CatsModule`을 정의하고, 이 모듈과 관련된 모든 것을 `cats` 디렉토리로 이동했습니다. 마지막으로 해야 할 일은 이 모듈을 루트 모듈(`app.module.ts` 파일에서 정의된 `AppModule`)에 가져오는 것입니다.

```typescript
// app.module.ts

import { Module } from '@nestjs/common';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule {}
```

이제 우리 폴더 구조는 아래와 같습니다.

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
> &nbsp;&nbsp;&nbsp;&nbsp;└ cats.module.ts  👈
>
> &nbsp;&nbsp;&nbsp;&nbsp;└ cats.service.ts
>
> app.module.ts
>
> main.ts

​		

### Shared modules[#](https://docs.nestjs.com/modules#shared-modules)

Nest에서는 모듈이 기본적으로 **싱글톤**이며, 따라서 동일한 프로바이더 인스턴스를 여러 모듈 간에 손쉽게 공유할 수 있습니다.

모든 모듈은 자동적으로 공유된 모듈입니다. 한 번 생성되면 어떤 모듈안에서든지 재사용이 가능합니다. 여러 다른 모듈 간에 `CatsService`의 인스턴스를 공유하고자 한다면, 먼저 해당 모듈의 `exports` 배열에 `CatsService` 프로바이더를 추가하여 내보내야 합니다. 아래와 같이 작성됩니다:

```typescript
//cats.module.ts

import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService]  👈
})
export class CatsModule {}
```

이제 `CatsModule`을 가져오는 모든 모듈은 `CatsService`에 접근할 수 있으며, 이를 가져오는 다른 모든 모듈과 동일한 인스턴스를 공유합니다.

​	

### Module re-exporting[#](https://docs.nestjs.com/modules#module-re-exporting)

위에서 볼 수 있듯이 모듈은 내부 프로바이더를 내보낼 수 있습니다. 또한, 가져온 모듈을 다시 내보낼 수도 있습니다. 아래 예시에서는 `CommonModule`이 `CoreModule`에 가져와지고 동시에 내보내져서, 이를 가져오는 다른 모듈에서 사용할 수 있게 됩니다.

```typescript
@Module({
  imports: [CommonModule],
  exports: [CommonM odule],
})
export class CoreModule {}

```

​	

### Dependency injection[#](https://docs.nestjs.com/modules#dependency-injection)

모듈 클래스는 providers에게 주입할 수 있습니다.(e.g., for configuration purposes): 

```typescript
// cats.module.ts

import { Module } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Module({
  controllers: [CatsController],
  providers: [CatsService],
})
export class CatsModule {
  constructor(private catsService: CatsService) {}
}
```

그러나 모듈 클래스 자체는 원형 의존성([circular dependency](https://docs.nestjs.com/fundamentals/circular-dependency))으로 인해 프로바이더로 주입될 수 없습니다

​	

### Global modules[#](https://docs.nestjs.com/modules#global-modules)

만약 항상 동일한 모듈 세트를 어디서나 가져와야 하는 경우 번거로워질 수 있습니다. Nest에서 Angular와 달리 `providers`는 전역 범위에 등록되지 않습니다. 한 번 정의되면 해당 providers는 어디에서나 사용 가능합니다. 그러나 Nest는 providers를 모듈 스코프 내에 캡슐화합니다. 캡슐화된 모듈을 가져오지 않고는 해당 모듈의 providers를 다른 곳에서 사용할 수 없습니다.

만약 어디서나 사용 가능한 프로바이더 집합을 제공하려면 (예: 헬퍼, 데이터베이스 연결 등) 모듈에 `@Global()` 데코레이터를 사용하여 **전역**으로 설정하세요.

```typescript
import { Module, Global } from '@nestjs/common';
import { CatsController } from './cats.controller';
import { CatsService } from './cats.service';

@Global()  👈
@Module({
  controllers: [CatsController],
  providers: [CatsService],
  exports: [CatsService],
})
export class CatsModule {}
```

`@Global()` 데코레이터는 모듈을 전역 범위로 설정합니다. 전역 모듈은 일반적으로 루트 또는 코어 모듈에서 한 번만 등록되어야 합니다. 위의 예제에서 `CatsService` providers는 어디서나 사용 가능하며, **서비스를 주입하려는 모듈은 imports 배열에 `CatsModule`을 가져오지 않아도 될 것입니다.**

> **HINT**
>
> Making everything global is not a good design decision. Global modules are available to reduce the amount of necessary boilerplate. The `imports` array is generally the preferred way to make the module's API available to consumers.

​	

### Dynamic modules[#](https://docs.nestjs.com/modules#dynamic-modules)

Nest 모듈 시스템에는 **동적 모듈**이라는 강력한 기능이 포함되어 있습니다. 이 기능을 사용하면 동적으로 프로바이더를 등록하고 구성할 수 있는 사용자 정의 가능한 모듈을 쉽게 만들 수 있습니다. 동적 모듈은  [here](https://docs.nestjs.com/fundamentals/dynamic-modules)에서 자세하게 다루어져 있습니다. 이 장에서는 모듈 소개를 완료하기 위해 간략한 개요를 제공합니다.

다음은 `DatabaseModule`의 동적 모듈 정의 예시입니다:

```typescript
import { Module, DynamicModule } from '@nestjs/common';
import { createDatabaseProviders } from './database.providers';
import { Connection } from './connection.provider';

@Module({
  providers: [Connection],
})
export class DatabaseModule {
  static forRoot(entities = [], options?): DynamicModule {
    const providers = createDatabaseProviders(options, entities);
    return {
      module: DatabaseModule,
      providers: providers,
      exports: providers,
    };
  }
}
```

> **HINT**
>
> `forRoot()` 메서드는 동기적으로 또는 비동기적으로(즉, Promise를 통해) 동적 모듈을 반환할 수 있습니다.

이 모듈은 기본적으로 `Connection` provider를 정의하지만(`@Module()` 데코레이터 메타데이터에서), 추가로 `forRoot()` 메서드에 전달된 `entities` 및 `options` 객체에 따라 provider 컬렉션을 노출합니다.(예를 들면 리포지토리들) 동적 모듈에서 반환된 속성은 `@Module()` 데코레이터에서 정의된 기본 모듈 메타데이터를 확장(override가 아닌)하는 것에 주목하세요. 이것이 정적으로 선언된 `Connection` provider와 동적으로 생성된 리포지토리 provider가 모듈에서 내보내지는 방식입니다.

동적 모듈을 전역 범위에서 등록하려면 `global` 속성을 `true`로 설정하세요.

```typescript
{
  global: true,
  module: DatabaseModule,
  providers: providers,
  exports: providers,
}
```

> **WARNING**
>
> As mentioned above, making everything global **is not a good design decision**.

`DatabaseModule`은 다음과 같은 방식으로 가져와 구성될 수 있습니다:

```typescript
import { Module } from '@nestjs/common';
import { DatabaseModule } from './database/database.module';
import { User } from './users/entities/user.entity';

@Module({
  imports: [DatabaseModule.forRoot([User])],
})
export class AppModule {}
```

만약 동적 모듈을 다시 내보내려면, exports 배열에서 `forRoot()` 메서드 호출을 생략할 수 있습니다:

```typescript
import { Module } from '@nestjs/common';
import { DatabaseModule } from './database/database.module';
import { User } from './users/entities/user.entity';

@Module({
  imports: [DatabaseModule.forRoot([User])],
  exports: [DatabaseModule],
})
export class AppModule {}
```

The [Dynamic modules](https://docs.nestjs.com/fundamentals/dynamic-modules) chapter covers this topic in greater detail, and includes a [working example](https://github.com/nestjs/nest/tree/master/sample/25-dynamic-modules).

> **HINT**
>
> Learn how to build highly customizable dynamic modules with the use of `ConfigurableModuleBuilder` here in [this chapter](https://docs.nestjs.com/fundamentals/dynamic-modules#configurable-module-builder).

​	

​	

## Middleware

미들웨어는 라우트 핸들러 이전에 호출되는 함수입니다. 미들웨어 함수는 `요청` 및 `응답` 객체에 액세스할 수 있으며, 애플리케이션의 요청-응답 주기에서 `next()` 미들웨어 함수에 액세스할 수 있습니다. 다음 미들웨어 함수는 일반적으로 `next`라는 변수로 표시됩니다.

Nest 미들웨어는 기본적으로 express 미들웨어와 동등합니다. 다음은 공식 express 문서에서 가져온 미들웨어의 기능에 대한 설명입니다:

> Middleware functions can perform the following tasks:
>
> - execute any code.
> - make changes to the request and the response objects.
> - end the request-response cycle.
> - call the next middleware function in the stack.
> - if the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging. (현재의 미들웨어 함수가 요청-응답 주기를 종료하지 않으면 next()를 호출하여 제어를 다음 미들웨어 함수로 전달해야 합니다. 그렇지 않으면 요청이 미해결 상태로 남게 됩니다.)

Nest 미들웨어는 함수로도, 또는 `@Injectable()` 데코레이터를 사용한 클래스로 구현할 수 있습니다. 클래스는 `NestMiddleware` 인터페이스를 구현해야 하며, 함수는 특별한 요구사항이 없습니다. 간단한 미들웨어 기능을 클래스 방법을 사용하여 구현하는 것으로 시작해보겠습니다.

> **WARNING**
>
> `Express` and `fastify` handle middleware differently and provide different method signatures, read more [here](https://docs.nestjs.com/techniques/performance#middleware).

```typescript
//logger.middleware.ts

import { Injectable, NestMiddleware } from '@nestjs/common';
import { Request, Response, NextFunction } from 'express';

@Injectable()
export class LoggerMiddleware implements NestMiddleware {
  use(req: Request, res: Response, next: NextFunction) {
    console.log('Request...');
    next();
  }
}
```

​	

### Dependency injection[#](https://docs.nestjs.com/middleware#dependency-injection)

Nest 미들웨어는 완전히 의존성 주입(Dependency Injection)을 지원합니다. 마치 프로바이더(Providers)와 컨트롤러(Controllers)와 마찬가지로, 미들웨어도 동일한 모듈 내에서 사용 가능한 의존성을 주입할 수 있습니다. 전통적으로 이는 생성자(Constructor)를 통해 이루어집니다.

​	

### Applying middleware[#](https://docs.nestjs.com/middleware#applying-middleware)

Nest.js에서는 `@Module()` 데코레이터에 미들웨어를 직접 설정할 수 있는 장소가 없습니다. 대신, 모듈 클래스의 `configure()` 메서드를 사용하여 미들웨어를 설정합니다. 미들웨어를 포함하는 모듈은 `NestModule` 인터페이스를 구현해야 합니다. `AppModule` 수준에서 `LoggerMiddleware`를 설정해 보겠습니다.

```typescript
//app.module.ts

import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes('cats');
  }
}
```

위의 예제에서는 이전에 `CatsController` 내에서 정의된 `/cats` 라우트 핸들러에 대해 `LoggerMiddleware`를 설정했습니다. 또한, 미들웨어를 특정 요청 메서드로 제한할 수 있으며, 이는 미들웨어를 구성할 때 `forRoutes()` 메서드에 라우트 경로와 요청 메서드를 포함하는 객체를 전달하여 수행할 수 있습니다. 

아래 예제에서는 원하는 요청 메서드 유형을 참조하기 위해 `RequestMethod`열거형을 가져오는 것에 주목하세요.

```typescript
//app.module.ts

import { Module, NestModule, RequestMethod, MiddlewareConsumer } from '@nestjs/common'; 👈
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes({ path: 'cats', method: RequestMethod.GET });  👈
  }
}
```

> **HINT**
>
> `configure()` 메서드는 `async/await`를 사용하여 비동기적으로 만들 수 있습니다. 즉, `configure()` 메서드 본문 내에서 비동기 작업의 완료를 `await`할 수 있습니다.

> **WARNING**
>
> `express` 어댑터를 사용할 때, NestJS 앱은 기본적으로 `body-parser` 패키지에서 제공하는 `json` 및 `urlencoded` 미들웨어를 등록합니다. 따라서 `MiddlewareConsumer`를 통해 이 미들웨어를 사용자 정의하려면 `NestFactory.create()`로 애플리케이션을 생성할 때 `bodyParser` 플래그를 `false`로 설정하여 전역 미들웨어를 비활성화해야 합니다.

​	

### Route wildcards[#](https://docs.nestjs.com/middleware#route-wildcards)

패턴 기반 라우트도 지원됩니다. 예를 들어, 별표(*)는 와일드카드로 사용되며 어떠한 문자 조합이든 일치합니다:

```typescript
forRoutes({ path: 'ab*cd', method: RequestMethod.ALL });
```

`'ab*cd'` 라우트 경로는 `abcd`, `ab_cd`, `abecd` 등과 일치합니다. 라우트 경로에는 `?` , `+` , `*` 및 `()`와 같은 문자가 사용될 수 있으며, 이들은 정규 표현식과 관련된 부분 집합입니다. 하이픈(`-`)과 점(`.`)은 문자열 기반 경로에서 글자 그대로 해석됩니다.

> **WARNING**
>
> The `fastify` package uses the latest version of the `path-to-regexp` package, which no longer supports wildcard asterisks `*`. Instead, you must use parameters (e.g., `(.*)`, `:splat*`).

​	

### Middleware consumer[#](https://docs.nestjs.com/middleware#middleware-consumer)

`MiddlewareConsumer`은 도우미 클래스로, 미들웨어를 관리하기 위한 여러 내장 메서드를 제공합니다. 이들은 모두 간편하게 [fluent style](https://en.wikipedia.org/wiki/Fluent_interface)로 연결될 수 있습니다. `forRoutes()` 메서드는 단일 문자열, 여러 문자열, `RouteInfo` 객체, 컨트롤러 클래스, 심지어 여러 컨트롤러 클래스를 인수로 받을 수 있습니다. 대부분의 경우 쉼표로 구분된 컨트롤러 목록을 전달할 것입니다. 아래는 단일 컨트롤러를 사용한 예제입니다:

```typescript
//app.moduls.ts

import { Module, NestModule, MiddlewareConsumer } from '@nestjs/common';
import { LoggerMiddleware } from './common/middleware/logger.middleware';
import { CatsModule } from './cats/cats.module';
import { CatsController } from './cats/cats.controller';

@Module({
  imports: [CatsModule],
})
export class AppModule implements NestModule {
  configure(consumer: MiddlewareConsumer) {
    consumer
      .apply(LoggerMiddleware)
      .forRoutes(CatsController);
  }
}
```

> **HINT**
>
> `apply()` 메서드는 단일 미들웨어를 취할 수도 있고, [다중 미들웨어를 지정](https://docs.nestjs.com/middleware#multiple-middleware)하기 위해 여러 인수를 취할 수도 있습니다.

​	

### Excluding routes[#](https://docs.nestjs.com/middleware#excluding-routes)

때로는 특정 라우트에서 미들웨어를 **적용하지 않도록** 하고 싶을 때가 있습니다. `exclude()` 메서드를 사용하여 특정 라우트를 쉽게 제외할 수 있습니다. 이 메서드는 제외할 라우트를 식별하는 단일 문자열, 여러 문자열 또는 `RouteInfo` 객체를 인수로 받을 수 있습니다. 아래는 예시입니다:

```typescript
consumer
  .apply(LoggerMiddleware)
  .exclude(
    { path: 'cats', method: RequestMethod.GET },
    { path: 'cats', method: RequestMethod.POST },
    'cats/(.*)',
  )
  .forRoutes(CatsController);

// forRoutes 뒤에 exclude를 사용하면 에러발생
```

> **HINT**
>
> The `exclude()` method supports wildcard parameters using the [path-to-regexp](https://github.com/pillarjs/path-to-regexp#parameters) package.

위의 예제에서는 `LoggerMiddleware`가 `CatsController` 내에서 정의된 모든 라우트에 바인딩되지만, `exclude()` 메서드에 전달된 세 개의 라우트를 제외한 나머지 라우트에 적용될 것입니다.

​	

### Functional middleware[#](https://docs.nestjs.com/middleware#functional-middleware)

우리가 사용한 `LoggerMiddleware` 클래스는 매우 간단합니다. 멤버나 추가적인 메서드, 의존성이 없습니다. 왜 클래스 대신에 간단한 함수로 정의할 수 없을까요? 사실, 가능합니다. 이러한 유형의 미들웨어를 **함수형 미들웨어**라고 합니다. 클래스 기반의 `LoggerMiddleware`를 함수형 미들웨어로 변환해보겠습니다. 그러면 두 방식의 차이점을 이해할 수 있습니다:

```typescript
import { Request, Response, NextFunction } from 'express';

export function logger(req: Request, res: Response, next: NextFunction) {
  console.log(`Request...`);
  next();
};
```

> **HINT**
>
> 당신의 미들웨어가 어떠한 의존성도 필요로하지 않을 때에는, 더 간단한 대안인 **함수형 미들웨어** 사용을 고려하세요.

​	

### Multiple middleware[#](https://docs.nestjs.com/middleware#multiple-middleware)

위에서 언급한대로 여러 미들웨어를 순차적으로 실행하려면, `apply()` 메서드 내부에 쉼표로 구분된 목록을 제공하면 됩니다:

```typescript
consumer.apply(cors(), helmet(), logger).forRoutes(CatsController);
```

​	

### Global middleware[#](https://docs.nestjs.com/middleware#global-middleware)

만약 모든 등록된 라우트에 미들웨어를 한 번에 바인딩하려면, `INestApplication`인스턴스에서 제공하는 `use()` 메서드를 사용할 수 있습니다:

```typescript
//main.ts

const app = await NestFactory.create(AppModule);
app.use(logger);
await app.listen(3000);
```

> **HINT**
>
> 전역 미들웨어에서 <u>*DI container</u>에 접근하는 것은 불가능합니다. `app.use()`를 사용할 때는 대신 [함수형 미들웨어](https://docs.nestjs.com/middleware#functional-middleware)를 사용할 수 있습니다. 또는 클래스 미들웨어를 사용하고 `AppModule` (또는 다른 모듈) 내에서 `.forRoutes('*')`를 사용하여 소비할 수 있습니다.

<span style='color: gray; font-size: 0.8em'>*DI container : DI(Dependency Injection) 컨테이너는 의존성 주입을 관리하고 제공하는 메커니즘입니다. NestJS에서는 내장된 DI 컨테이너를 사용하여 애플리케이션의 여러 부분 간에 의존성을 주입하고 관리합니다. <br>DI는 코드를 더 모듈화하고 재사용 가능하게 만들며, 테스트와 같은 측면에서도 이점을 제공합니다.DI 컨테이너는 클래스 인스턴스를 생성하고, 클래스의 생성자에 필요한 의존성을 해결하여 객체 간의 의존성을 자동으로 처리합니다. 이를 통해 코드는 느슨한 결합을 유지하고, 유연성과 테스트 용이성을 증가시킵니다. <br>간단히 말해, DI 컨테이너는 애플리케이션에서 사용되는 다양한 서비스, 컴포넌트, 및 의존성들을 효율적으로 관리하는 역할을 합니다. NestJS에서는 이 DI 컨테이너를 통해 의존성을 주입하고 모듈을 효과적으로 조직화할 수 있습니다.</span>

​	

​	

## Exception filters

Nest는 애플리케이션 전반에 걸쳐 모든 처리되지 않은 예외를 처리하는 내장 예외 레이어를 제공합니다. 애플리케이션 코드에서 처리되지 않은 예외가 발생하면 이 레이어에 의해 잡히며, 자동으로 적절한 사용자 친화적인 응답을 전송합니다.

기본적으로 이 동작은 내장된 전역 예외 필터에 의해 수행됩니다. 이 예외 필터는 HttpException 형식의 예외 (그 하위 클래스 포함)를 처리합니다. 예외가 인식되지 않을 때 (HttpException이나 HttpException에서 상속되지 않은 클래스), 내장된 예외 필터는 다음과 같은 기본 JSON 응답을 생성합니다:

```json
{
  "statusCode": 500,
  "message": "Internal server error"
}
```

> **HINT**
>
> 전역 예외 필터는 `http-errors` 라이브러리를 일부 지원합니다. 기본적으로 `statusCode` 및 `message` 속성을 포함하는 모든 발생한 예외는 적절하게 채워져 응답으로 반환됩니다 (인식되지 않은 예외에 대한 기본 `InternalServerErrorException` 대신).

​	

### Throwing standard exceptions[#](https://docs.nestjs.com/exception-filters#throwing-standard-exceptions)

Nest는 `@nestjs/common` 패키지에서 노출되는 내장 `HttpException` 클래스를 제공합니다. 전형적인 HTTP REST/GraphQL API 기반 애플리케이션에서는 특정 오류 조건이 발생할 때 표준 HTTP 응답 객체를 보내는 것이 좋은 관행입니다.

예를 들어, `CatsController`에는 `findAll()` 메서드(한 `GET` 라우트 핸들러)가 있습니다. 이 라우트 핸들러가 어떤 이유로든 예외를 throw한다고 가정해 봅시다. 이를 시연하기 위해 다음과 같이 하드 코딩해 보겠습니다:

```typescript
//cats.controller.ts
import { Controller, Get, HttpException, HttpStatus } from '@nestjs/common';

@Get()
async findAll() {
  throw new HttpException('Forbidden', HttpStatus.FORBIDDEN);
}
```

> **HINT**
>
> We used the `HttpStatus` here. This is a helper enum imported from the `@nestjs/common` package.

클라이언트가 이 엔드포인트를 호출하면 응답은 다음과 같을 것입니다:

```json
{
  "statusCode": 403,
  "message": "Forbidden"
}
```


HttpException 생성자는 응답을 결정하는 두 가지 필수 인수를 받습니다:

- response 인수는 JSON 응답 본문을 정의합니다. 아래에 설명된대로 문자열 또는 객체가 될 수 있습니다.

- status 인수는 [HTTP status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)를 정의합니다.

기본적으로 JSON 응답 본문은 두 가지 속성을 포함합니다:

- `statusCode`: status 인수에서 제공된 HTTP 상태 코드로 기본 설정됩니다.
- `message`: status를 기반으로 한 HTTP 오류에 대한 간단한 설명

JSON `응답` 본문의 메시지 부분만 덮어쓰려면 response 인수에 문자열을 제공하면 됩니다. 전체 JSON 응답 본문을 덮어쓰려면 `response` 인수에 객체를 전달하면 됩니다. Nest는 객체를 직렬화하고 이를 JSON 응답 본문으로 반환합니다.

두 번째 생성자 인수인 `status`는 유효한 HTTP 상태 코드여야 합니다. 관례적으로 `@nestjs/common`에서 가져온 `HttpStatus` 열거형을 사용하는 것이 좋습니다.

**세 번째** 생성자 인수 (선택적) options는 오류  [cause](https://nodejs.org/en/blog/release/v16.9.0/#error-cause)을 제공하는 데 사용할 수 있습니다. 이 `원인` 객체는 응답 객체로 직렬화되지 않지만 `HttpException`이 throw되는 데 원인이 된 내부 오류에 대한 유용한 정보를 제공하는 데 유용할 수 있습니다.

전체 응답 본문을 덮어쓰고 오류 원인을 제공하는 예시입니다:

```typescript
//cats.controller.ts

@Get()
async findAll() {
  try {
    await this.service.findAll()
  } catch (error) { 
    throw new HttpException({
      status: HttpStatus.FORBIDDEN,
      error: 'This is a custom message',
    }, HttpStatus.FORBIDDEN, {
      cause: error
    });
  }
}
```

Using the above, this is how the response would look:

```json
{
  "status": 403,
  "error": "This is a custom message"
}
```

​	

### Custom exceptions[#](https://docs.nestjs.com/exception-filters#custom-exceptions)

대부분의 경우 사용자 정의 예외를 작성할 필요가 없으며, 다음 섹션에서 설명하는대로 내장된 Nest HTTP 예외를 사용할 수 있습니다. 사용자 정의 예외를 만들어야 하는 경우에는 사용자 정의 예외를 기본 `HttpException` 클래스에서 상속하는 예외 계층을 만드는 것이 좋은 관행입니다. 이 접근 방식을 사용하면 Nest가 사용자 정의 예외를 인식하고 오류 응답을 자동으로 처리할 것입니다. 이러한 사용자 정의 예외를 구현해 보겠습니다:

```typescript
//forbidden.exception.ts

export class ForbiddenException extends HttpException {
  constructor() {
    super('Forbidden', HttpStatus.FORBIDDEN);
  }
}
```

`ForbiddenException`이 기본 `HttpException`을 확장하므로 내장된 예외 처리기와 원활하게 작동하며, 따라서 `findAll()` 메서드 내에서 사용할 수 있습니다.

​	

### Built-in HTTP exceptions[#](https://docs.nestjs.com/exception-filters#built-in-http-exceptions)

Nest는 기본 `HttpException`을 상속하는 일련의 표준 예외를 제공합니다. 이 예외들은 `@nestjs/common` 패키지에서 노출되며, 가장 일반적인 HTTP 예외 중 많은 것들을 나타냅니다:

- `BadRequestException`
- `UnauthorizedException`
- `NotFoundException`
- `ForbiddenException`
- `NotAcceptableException`
- `RequestTimeoutException`
- `ConflictException`
- `GoneException`
- `HttpVersionNotSupportedException`
- `PayloadTooLargeException`
- `UnsupportedMediaTypeException`
- `UnprocessableEntityException`
- `InternalServerErrorException`
- `NotImplementedException`
- `ImATeapotException`
- `MethodNotAllowedException`
- `BadGatewayException`
- `ServiceUnavailableException`
- `GatewayTimeoutException`
- `PreconditionFailedException`

내장 예외들은 options 매개변수를 사용하여 오류 원인과 오류 설명을 모두 제공할 수도 있습니다:

```typescript
throw new BadRequestException('Something bad happened', { cause: new Error(), description: 'Some error description' })
```

위의 내용을 사용하면 응답은 다음과 같이 보일 것입니다:

```json
{
  "message": "Something bad happened",
  "error": "Some error description",
  "statusCode": 400,
}
```

​	

### Exception filters[#](https://docs.nestjs.com/exception-filters#exception-filters-1)

기본(내장) 예외 필터는 많은 경우를 자동으로 처리할 수 있지만 예외 레이어에 대한 완전한 제어가 필요한 경우가 있습니다. 예를 들어 로깅을 추가하거나 어떤 동적 요소를 기반으로 다른 JSON 스키마를 사용하고 싶을 수 있습니다. 예외 필터는 정확히 이러한 목적으로 설계되었습니다. 이를 사용하면 흐름 제어와 클라이언트로 보내는 응답의 내용을 정확하게 제어할 수 있습니다.

HttpException 클래스의 인스턴스인 예외를 catch하고 해당 예외에 대해 사용자 정의 응답 로직을 구현하는 예외 필터를 만들어 보겠습니다. 이를 위해 기본 플랫폼 Request 및 Response 객체에 액세스해야 합니다. 원본 url을 가져와 로깅 정보에 포함시키기 위해 Request 객체에 액세스합니다. response.json() 메서드를 사용하여 직접 전송되는 응답을 제어하기 위해 Response 객체를 사용할 것입니다.

```typescript
//http-exception.filter.ts

import { ExceptionFilter, Catch, ArgumentsHost, HttpException } from '@nestjs/common';
import { Request, Response } from 'express';

@Catch(HttpException)   //1️⃣
export class HttpExceptionFilter implements ExceptionFilter {
  catch(exception: HttpException, host: ArgumentsHost) {     //2️⃣ 
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();
    const status = exception.getStatus();

    response
      .status(status)
      .json({
        statusCode: status,
        timestamp: new Date().toISOString(),
        path: request.url,
      });
  }
}
```

> **HINT**
>
> 모든 예외 필터는 일반적인 `ExceptionFilter<T>` 인터페이스를 구현해야 합니다. 이것은 `catch(exception: T, host: ArgumentsHost)` 메서드를 해당 시그니처와 함께 제공해야 한다는 것을 요구합니다. `T`는 예외의 유형을 나타냅니다.

> **WARNING**
>
> If you are using `@nestjs/platform-fastify` you can use `response.send()` instead of `response.json()`. Don't forget to import the correct types from `fastify`.

1️⃣`@Catch(HttpException)` 데코레이터는 예외 필터에 필요한 메타데이터를 바인딩하며, 이를 통해 Nest에게 이 특정 필터가 HttpException 유형의 예외를 찾고 있으며 다른 유형의 예외는 찾지 않는다고 알려줍니다. `@Catch()` 데코레이터는 단일 매개변수 또는 쉼표로 구분된 목록을 허용할 수 있습니다. 이를 통해 한 번에 여러 유형의 예외에 대한 필터를 설정할 수 있습니다.

​	

### Arguments host[#](https://docs.nestjs.com/exception-filters#arguments-host)

2️⃣ `catch()` 메서드의 매개변수를 살펴보겠습니다. `exception` 매개변수는 현재 처리 중인 예외 객체입니다. `host` 매개변수는 `ArgumentsHost` 객체입니다. `ArgumentsHost`는 [<u>*실행 컨텍스트 장</u>](https://docs.nestjs.com/fundamentals/execution-context)에서 자세히 살펴볼 강력한 유틸리티 객체입니다. 이 코드 샘플에서는 `ArgumentsHost`에 대한 참조를 사용하여 원본 요청 핸들러에 전달되는 `Request` 및 `Response` 객체에 대한 참조를 얻습니다(예외가 발생한 컨트롤러에서). 이 코드 샘플에서는 `ArgumentsHost`의 몇 가지 도우미 메서드를 사용하여 원하는 `Request` 및 `Response` 객체를 가져왔습니다. `ArgumentsHost`에 대해 더 알아보려면 [여기](https://docs.nestjs.com/fundamentals/execution-context)를 참조하세요.

<span style="color: gray; font-size: 0.8em">*이 추상화 수준의 이유는 'ArgumentsHost'가 모든 컨텍스트에서 기능하기 때문입니다(예를 들어 현재 작업 중인 HTTP 서버 컨텍스트 뿐만 아니라 Microservices 및 WebSockets도 포함됨). 실행 컨텍스트 장에서는 `ArgumentsHost` 및 해당 도우미 함수의 강력함을 활용하여 **모든** 실행 컨텍스트에 대한 [기본 인수에 액세스](https://docs.nestjs.com/fundamentals/execution-context#host-methods)하는 방법을 살펴볼 것입니다. 이를 통해 모든 컨텍스트에서 작동하는 일반적인 예외 필터를 작성할 수 있게 됩니다.</span>

​	

### Binding filters[#](https://docs.nestjs.com/exception-filters#binding-filters)

새로운 `HttpExceptionFilter`를 `CatsController`의 `create()` 메서드에 연결해 보겠습니다.

```typescript
//cats.controller.ts

import { Controller, Post, UseFilters } from '@nestjs/common';

@Post()
@UseFilters(new HttpExceptionFilter())
async create(@Body() createCatDto: CreateCatDto) {
  throw new ForbiddenException();
}
```

> **HINT**
>
> The `@UseFilters()` decorator is imported from the `@nestjs/common` package.

여기서 `@UseFilters()` 데코레이터를 사용했습니다. `@Catch()` 데코레이터와 유사하게 단일 필터 인스턴스 또는 필터 인스턴스의 쉼표로 구분된 목록을 사용할 수 있습니다. 여기서는 `HttpExceptionFilter`의 인스턴스를 생성했습니다. 또는 클래스를 전달하고 (인스턴스 대신) 프레임워크에 인스턴스화 책임을 남기고 **의존성 주입**을 활성화할 수 있습니다.

```typescript
//cats.controller.ts

@Post()
@UseFilters(HttpExceptionFilter)
async create(@Body() createCatDto: CreateCatDto) {
  throw new ForbiddenException();
}
```

> **HINT**
>
> 가능하면 인스턴스 대신 클래스를 사용하여 필터를 적용하는 것이 좋습니다. 이렇게 하면 Nest가 동일한 클래스의 인스턴스를 모듈 전체에서 쉽게 재사용할 수 있어 **메모리 사용량이 감소**합니다.

위의 예제에서는 HttpExceptionFilter가 단일 create() 라우트 핸들러에만 적용되어 메서드 범위로 설정되었습니다. 예외 필터는 메서드 범위의 컨트롤러/리졸버/게이트웨이, 컨트롤러 범위, 또는 전역 범위에서 범위를 설정할 수 있습니다. 예를 들어 필터를 컨트롤러 범위로 설정하려면 다음과 같이 할 수 있습니다:

```typescript
//cats.controller.ts

@UseFilters(new HttpExceptionFilter())
export class CatsController {}
```

이 구성은 `CatsController` 내에서 정의된 모든 라우트 핸들러에 대해 `HttpExceptionFilter`를 설정합니다.

글로벌 범위의 필터를 만들려면 다음을 수행합니다:

```typescript
//main.ts

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalFilters(new HttpExceptionFilter());
  await app.listen(3000);
}
bootstrap();
```

> **WARNING**
>
> The `useGlobalFilters()` method does not set up filters for gateways or hybrid applications.

글로벌 범위의 필터는 모든 컨트롤러 및 모든 라우트 핸들러에 걸쳐 전체 애플리케이션에서 사용됩니다. 의존성 주입 측면에서 모듈의 컨텍스트 외부에서 등록된 글로벌 필터 (위의 예제와 같이 `useGlobalFilters()`를 사용함)는 모듈의 컨텍스트 외부에서 수행되므로 종속성을 주입할 수 없습니다. 이 문제를 해결하려면 다음 구성을 사용하여 **어떤 모듈에서든 직접** 글로벌 범위의 필터를 등록할 수 있습니다:

```typescript
//app.module.ts

import { Module } from '@nestjs/common';
import { APP_FILTER } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_FILTER,
      useClass: HttpExceptionFilter,
    },
  ],
})
export class AppModule {}
```

> **HINT**
>
> 이 방법을 사용하여 필터에 대한 종속성 주입을 수행할 때 이 구조가 사용되는 모듈에 관계없이 필터는 사실상 전역적입니다. 이 작업은 어디에서 수행해야 할까요? 필터(위 예제의 `HttpExceptionFilter`)가 정의된 모듈을 사용(Choose)하십시오. 또한 `useClass`가 사용자 지정 공급자 등록을 처리하는 유일한 방법은 아닙니다. 자세한 내용은 [여기](https://docs.nestjs.com/fundamentals/custom-providers)를 참조하십시오.

이 기술을 사용하여 필요한 만큼 많은 필터를 추가할 수 있습니다. 각각을 providers 배열에 추가하면 됩니다.

​	

### Catch everything[#](https://docs.nestjs.com/exception-filters#catch-everything)

관리하지 않은 모든 예외를 잡기 위해서는 (예외 유형과 관계없이) `@Catch()`데코레이터의 매개변수 목록을 비워둡니다. 예: `@Catch()`

아래 예제에서는 [HTTP adapter](https://docs.nestjs.com/faq/http-adapter)를 사용하여 응답을 전달하며 플랫폼에 구애받지 않는 코드입니다. 또한 플랫폼별 객체 (`Request` 및 `Response`)를 직접 사용하지 않습니다.

```typescript
import {
  ExceptionFilter,
  Catch,
  ArgumentsHost,
  HttpException,
  HttpStatus,
} from '@nestjs/common';
import { HttpAdapterHost } from '@nestjs/core';

@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  constructor(private readonly httpAdapterHost: HttpAdapterHost) {}

  catch(exception: unknown, host: ArgumentsHost): void {
    // In certain situations `httpAdapter` might not be available in the
    // constructor method, thus we should resolve it here.
    const { httpAdapter } = this.httpAdapterHost;

    const ctx = host.switchToHttp();

    const httpStatus =
      exception instanceof HttpException
        ? exception.getStatus()
        : HttpStatus.INTERNAL_SERVER_ERROR;

    const responseBody = {
      statusCode: httpStatus,
      timestamp: new Date().toISOString(),
      path: httpAdapter.getRequestUrl(ctx.getRequest()),
    };

    httpAdapter.reply(ctx.getResponse(), responseBody, httpStatus);
  }
}
```

> **WARNING**
>
> When combining an exception filter that catches everything with a filter that is bound to a specific type, the "Catch anything" filter **should be declared** first to allow the specific filter to correctly handle the bound type.

​	

### Inheritance(상속)[#](https://docs.nestjs.com/exception-filters#inheritance)

일반적으로 애플리케이션 요구 사항을 충족하는 데 사용되는 완전히 사용자 정의된 예외 필터를 만들 것입니다. 그러나 특정 요소를 기반으로 내용을 덮어쓰기 위해 내장된 기본 글로벌 예외 필터를 확장하기를 원하는 경우가 있을 수 있습니다.

예외 처리를 기본 필터에 위임하려면 `BaseExceptionFilter`를 확장하고 상속된 `catch()`메서드를 호출해야 합니다.

```typescript
//all-exceptions.filter.ts

import { Catch, ArgumentsHost } from '@nestjs/common';
import { BaseExceptionFilter } from '@nestjs/core';

@Catch()
export class AllExceptionsFilter extends BaseExceptionFilter {
  catch(exception: unknown, host: ArgumentsHost) {
    super.catch(exception, host);
  }
}
```

> **WARNING**
>
> `BaseExceptionFilter`를 확장하는 메서드 및 컨트롤러 범위 필터는 `new`로 인스턴스화해서는 안됩니다. 대신 프레임워크가 자동으로 인스턴스화하도록 해야 합니다.

위의 구현은 접근 방식을 보여주는 뼈대일 뿐입니다. 확장된 예외 필터의 실제 구현에는 특정한 **비즈니스** 로직이 포함됩니다(예: 여러 조건을 처리).

전역 필터는 기본 필터를 확장**할 수 있습**니다. 이는 두 가지 방법 중 하나로 수행될 수 있습니다.

첫 번째 방법은 사용자 정의 전역 필터를 인스턴스화할 때 `HttpAdapter` 참조를 주입하는 것입니다.

```typescript
async function bootstrap() {
  const app = await NestFactory.create(AppModule);

  const { httpAdapter } = app.get(HttpAdapterHost);
  app.useGlobalFilters(new AllExceptionsFilter(httpAdapter));

  await app.listen(3000);
}
bootstrap();
```

두 번째 방법은 APP_FILTER 토큰을 사용하는 것입니다. [as shown here](https://docs.nestjs.com/exception-filters#binding-filters).

​	

​	

## Pipes

> **클라이언트로부터 받은 데이터를 애플리케이션에서 사용할 수 있는 형식으로 변환하거나, 유효성 검사, 가공 등의 작업을 수행하는데 사용되는 중간 레이어**

파이프는 `@Injectable()` 데코레이터로 주석이 달린 클래스로, `PipeTransform` 인터페이스를 구현합니다.

"Pipes"는 주로 두 가지 사용 사례가 있습니다:

1. **변환 (Transformation):** 입력 데이터를 원하는 형식으로 변환하는 역할을 합니다. 예를 들어, 문자열에서 정수로 변환하거나, 날짜 형식을 변경하는 등의 작업이 여기에 해당됩니다.
2. **유효성 검사 (Validation):** 입력 데이터를 평가하고 유효한 경우에는 변경 없이 그대로 전달하며, 그렇지 않으면 예외를 발생시킵니다. 입력 데이터의 유효성을 확인하는 역할을 수행합니다.

두 경우 모두 파이프는 [컨트롤러 라우트 핸들러](https://docs.nestjs.com/controllers#route-parameters)에서 처리되는 `인수`에 작용합니다. Nest는 메서드가 호출되기 직전에 파이프를 삽입하고, 파이프는 해당 메서드에 전달되는 인수에서 작동합니다. 변환 또는 유효성 검사 작업은 그때 수행되며, 이후 라우트 핸들러는 (가능한 경우) 변환된 인수로 호출됩니다.

Nest에는 즉시 사용할 수 있는 여러 내장 파이프가 제공됩니다. 또한 사용자 정의 파이프를 만들 수도 있습니다. 이 장에서는 내장 파이프를 소개하고 이를 라우트 핸들러에 바인딩하는 방법을 보여줄 것입니다. 그런 다음 몇 가지 처음부터 파이프를 만드는 방법을 살펴보겠습니다.

> **HINT**
>
> 파이프는 예외 영역 내에서 실행됩니다. 즉, 파이프에서 예외가 발생하면 예외 레이어 (전역 예외 필터 및 현재 컨텍스트에 적용된 모든 [예외 필터](https://docs.nestjs.com/exception-filters))에서 처리됩니다. 위의 내용을 고려하면 파이프에서 예외가 발생하면 후속으로 컨트롤러 메서드가 실행되지 않음을 명확히 알 수 있습니다. 이는 외부 소스에서 애플리케이션으로 들어오는 데이터를 시스템 경계에서 유효성 검사하는 데 사용되는 최선의 실천 기법을 제공합니다.

​	

### Built-in pipes[#](https://docs.nestjs.com/pipes#built-in-pipes)

Nest는 기본적으로 아래 9개의 파이프를 제공합니다:

1. **ValidationPipe:** 기본적인 형식 유효성 검사를 제공합니다. 유효성 검사 실패 시 예외를 throw합니다.
2. **ParseIntPipe:** 전달된 값이 유효한 정수인지 확인하고, 정수로 변환합니다. 실패 시 예외를 throw합니다.
3. **ParseFloatPipe:** 전달된 값이 유효한 부동 소수점 숫자인지 확인하고, 부동 소수점으로 변환합니다. 실패 시 예외를 throw합니다.
4. **ParseBoolPipe:** 전달된 값이 유효한 부울 값인지 확인하고, 부울 값으로 변환합니다. 실패 시 예외를 throw합니다.
5. **ParseUUIDPipe:** 전달된 값이 UUID 형식인지 확인하고, UUID로 변환합니다. 실패 시 예외를 throw합니다.
6. **DefaultValuePipe:** 기본값을 지정할 수 있습니다. 값이 주어지지 않은 경우, 지정된 기본값을 사용합니다.
7. **TransformPipe:** 사용자 정의 변환 함수를 적용하여 값을 변환합니다.
8. **ValidationPipe:** 전달된 값이 유효한 DTO(Data Transfer Object)인지 확인하고, 실패 시 예외를 throw합니다.
9. **ValidationPipe:** DTO의 생성자에서 자동으로 타입 유효성을 검사합니다. 실패 시 예외를 throw합니다.

They're exported from the `@nestjs/common` package.

Let's take a quick look at using `ParseIntPipe`. This is an example of the **transformation** use case, where the pipe ensures that a method handler parameter is converted to a JavaScript integer (or throws an exception if the conversion fails). Later in this chapter, we'll show a simple custom implementation for a `ParseIntPipe`. The example techniques below also apply to the other built-in transformation pipes (`ParseBoolPipe`, `ParseFloatPipe`, `ParseEnumPipe`, `ParseArrayPipe` and `ParseUUIDPipe`, which we'll refer to as the `Parse*` pipes in this chapter).

`ParseIntPipe`를 사용하는 간단한 예제를 살펴보겠습니다. 이는 **변환**(use case)의 예시로, 파이프는 메서드 핸들러의 매개변수를 JavaScript 정수로 변환하는 것을 보장하거나(변환이 실패하면 예외를 throw), 나중에 이 장에서는 `ParseIntPipe`에 대한 간단한 사용자 정의 구현을 보여줄 것입니다. 아래의 예제 기법은 이 장에서 다루는 다른 내장 변환 파이프(`ParseBoolPipe`, `ParseFloatPipe`, `ParseEnumPipe`, `ParseArrayPipe` 및 `ParseUUIDPipe`, 여기서는 `Parse*` 파이프로 참조합니다)에도 적용됩니다.

​	

### Binding pipes[#](https://docs.nestjs.com/pipes#binding-pipes)

파이프를 사용하려면 파이프 클래스의 인스턴스를 적절한 컨텍스트에 연결(바인딩)해야 합니다. `ParseIntPipe` 예제에서는 파이프를 특정 라우트 핸들러 메서드와 연관시키고, 해당 메서드가 호출되기 전에 파이프가 실행되도록 하려고 합니다. 이를 메서드 매개변수 레벨에서 파이프를 바인딩한다고 표현하겠습니다.

```typescript
@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

이러면 두 조건 중 하나가 참이라는 것을 보장합니다. `findOne()` 메서드에서 받는 매개변수가 숫자이거나(as expected in our call to `this.catsService.findOne()`), 라우터 핸들러에서 예외를 전달했거나.

예를 들어, 다음과 같이 라우트가 호출된다고 가정해보겠습니다:

```http
GET localhost:3000/abc
```

Nest는 아래와 같은 예외를 줄 것입니다.

```json
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}
```

예외는  `findOne()`메서드의 본문 실행을 방지할 것입니다.

위의 예제에서는 클래스(`ParseIntPipe`)를 인스턴스로 전달하는 것이 아니라 프레임워크에 인스턴스화 책임을 맡겨 의존성 주입을 가능하게 합니다. 파이프와 가드와 마찬가지로 대신 in-place instance를 전달할 수 있습니다. in-place instance를 전달하는 것은 옵션을 전달하여 내장 파이프의 동작을 사용자 정의하고 싶을 때 유용합니다.

```typescript
@Get(':id')
async findOne(
  @Param('id', new ParseIntPipe({ errorHttpStatusCode: HttpStatus.NOT_ACCEPTABLE }))
  id: number,
) {
  return this.catsService.findOne(id);
}
```

다른 변환 파이프들 (**Parse*** 파이프들)을 바인딩하는 방법도 유사합니다. 이러한 파이프들은 모두 라우트 매개변수, 쿼리 문자열 매개변수 및 요청 본문 값의 유효성을 검사하는 맥락에서 작동합니다.

예를 들어 쿼리 문자열 매개변수와 함께 사용할 때:

```typescript
@Get()
async findOne(@Query('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

여기에서는 `ParseUUIDPipe`를 사용하여 문자열 매개변수를 구문 분석하고 해당 값이 UUID인지 유효성을 검사하는 예제입니다.

```typescript
@Get(':uuid')
async findOne(@Param('uuid', new ParseUUIDPipe()) uuid: string) {
  return this.catsService.findOne(uuid);
}
```

> **HINT**
>
> When using `ParseUUIDPipe()` you are parsing UUID in version 3, 4 or 5, if you only require a specific version of UUID you can pass a version in the pipe options.

위에서는 다양한 내장 파이프인 __Parse*__ 패밀리를 바인딩하는 예제를 살펴보았습니다. 유효성 검사 파이프를 바인딩하는 것은 조금 다릅니다. 다음 섹션에서 이에 대해 논의하겠습니다.

> **HINT**
>
> [Validation techniques](https://docs.nestjs.com/techniques/validation) 섹션에서는 유효성 검사 파이프에 대한 포괄적인 예제를 자세히 살펴볼 수 있습니다.

​	

### Custom pipes[#](https://docs.nestjs.com/pipes#custom-pipes)

언급한대로 사용자 정의 파이프를 만들 수 있습니다. Nest는 강력한 내장 `ParseIntPipe`와 `ValidationPipe`를 제공하지만, 각각을 처음부터 간단한 사용자 정의 버전으로 만들어보면 어떻게 사용자 정의 파이프가 구성되는지 확인할 수 있습니다.

먼저 간단한 `ValidationPipe`부터 시작하겠습니다. 처음에는 입력 값을 받아들이고 즉시 동일한 값을 반환하는 식별 함수처럼 동작하도록 만들어 보겠습니다.

```typescript
//validation.pipe.ts

import { PipeTransform, Injectable, ArgumentMetadata } from '@nestjs/common';

@Injectable()
export class ValidationPipe implements PipeTransform {
  transform(value: any, metadata: ArgumentMetadata) {
    return value;
  }
}
```

> **HINT**
>
> `PipeTransform<T, R>`은 어떠한 파이프든 반드시 구현해야 하는 제네릭 인터페이스입니다. 이 제네릭 인터페이스는 변환 로직을 정의하기 위한 구조를 제공합니다. 제네릭 타입 `T`는 입력 `value`의 타입을 나타내고, `R`은 `transform()` 메서드의 반환 타입을 나타냅니다.

각 파이프는 `PipeTransform` 인터페이스 계약을 충족시키기 위해 `transform()` 메서드를 구현해야 합니다. 이 메서드는 두 개의 매개변수를 갖습니다:

- `value`

- `metadata`

`value` 매개변수는 현재 처리 중인 메서드 인자(argument)를 나타내며(라우트 처리 메서드에서 수신되기 전), `metadata`는 현재 처리 중인 메서드 인자(argument)의 메타데이터를 나타냅니다. 메타데이터 객체에는 다음과 같은 속성들이 있습니다:

```typescript
export interface ArgumentMetadata {
  type: 'body' | 'query' | 'param' | 'custom';
  metatype?: Type<unknown>;
  data?: string;
}
```

이러한 속성들은 현재 처리 중인 인자(argument)를 설명합니다.

|          |                                                              |
| -------- | ------------------------------------------------------------ |
| type     | 해당 속성은 현재 처리 중인 매개변수가 body `@Body()`, query `@Query()`, param `@Param()` 또는 사용자 정의 매개변수인지 여부를 나타냅니다. (자세한 내용은 [여기](https://docs.nestjs.com/custom-decorators)에서 확인하세요.) |
| metatype | 이 속성은 인자(argument)의 메타타입을 제공합니다. 예를 들어 `String`일 수 있습니다. <br />주의: 라우트 핸들러 메서드 시그니처에서 타입 선언을 생략하거나 일반 JavaScript를 사용한 경우 값은 `undefined`입니다. |
| data     | 데코레이터에 전달된 문자열, 예를 들면 `@Body('string')`에서의 `'string'`입니다. 데코레이터 괄호를 비워 둔 경우 `undefined`입니다. |

> **WARNING**
>
> TypeScript 인터페이스는 트랜스파일 중에 사라집니다. 따라서 메서드 매개변수의 타입이 클래스 대신 인터페이스로 선언된 경우 `metatype` 값은 `Object`가 될 것입니다.

​	

### Schema based validation[#](https://docs.nestjs.com/pipes#schema-based-validation)

우리의 유효성 검사 파이프를 좀 더 유용하게 만들어 봅시다. 아마도 `CatsController`의 `create()` 메서드를 자세히 살펴보면, 서비스 메서드를 실행하기 전에 게시 본문 객체가 유효한지 확인하고 싶을 것입니다.

```typescript
@Post()
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

Let's focus in on the `createCatDto` body parameter. Its type is `CreateCatDto`:

```typescript
//create-cat.dto.ts

export class CreateCatDto {
  name: string;
  age: number;
  breed: string;
}
```

우리는 create 메서드로 들어오는 모든 요청이 유효한 body를 포함하고있다고 보장하고자 합니다. 따라서 `createCatDto` 객체의 세 멤버를 검증해야 합니다. 이 작업을 라우트 핸들러 메서드 내부에서 수행할 수 있지만, 이렇게 하는 것은 단일 책임 원칙(**single responsibility principle** (SRP))을 깨뜨리게 됩니다.

또 다른 접근 방법은 **validator class**를 만들고 해당 작업을 거기에 위임하는 것입니다. 이 방법은 각 메서드의 시작 부분에서이 유효성 검사기를 호출하는 것을 기억해야 한다는 단점이 있습니다.

유효성 검사 미들웨어를 생성하는 것은 어떨까요? 이는 작동할 수 있지만 유감스럽게도 애플리케이션 전체에서 모든 컨텍스트에서 사용할 수 있는 **generic middleware**를 생성하는 것은 불가능합니다. 이는 미들웨어가 **execution context**, 호출될 핸들러 및 해당 매개변수 등에 대해 알지 못하기 때문입니다.

물론 이런 상황이 파이프가 설계된 정확한 사용 사례입니다. 그러니 이제 우리의 유효성 검사 파이프를 더 정교하게 만들어 봅시다.

​	

### Object schema validation[#](https://docs.nestjs.com/pipes#object-schema-validation)

객체 유효성 검사를 깨끗하고 DRY(Don't Repeat Yourself)한 방식으로 수행하는 여러 가지 접근 방법이 있습니다. 일반적인 접근 방법 중 하나는 스키마 기반 유효성 검사를 사용하는 것입니다. 이 접근 방법을 시도해 보겠습니다.

Zod 라이브러리를 사용하면 읽기 쉬운 API로 간단하게 스키마를 생성할 수 있습니다. Zod 기반 스키마를 활용하는 유효성 검사 파이프를 만들어 보겠습니다.

먼저 필요한 패키지를 설치하겠습니다:

```cmd
$ npm install --save zod
```

아래의 코드 샘플에서는 스키마를 `constructor` 인자로 받는 간단한 클래스를 만듭니다. 그런 다음 스키마를 통해 들어오는 인수를 검증하는 `schema.parse()` 메서드를 적용합니다.

앞서 언급한 대로 **validation pipe** 파이프는 값이 변경되지 않거나 예외를 throw하는 두 가지 동작 중 하나를 수행합니다.

다음 섹션에서는 `@UsePipes()` 데코레이터를 사용하여 특정 컨트롤러 메서드에 적절한 스키마를 제공하는 방법을 볼 것입니다. 이를 통해 목표로한 대로 유효성 검사 파이프를 다양한 컨텍스트에서 재사용할 수 있게 됩니다.

```typescript
import { PipeTransform, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { ZodSchema  } from 'zod';

export class ZodValidationPipe implements PipeTransform {
  constructor(private schema: ZodSchema) {}

  transform(value: unknown, metadata: ArgumentMetadata) {
    try {
      const parsedValue = this.schema.parse(value);
      return parsedValue;
    } catch (error) {
      throw new BadRequestException('Validation failed');
    }
  }
}
```

​	

### Binding validation pipes[#](https://docs.nestjs.com/pipes#binding-validation-pipes)

이전에는 변환 파이프(`ParseIntPipe` 및 나머지 `Parse*` 파이프와 같은)를 바인딩하는 방법을 살펴보았습니다.

유효성 검사 파이프를 바인딩하는 것도 매우 간단합니다.

이 경우에는 메서드 호출 수준에서 파이프를 바인딩하고자 합니다. 현재 예제에서 `ZodValidationPipe`를 사용하려면 다음과 같은 작업을 수행해야 합니다:

- ZodValidationPipe의 인스턴스 생성

- 파이프의 클래스 생성자에서 컨텍스트별 Zod 스키마 전달

- 파이프를 메서드에 바인딩

Zod 스키마 예제:

```typescript
import { z } from 'zod';

export const createCatSchema = z
  .object({
    name: z.string(),
    age: z.number(),
    breed: z.string(),
  })
  .required();

export type CreateCatDto = z.infer<typeof createCatSchema>;
```

We do that using the `@UsePipes()` decorator as shown below:

```typescript
//cats.controller.ts

@Post()
@UsePipes(new ZodValidationPipe(createCatSchema))
async create(@Body() createCatDto: CreateCatDto) {
  this.catsService.create(createCatDto);
}
```

> **HINT**
>
> The `@UsePipes()` decorator is imported from the `@nestjs/common` package.

> **WARNING**
>
> `zod` 라이브러리를 사용하려면 `tsconfig.json` 파일에서 `strictNullChecks` 구성이 활성화되어 있어야 합니다.

​	

### Class validator[#](https://docs.nestjs.com/pipes#class-validator)

> **WARNING**
>
> The techniques in this section require TypeScript and are not available if your app is written using vanilla JavaScript.

우리의 유효성 검사 기술에 대한 대안적인 구현을 살펴봅시다.

Nest는 [class-validator](https://github.com/typestack/class-validator) 라이브러리와 잘 동작합니다. 이 강력한 라이브러리를 사용하면 데코레이터 기반 유효성 검사를 수행할 수 있습니다. 데코레이터 기반 유효성 검사는 특히 Nest의 **Pipe**기능과 결합될 때 매우 강력합니다. 왜냐하면 우리는 처리된 프로퍼티의 `메타타입`에 접근할 수 있기 때문입니다. 시작하기 전에 필요한 패키지를 설치해야 합니다:

```cmd
$ npm i --save class-validator class-transformer
```

이러한 패키지를 설치한 후에는 `CreateCatDto` 클래스에 몇 가지 데코레이터를 추가할 수 있습니다. 이 기술의 중요한 장점 중 하나는 `CreateCatDto` 클래스가 Post 본문 객체의 단일 진실의 소스로 유지된다는 것입니다 (별도의 유효성 검사 클래스를 만들 필요가 없습니다).

```typescript
//create-cat.dto.ts

import { IsString, IsInt } from 'class-validator';

export class CreateCatDto {
  @IsString()
  name: string;

  @IsInt()
  age: number;

  @IsString()
  breed: string;
}
```

> **HINT**
>
> Read more about the class-validator decorators [here](https://github.com/typestack/class-validator#usage).

Now we can create a `ValidationPipe` class that uses these annotations.

```typescript
//validation.pipe.ts

import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';
import { validate } from 'class-validator';
import { plainToInstance } from 'class-transformer';

@Injectable()
export class ValidationPipe implements PipeTransform<any> {
  async transform(value: any, { metatype }: ArgumentMetadata) {
    if (!metatype || !this.toValidate(metatype)) {
      return value;
    }
    const object = plainToInstance(metatype, value);
    const errors = await validate(object);
    if (errors.length > 0) {
      throw new BadRequestException('Validation failed');
    }
    return value;
  }

  private toValidate(metatype: Function): boolean {
    const types: Function[] = [String, Boolean, Number, Array, Object];
    return !types.includes(metatype);
  }
}
```

> **HINT**
>
> `ValidationPipe`은 Nest에서 기본적으로 제공되기 때문에 사용자 정의 유효성 검사 파이프를 직접 작성할 필요는 없습니다. 내장된 `ValidationPipe`에는 이 장에서 만든 샘플보다 더 많은 옵션이 제공되며, 이 샘플은 사용자 정의 파이프의 메커니즘을 설명하기 위해 기본적으로 유지되었습니다. 자세한 내용 및 다양한 예제는 [여기](https://docs.nestjs.com/techniques/validation)에서 확인할 수 있습니다.

> **NOTICE**
>
> We used the [class-transformer](https://github.com/typestack/class-transformer) library above which is made by the same author as the **class-validator** library, and as a result, they play very well together.

이 코드를 살펴보겠습니다. 먼저, `transform()` 메서드가 `async`로 표시되어 있습니다. 이는 Nest가 동기적 및 비동기적 파이프를 모두 지원하기 때문에 가능합니다. 이 메서드를 `async`로 만든 이유는 class-validator의 일부 검증이 [비동기일 수 있기 때문](https://github.com/typestack/class-validator#custom-validation-classes)입니다 (Promises를 사용합니다).

다음으로 destructuring을 사용하여 `metatype` 필드를 (`ArgumentMetadata`에서 이 멤버만 추출하여) `metatype` 매개변수로 할당하고 있습니다. 이는 `ArgumentMetadata` 전체를 얻는 대신 추가 문을 추가하여 `metatype` 변수를 할당하는 단축 표기법일 뿐입니다.

다음으로 `toValidate()`라는 도우미 함수에 주목하세요. 이 함수는 현재 처리 중인 인수가 기본 JavaScript 타입인 경우 (이들에는 유효성 검사 데코레이터가 연결될 수 없으므로 유효성 검사 단계를 실행할 필요가 없습니다) 검증 단계를 우회하는 역할을 합니다.

다음으로 `plainToInstance()` 함수를 사용하여 일반 JavaScript 인수 객체를 타입이 지정된 객체로 변환하여 유효성을 적용합니다. 이를 수행해야 하는 이유는 네트워크 요청에서 역직렬화된 들어오는 post body 객체에는 **어떤 타입 정보도 없기 때문**입니다 (이는 Express와 같은 기본 플랫폼의 동작 방식입니다). Class-validator는 앞서 DTO에 정의한 유효성 데코레이터를 사용해야 하므로 이 변환을 수행하여 들어오는 본문을 적절히 데코레이트된 객체로 처리해야 합니다.

마지막으로 앞서 언급했듯이 이는 **유효성 검사 파이프**이기 때문에 값이 변경되지 않거나 예외를 throw하는 두 가지 동작 중 하나를 수행합니다.

마지막 단계는 `ValidationPipe`를 바인딩하는 것입니다. 파이프는 매개변수 스코프, 메서드 스코프, 컨트롤러 스코프 또는 글로벌 스코프로 바인딩될 수 있습니다. 앞서 Zod 기반의 유효성 검사 파이프에서 메서드 레벨에서 파이프를 바인딩하는 예제를 보았습니다. 아래의 예제에서는 파이프 인스턴스를 라우트 핸들러의 `@Body()` 데코레이터에 바인딩하여 파이프가 호출되어 post body를 유효성 검사하도록 만듭니다.

```typescript
//cats.controller.ts

@Post()
async create(
  @Body(new ValidationPipe()) createCatDto: CreateCatDto,
) {
  this.catsService.create(createCatDto);
}
```

매개변수 스코프 파이프는 유효성 검사 로직이 특정 매개변수에만 관련된 경우 유용합니다.

​	

### Global scoped pipes[#](https://docs.nestjs.com/pipes#global-scoped-pipes)

`ValidationPipe`은 가능한 한 일반적으로 만들어졌기 때문에 **전역 스코프** 파이프로 설정하여 전체 애플리케이션에서 모든 라우트 핸들러에 적용할 수 있습니다.

```typescript
//main.ts

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  app.useGlobalPipes(new ValidationPipe());
  await app.listen(3000);
}
bootstrap();
```

> **NOTICE**
>
> [hybrid apps](https://docs.nestjs.com/faq/hybrid-application)의 경우 `useGlobalPipes()` 메서드는 게이트웨이 및 마이크로서비스를 위한 파이프를 설정하지 않습니다. "표준" (비하이브리드) 마이크로서비스 앱의 경우 `useGlobalPipes()`는 파이프를 전역으로 마운트합니다.

전역 파이프는 전체 애플리케이션, 모든 컨트롤러 및 모든 라우트 핸들러에서 사용됩니다.

의존성 주입 관점에서 주의할 점은 모듈 외부에서 등록된 전역 파이프(`useGlobalPipes()`를 사용한 경우와 같이)는 모듈의 컨텍스트 외부에서 바인딩되었기 때문에 의존성을 주입할 수 없습니다. 이 문제를 해결하기 위해 이 구성을 사용하여 **어떤 모듈에서든 직접** 전역 파이프를 설정할 수 있습니다:

```typescript
//app.module.ts

import { Module } from '@nestjs/common';
import { APP_PIPE } from '@nestjs/core';

@Module({
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe,
    },
  ],
})
export class AppModule {}
```

> **HINT**
>
> 파이프에 대한 의존성 주입을 수행하기 위해이 접근 방식을 사용할 때, 이 구성이 사용된 모듈에 관계없이 파이프는 사실상 전역입니다. 이를 어디에 설정해야 할까요? 파이프(위의 예제에서는 `ValidationPipe`)가 정의된 모듈을 선택하세요. 또한 `useClass`는 사용자 지정 프로바이더 등록을 다루는 유일한 방법이 아닙니다. 여기에서 더 알아보세요.



​	

### The built-in ValidationPipe[#](https://docs.nestjs.com/pipes#the-built-in-validationpipe)

기억하세요. `ValidationPipe`은 Nest에서 기본적으로 제공되기 때문에 사용자 정의 유효성 검사 파이프를 직접 작성할 필요는 없습니다. 내장된 `ValidationPipe`은 이 장에서 만든 샘플보다 더 많은 옵션을 제공하며, 이 샘플은 사용자 정의 파이프의 메커니즘을 설명하기 위해 기본으로 유지되었습니다. 자세한 내용과 다양한 예제는 [여기](https://docs.nestjs.com/techniques/validation)에서 확인할 수 있습니다.

​	

### Transformation use case[#](https://docs.nestjs.com/pipes#transformation-use-case)

유효성 검사는 사용자 정의 파이프의 유일한 사용 사례가 아닙니다. 이 장의 시작에서 파이프가 입력 데이터를 원하는 형식으로 **변환**할 수도 있다고 언급했습니다. 이는 `transform` 함수에서 반환된 값이 인수의 이전 값 전체를 완전히 대체할 수 있기 때문에 가능합니다.

이것이 유용한 경우는 언제일까요? 때로는 클라이언트에서 전달된 데이터가 라우트 핸들러 메서드에서 적절하게 처리되기 전에 어떠한 변경을 거쳐야 하는 경우가 있습니다. 예를 들어 문자열을 정수로 변환해야 할 수 있습니다. 또한 일부 필수 데이터 필드가 누락된 경우 기본값을 적용하고 싶을 수 있습니다. **변환 파이프**는 이러한 기능을 수행할 수 있으며 클라이언트 요청과 요청 핸들러 사이에 처리 함수를 끼워넣어 작동합니다.

다음은 문자열을 정수 값으로 파싱하는 역할을 담당하는 간단한 `ParseIntPipe`의 예제입니다. (앞서 언급한 대로 Nest에는 더 정교한 내장 `ParseIntPipe`가 있습니다. 이것은 사용자 정의 변환 파이프의 간단한 예로 포함되어 있습니다).

```typescript
//parse-int.pipe.ts

import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';

@Injectable()
export class ParseIntPipe implements PipeTransform<string, number> {
  transform(value: string, metadata: ArgumentMetadata): number {
    const val = parseInt(value, 10);
    if (isNaN(val)) {
      throw new BadRequestException('Validation failed');
    }
    return val;
  }
}
```

We can then bind this pipe to the selected param as shown below:

```typescript
@Get(':id')
async findOne(@Param('id', new ParseIntPipe()) id) {
  return this.catsService.findOne(id);
}
```

또 다른 유용한 변환 사례는 요청에서 제공된 ID를 사용하여 데이터베이스에서 **기존 사용자** 엔터티를 선택하는 것일 수 있습니다:

```typescript
@Get(':id')
findOne(@Param('id', UserByIdPipe) userEntity: UserEntity) {
  return userEntity;
}
```

이 파이프의 구현은 독자에게 맡겨둡니다. 그러나 다른 모든 변환 파이프와 마찬가지로 이는 입력 값(`id`)을 받아 출력 값(`UserEntity` 객체)을 반환합니다. 이렇게 하면 공통 파이프로 핸들러에서의 반복 코드를 추상화하여 코드를 더 선언적이고 [DRY](https://en.wikipedia.org/wiki/Don't_repeat_yourself)하게 만들 수 있습니다.

​	

### Providing defaults[#](https://docs.nestjs.com/pipes#providing-defaults)

`Parse*` 파이프들은 매개변수의 값을 정의되어 있다고 기대합니다. `null` 또는 `undefined` 값을 받으면 예외를 throw합니다. 쿼리 문자열 매개변수 값이 누락된 경우에 대비해 엔드포인트가 이를 처리하도록 하려면 `Parse*` 파이프가 이러한 값을 처리하기 전에 주입될 기본값을 제공해야 합니다. 이를 위해 `DefaultValuePipe`가 사용됩니다. 간단히 아래와 같이 관련된 `Parse*` 파이프 앞에 `@Query()` 데코레이터에서 `DefaultValuePipe`를 인스턴스화하면 됩니다:

```typescript
@Get()
async findAll(
  @Query('activeOnly', new DefaultValuePipe(false), ParseBoolPipe) activeOnly: boolean,
  @Query('page', new DefaultValuePipe(0), ParseIntPipe) page: number,
) {
  return this.catsService.findAll({ activeOnly, page });
}
```

​	

​	

​	

​	

​	


