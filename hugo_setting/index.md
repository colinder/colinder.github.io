# Hugo_setting stroy (feat.LoveTt 제작자)


​		

# 블로그를 만들면서...

최초 기술블로그를 제작하면서 바닥부터 모든 것을 스스로 만들어 보기로 결심하고 제작을 시작하지만, 결코 쉽지 않았다. 개인적으로 꼭 원했던 기능이었던 tag기능과 categories를 구현하지 못하면서... 결국 기존에 있는 라이브러리들을 활용하기로 한다. 

​	

**가장 많은 사용자가 존재하는 jekyll로 시작한다. **

​	

## jekyll 기초 설정

> `Jekyll` : **Jekyll**은 Ruby **Gem**으로 제공되며 템플릿과 템플릿의 구성요소, 인라인 코드, 마크다운과 같은 동적인 구성요소를 정적인 웹페이지로 만들어주는 파싱 엔진

​	

### jekyll 서버 구동 방법

```ruby
# 공식 홈페이지 설명
~$ gem install bundler jekyll			# jekyll 구동을 위한 프로그램 설치
~$ jekyll new MYBLOG					# MYBLOG 라는 이름의 블로그 폴더 & 기초틀 생성 
~$ cd MYBLOG							# MYBLOG 폴더로 이동
~/MYBLOG $ bundle exec jekyll serve		# MYBLOG 서버 구동
```

```ruby
# 이후 명령창에
Servuer address : http://127.0.0.1:4000/
로 인터넷 접속하면 jekell의 디폴트 서버 모습이 보인다.
```

이게 다다. 

​	

**포스팅을 하면서 이 내용을 정리할 때쯤. 알 수 없는 치명적인 오류로 인해 작성했던 .md파일과 git 서버가 폭파했다.** 여러 방면으로 복구를 시도했지만, 모든 방법이 실패했고. 그리하여.. 난 jekyll을 떠나 Hugo로 이동하기로 했다.

---

​		

## {{< link "https://gohugo.io/" Hugo >}}

### Why Hugo?

> #### #1. window를 공식 지원한다. ( Jekyll은 window를 공식적으로 지원하진 않는다. )
>
> → Hugo가 Jekyll 보다 window환경에서 안정적이지 않을까?
>
> #### #2. The world’s fastest framework for building websites
>
> → 빠르다. 



Hugo로 블로그를 구축한다면 이를 git의 서버를 통해 배포한다. 

​	

## Hugo로 github.io 블로그 만들기

### 1. Git, Hugo 설치

- Git 설치란?   {{< link "https://gitforwindows.org/" Git_bash >}}설치를 뜻한다. 링크를 타고 들어가 적절한 버전을 선택해 설치를 진행한다. 

  ​	*👉 bash를 실행시키고 `git --version`을 입력해서 version 정보가 확인되면 정상 설치가 된 것이다.*

  ​	

- Hugo 설치 (공식홈페이지의 {{< link "https://gohugo.io/getting-started/quick-start/" Quick_Start >}} 참고)

  ​	*👉 난 {{< link "https://github.com/gohugoio/hugo/releases" Hugo_release>}}에서 최신버전을 다운받아 직접 설치했다.*

  1. `C:\Hugo\bin\`에 압축을 해제.
  2. `window + Q`로 검색창을 연 뒤 `변수`를 검색해서 `시스템 환경 변수 편집`에 들어간다.
  3. 아래의 `환경 변수(N)`를 클릭.
  4. 위쪽 박스에서 `Path`를 더블클릭한다.
  5. `새로 만들기`를 클릭 후, 아까 압축을 풀었던 곳인 `C:\Hugo\bin`를 등록.
  6. 닫고 배경화면에서 bash 실행 후 `hugo version`을 입력해서 version 정보가 나오면 잘 설치가 된 것.
  
  

### 2. Hugo 블로그 구축하기

프로젝트를 구축할 폴더(혹은 장소)에서 bash를 실행

```bash
hugo new site <프로젝트 이름>		# ex) hugo new site MyBlog → MyBlog라는 프로젝트 폴더를 생성.
```

{{< image src="/images/Hugo_01.png" caption="휴고 사이트 생성 명령어" width="600px" >}}

이때 생성된 폴더는 프로젝트폴더, root폴더, default폴더 등으로 불리며,

{{< image src="/images/Hugo_02.png" >}}

기본 구조는 이러하며, 먼저 테마를 설정해보자. 필자는 LoveIt 테마를 선택해 사용함.

​	*Theme는 `다운` →  `적용`의 2단계를 거친다.*

​	

​	**1. 다운**

먼저 LoveIt 테마 {{< link "https://github.com/dillonzq/LoveIt" Git홈페이지 >}}에 가 clone을 떠온다.

프로젝트폴더에서 bash를 실행 후 

```bash
cd theme		# theme 폴더로 이동
git clone https://github.com/dillonzq/LoveIt.git		# LoveIt 테마 클론(다운)
```

​	

​	**2. 적용**

프로젝트폴더속 `config.toml`파일을 열어 수정을 하여 적용하는데 적용 방법은 각 Theme의 git_site에 정리 되어 있다.  LoveIt theme의 경우 아래와 같은 방법으로 설정하며 추가 버전이 release되어 있어 이를 적용하는 방법은 공식 문서를 읽어보며 custom한다. 

```config.toml
baseURL = "http://example.org/"
# [en, zh-cn, fr, ...] determines default content language
defaultContentLanguage = "en"
# language code
languageCode = "en"

# Change the default theme to be use when building the site with Hugo
theme = "LoveIt"

...
...
```


