# typescript Refs_error


​	

# vue typescript_<span style="color: #56C4FF">$refs</span> error

>1. Object is possibly 'undefined'
>
>2. Property 'value' does not exist on type 'Vue | Element | (Vue | Element)[]'.       
>   Property 'value' does not exist on type 'Vue'

**typescript를 적용한 vue에서 $refs 값을 가져와 사용하는데 위와 같은 문제가 발생했습니다.**

​		

## 에러가 나오는 이유

개발자가 보기에는 너무 당연히 데이터가 있지만, 객체가 비어 있을 수도 있는데 해당 객체의 내부 메소드를 사용하거나 내부 객체 키에 값을 넣어주려고 할 때 발생합니다.

```typescript
const refs = this.$refs.image		//👈 여기서 에러 발생
const name = refs.value.name
```

​		

옵셔널체이닝, if문으로 undefined 거르기 등을 시도해 보았지만, 오류가 계속 되었고 **타입 단언 사용**으로 해결하였습니다.

```typescript
// eslint-disable-next-line
const refs = this.$refs.image as any	//👈
const name = refs.value.name
```

any 타입으로 지정해주며 깔끔히 무시해버리기.

​		

​		

관련해 자료를 찾아보니 별로 권장되는 방법은 아닌 것 같습니다. 다만, 저는 옵셔널 체이닝과 if문으로 걸리지지 않았기 때문에 적용해 보았고 해결이 되어 그냥 진행하기로 하였습니다.

​		

## 🤔선택적으로 적용해보세요. 아마, 옵셔널체이닝이 가장 권장되는 방법이 아닐까 예측해봅니다.

​	

​	


