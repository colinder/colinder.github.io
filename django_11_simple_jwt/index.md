# Django_11_simple_JWT


​	

# Django JWT(Json Web Token)

> Django의 대표적인 jwt 패키지는 [djangorestframework-jwt](https://jpadilla.github.io/django-rest-framework-jwt/), [djangorestframework-simplejwt](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/)가 있지만 전자는 업데이트가 더이상 진행되지 않아서 후자를 사용하는 것을 추천합니다.

​	

## djangorestframework-simplejwt

> 간단한 사용법 정리

```python 
def Usage(request):
    ## SIMPLEJWT METHOD TEST
    # print("================================")
    # a = JWTAuthentication()
    # b = a.get_header(request)
    # print(b)
    # print("================================")
    # raw_token = a.get_raw_token(b)
    # print(raw_token)
    # print("================================")
    # d = a.get_validated_token(raw_token)
    # print('유효한가', d)
    # print("================================")
    # auth = a.authenticate(request)
    # print(auth)
    # print(auth[0])
    # print("=================================")
    # # print(auth[1])
    # epoch_time = 1637654588
    # date_time = datetime.fromtimestamp( epoch_time )  
    # print("Given epoch time:", epoch_time)  
    # print("Converted Datetime:", date_time )
    # ts= (datetime.now() - datetime(1970,1,1)).total_seconds()
    # print('현재시간 에포크로 변환', round(ts))
    return JsonResponse({'detail': 'ok', "messages": {"message": "true"}}, status=200)
```

​	

​	

​	

​	

​	

