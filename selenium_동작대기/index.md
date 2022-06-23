# Selenium 동작 대기 방법


​	

# Selenium 동작대기

접근하려는 요소가 존재하지 않을 때 발생하는 **NoSuchElementException** error.
페이지와 서버가 통신중이거나 네트워크 지연 등으로 위의 크롤링을 원하는 요소가 html에 존재하기 전에 요소에 접근을 시도하기 때문에 발생하는 에러입니다.

어떤 멋진 코드로 이를 해결할 수 있을까? 고민하였지만 조금 무식한? 방법인 시간 지연으로 이를 해결하였습니다. 

​		

​		

## 1. time.sleep

- python 내장 library
- 지정한 시간만큼 지연 (프로세스 자체를 지정한 시간동안 기다림)

### 사용법

```javascript
from time import sleep

sleep(3) //3초간 기다림
```



​	

​		

## 2. Implicitly Wait (암묵적 대기)

- Selenium 메서드
- 크롬드라이버의 동작을 지연시킴
- 지정한 시간만큼 대기 (브라우저에서 사용되는 엔진 자체에서 파싱되는 시간을 기다림)

### 사용법

```javascript
from selenium import webdriver

driver = webdriver.Chrome('chromedriver.exe')
driver.implicitly_wait(10)  //10초간 기다림
```

​	

​	

​	

## 3. Explicitly Wait (명시적 대기)

- Selenium 메서드
- 조건이 True가 될 때 까지 지정한 시간만큼 대기
- 가장 멋진 방법 같지만, 사용 조건이 조금 까다로운 편.

### 사용법

```javascript
from selenium import webdriver
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC

browser=webdriver.Chrome('chromedriver')
browser.get("https://www.naver.com/")
WebDriverWait(driver, 100).until(EC.presence_of_element_located((By.CSS_SELECTOR, "div.header")))
//div.header가 나타날때 까지 최대 100초간 기다림.
```

​	

​	

​	

​	


