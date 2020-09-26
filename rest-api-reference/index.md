# REST


​	

# REST API Reference

[TOC]



## 1.사용자 API

| Members                     | Descriptions     |
| --------------------------- | ---------------- |
| POST /accounts/signup       | 회원가입         |
| POST /accounts/login        | 회원 로그인      |
| POST /accounts/userDetail   | 회원정보         |
| PUT /accounts/update        | 회원정보 수정    |
| DELETE /accounts/dropUser   | 회원탈퇴         |
| GET /accounts/emailAuth     | 이메일 인증      |
| GET /accounts/emailCheck    | 이메일 중복 확인 |
| GET /accounts/nicknameCheck | 닉네임 중복 확인 |



### 1.1 회원 로그인 POST /accounts/login

​	회원 로그인 API 입니다.

​	`Request parameters`

| Parameter | Type   | Description |
| --------- | ------ | ----------- |
| email     | String | 이메일      |
| password  | String | 비밀번호    |

​	`Response (Success)`

| Field           | Type          | Description        |
| --------------- | ------------- | ------------------ |
| uid             | String        | DB에서 관리하는 id |
| password        | String        | 회원 비밀번호      |
| passwordConfirm | String        | 회원 비밀번호 확인 |
| email           | String        | 이메일             |
| nickname        | String        | 닉네임             |
| content         | String        | 자기소개           |
| createDate      | LocalDateTime | 회원가입일         |
| likedpost       | String        | 좋아요한 글 목록   |

​	`Response (Fail)`

| Field | Type | Description |
| ----- | ---- | ----------- |
| data  |      | null        |





### 1.2 회원가입 POST /accounts/signup

​	회원가입 API 입니다.

​	`Request parameters`

| Field           | Type   | Description        |
| --------------- | ------ | ------------------ |
| password        | String | 회원 비밀번호      |
| passwordConfirm | String | 회원 비밀번호 확인 |
| email           | String | 이메일             |
| nickname        | String | 닉네임             |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | success     |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





#### 		1.2.1 이메일 중복확인 GET /accounts/emailCheck

회원가입시 이메일 중복확인하는 API 입니다.
​	`Request parameters`

| Field | Type   | Description |
| ----- | ------ | ----------- |
| email | String | 이메일      |

​	`Response(Success)`

| Field       | Type   | Description    |
| ----------- | ------ | -------------- |
| result.data | String | 아이디사용가능 |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 아이디중복  |

#### 		1.2.2닉네임 중복확인 GET /accounts/nicknameCheck

회원가입시 닉네임중복확인하는 API 입니다.
​	`Request parameters`

| Field    | Type   | Description |
| -------- | ------ | ----------- |
| nickname | String | 닉네임      |

​	`Response(Success)`

| Field       | Type   | Description    |
| ----------- | ------ | -------------- |
| result.data | String | 닉네임사용가능 |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 닉네임중복  |

#### 		1.2.3이메일 인증 GET /accounts/emailAuth

이메일 중복 확인시 이메일을 인증메일을 보내는 API 입니다.
​	`Request parameters`

| Field | Type   | Description |
| ----- | ------ | ----------- |
| email | String | 이메일      |

​	`Response(Success)`

| Field | Type | Description      |
| ----- | ---- | ---------------- |
| dice  | int  | 이메일 인증 코드 |

​	`Response (Fail)`

| Field | Type | Description |
| ----- | ---- | ----------- |
| data  |      | null        |





### 	1.3 회원 정보 수정 PUT /accounts/update 

회원정보 수정 API 입니다.

​	`Request parameters`

| Field    | Type   | Description        |
| -------- | ------ | ------------------ |
| uid      | String | DB에서 관리하는 id |
| password | String | 회원 비밀번호      |
| nickname | String | 닉네임             |
| content  | String | 자기소개           |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | success     |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 	1.4 회원 정보 POST /accounts/userDetail

회원정보를 조회하는 API 입니다.

​	`Request parameters`

| Field | Type   | Description        |
| ----- | ------ | ------------------ |
| uid   | String | DB에서 관리하는 id |


​	`Response (Success)`

| Field           | Type          | Description        |
| --------------- | ------------- | ------------------ |
| uid             | String        | DB에서 관리하는 id |
| password        | String        | 회원 비밀번호      |
| passwordConfirm | String        | 회원 비밀번호 확인 |
| email           | String        | 이메일             |
| nickname        | String        | 닉네임             |
| content         | String        | 자기소개           |
| createDate      | LocalDateTime | 회원가입일         |
| likedpost       | String        | 좋아요한 글 목록   |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 	1.5 회원탈퇴 DELETE /accounts/dropUser

회원 탈퇴 API 입니다.

​	`Request parameters`

| Field | Type   | Description        |
| ----- | ------ | ------------------ |
| uid   | String | DB에서 관리하는 id |


​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | success     |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





## 2.게시물 API

| Members                         | Descriptions              |
| ------------------------------- | ------------------------- |
| POST /articles/register         | 글 작성                   |
| GET /articles/showArticle       | 글 상세 조회              |
| PUT /articles/modify            | 글 수정                   |
| DELETE /articles/dropArticle    | 글 삭제                   |
| POST /articles/like             | 글 좋아요                 |
| POST /articles/likedList        | 좋아요 게시물 리스트      |
| GET /articles/searchArticle     | 글 검색                   |
| POST /articles/getRecommentList | 음식점 추천 리스트        |
| GET/articles/list               | 전체 글 리스트            |
| POST /articles/postedList       | 사용자가 작성한 글 리스트 |
| GET/articles/postedListByLikes  | 좋아요순 글 리스트        |
| GET/articles/postedListByHits   | 조회순  글 리스트         |




### 	2.1 글 작성 POST /articles/registe

글 작성 API 입니다.

​	`Request parameters`

| Field     | Type   | Description        |
| --------- | ------ | ------------------ |
| userid    | int    | 글 작성자의 id     |
| nickname  | String | 글 작성자의 닉네임 |
| title     | String | 글 제목            |
| content   | String | 글 내용            |
| hashtag   | String | 글 해시태그        |
| address   | String | 주소               |
| placename | String | 장소 이름          |
| url       | String | 상세보기 daum url  |
| lat       | String | 위도               |
| lon       | String | 경도               |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 글작성 성공 |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 글작성 실패 |





### 	2.2 글 상세 조회 GET /articles/showArticle

글 상세조회하는 API입니다.

​	`Request parameters`

| Field  | Type | Description             |
| ------ | ---- | ----------------------- |
| postId | int  | DB에서 관리하는 post id |


​	`Response(Success)`

| Field      | Type   | Description             |
| ---------- | ------ | ----------------------- |
| postId     | int    | DB에서 관리하는 post id |
| userid     | int    | 글 작성자의 id          |
| title      | String | 글 제목                 |
| lat        | String | 위도                    |
| lon        | String | 경도                    |
| content    | String | 글 내용                 |
| hashtag    | String | 글 해시태그             |
| address    | String | 주소                    |
| likes      | int    | 글 좋아요수             |
| createDate | String | 글 작성일               |
| nickname   | String | 글 작성자의 닉네임      |
| hits       | int    | 조회수                  |
| url        | String | 상세보기 url            |
| starpoint  | String | 크롤링 별점             |
| placename  | String | 장소 이름               |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 조회 실패   |





### 	2.3 글 수정 PUT /articles/modify

글 수정하는 API 입니다.

​	`Request parameters`

| Field     | Type   | Description        |
| --------- | ------ | ------------------ |
| userid    | int    | 글 작성자의 id     |
| nickname  | String | 글 작성자의 닉네임 |
| title     | String | 글 제목            |
| content   | String | 글 내용            |
| hashtag   | String | 글 해시태그        |
| address   | String | 주소               |
| placename | String | 장소 이름          |
| url       | String | 상세보기 daum url  |
| lat       | String | 위도               |
| lon       | String | 경도               |

​	`Response(Success)`

| Field      | Type   | Description             |
| ---------- | ------ | ----------------------- |
| postId     | int    | DB에서 관리하는 post id |
| userid     | int    | 글 작성자의 id          |
| title      | String | 글 제목                 |
| lat        | String | 위도                    |
| lon        | String | 경도                    |
| content    | String | 글 내용                 |
| hashtag    | String | 글 해시태그             |
| address    | String | 주소                    |
| likes      | int    | 글 좋아요수             |
| createDate | String | 글 작성일               |
| nickname   | String | 글 작성자의 닉네임      |
| hits       | int    | 조회수                  |
| url        | String | 상세보기 url            |
| starpoint  | String | 크롤링 별점             |
| placename  | String | 장소 이름               |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 	2.4 글 삭제 DELETE /articles/dropArticle

글 삭제하는 API입니다.

​	`Request parameters`

| Field  | Type | Description             |
| ------ | ---- | ----------------------- |
| postId | int  | DB에서 관리하는 post id |


​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 삭제 성공   |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 삭제 실패   |





### 	2.5 글 좋아요 POST /articles/like

글 좋아요 올리거나 내리는 API 입니다.

​	`Request parameters`

| Field  | Type | Description             |
| ------ | ---- | ----------------------- |
| postId | int  | DB에서 관리하는 post id |
| userid | int  | 글 작성자의 id          |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | success     |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 	2.6 좋아요 게시물 리스트 POST /articles/likedList

좋아요 누른 게시물을 리턴해주는 API입니다.

​	`Request parameters`

| Field  | Type | Description    |
| ------ | ---- | -------------- |
| userid | int  | 글 작성자의 id |


​	`Response(Success)`

| Field       | Type       | Description       |
| ----------- | ---------- | ----------------- |
| result.data | List<Post> | Post객체의 리스트 |

​	`Response (Fail)`

| Field       | Type | Description |
| ----------- | ---- | ----------- |
| result.data |      | null        |





### 	2.7 글 검색 GET /articles/searchArticle

글 검색 API 입니다.

​	`Request parameters`

| Field         | Type   | Description |
| ------------- | ------ | ----------- |
| keyword(path) | String | 검색어      |


​	`Response(Success)`

| Field       | Type          | Description                |
| ----------- | ------------- | -------------------------- |
| result.data | HashMap<Post> | 해쉬맵으로 검색결과를 리턴 |

​	`Response (Fail)`

| Field       | Type | Description |
| ----------- | ---- | ----------- |
| result.data |      | null        |





### 	2.8 음식점 추천 리스트 POST /articles/getRecommentList

음식점 추천 API입니다.

​	`Request parameters`

| Field   | Type   | Description                     |
| ------- | ------ | ------------------------------- |
| food    | String | 관련 태그를 ,로 연결하여 보내기 |
| isCafe  | String | 카페도 추천하면 추가            |
| isDrink | String | 술집도 추천하면 추가            |
| like    | String | 피드백에서 좋아요수가 부족할때  |
| watch   | String | 피드백에서 조회수가 부족할때    |
| star    | String | 피드백에서 별점이 부족할때      |




​	`Response(Success)`

| Field | Type | Description     |
| ----- | ---- | --------------- |
| 음식  | Post | 추천하는 음식점 |
| 카페  | Post | 추천하는 카페   |
| 술집  | Post | 추천하느 술집   |





### 	2.9 전체 글 리스트 GET /articles/list

전체 글을 받는 API입니다. 

​	`Response(Success)`

| Field          | Type          | Description      |
| -------------- | ------------- | ---------------- |
| result.comment | List<Integer> | 댓글수의 리스트  |
| result.list    | List<Post>    | 전체 글의 리스트 |





### 	2.10 사용자가 작성한 글 리스트 POST /articles/postedList

사용자가 작성한 글의 리스트를 리턴해주는 API 입니다.

​	`Request parameters`

| Field  | Type | Description    |
| ------ | ---- | -------------- |
| userid | int  | 글 작성자의 id |


​	`Response(Success)`

| Field  | Type       | Description      |
| ------ | ---------- | ---------------- |
| result | List<Post> | 전체 글의 리스트 |

​	`Response (Fail)`

| Field  | Type | Description |
| ------ | ---- | ----------- |
| result |      | null        |





### 	2.11좋아요순 글 리스트 GET /articles/postedListByLikes

좋아요 순으로 글의 리스트를 리턴해주는 API 입니다.


​	`Response(Success)`

| Field          | Type          | Description      |
| -------------- | ------------- | ---------------- |
| result.comment | List<Integer> | 댓글수의 리스트  |
| result.list    | List<Post>    | 전체 글의 리스트 |





### 	2.12 조회순 글 리스트 GET /articles/postedListByHits

조회 순으로 글의 리스트를 리턴해주는 API 입니다.


​	`Response(Success)`

| Field          | Type          | Description      |
| -------------- | ------------- | ---------------- |
| result.comment | List<Integer> | 댓글수의 리스트  |
| result.list    | List<Post>    | 전체 글의 리스트 |





### 	2.13 좋아요순 글 리스트 GET /articles/postedListByStarpoint

별점순으로 글의 리스트를 리턴해주는 API 입니다.


​	`Response(Success)`

| Field          | Type          | Description      |
| -------------- | ------------- | ---------------- |
| result.comment | List<Integer> | 댓글수의 리스트  |
| result.list    | List<Post>    | 전체 글의 리스트 |







## 3.글 임시저장  API

| Members                            | Descriptions          |
| ---------------------------------- | --------------------- |
| POST /subarticles/register         | 임시 글 작성          |
| GET /subarticles/detail/{postid}   | 임시저장 글 상세 조회 |
| GET /subarticles/list/{userid}     | 유저별 임시 글 리스트 |
| DELETE /subarticles/dropSubarticle | 임시 글 삭제          |





### 3.1 임시 글 작성 POST /subarticles/register

임시 글 작성 API입니다.

​	`Request parameters`

| Field     | Type   | Description        |
| --------- | ------ | ------------------ |
| userid    | int    | 글 작성자의 id     |
| nickname  | String | 글 작성자의 닉네임 |
| title     | String | 글 제목            |
| content   | String | 글 내용            |
| hashtag   | String | 글 해시태그        |
| address   | String | 주소               |
| placename | String | 장소 이름          |
| url       | String | 상세보기 daum url  |
| lat       | String | 위도               |
| lon       | String | 경도               |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 글작성 성공 |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | 글작성 실패 |





### 3.2 임시저장 글 상세 조회 GET /subarticles/detail/{postid}

임시 글 상세조회  API입니다.

​	`Request parameters`

| Field  | Type | Description             |
| ------ | ---- | ----------------------- |
| postid | int  | DB에서 관리하는 post id |

​	`Response(Success)`

| Field  | Type | Description         |
| ------ | ---- | ------------------- |
| result | Post | 유저별 임시저장  글 |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 3.3 유저별 임시 글 리스트 GET /subarticles/list/{userid}

유저별 임시 글 리스트  API입니다.

​	`Request parameters`

| Field  | Type | Description             |
| ------ | ---- | ----------------------- |
| userid | int  | DB에서 관리하는 user id |

​	`Response(Success)`

| Field  | Type       | Description                  |
| ------ | ---------- | ---------------------------- |
| result | List<Post> | 유저별 임시저장  글의 리스트 |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 3.4 임시 글 삭제 DELETE /subarticles/dropSubarticle

임시 글 삭제  API입니다.

글 삭제하는 API입니다.

​	`Request parameters`

| Field  | Type | Description             |
| ------ | ---- | ----------------------- |
| postId | int  | DB에서 관리하는 post id |

​	`Response(Success)`

| Field       | Type   | Description           |
| ----------- | ------ | --------------------- |
| result.data | String | 임시저장 글 삭제 성공 |

​	`Response (Fail)`

| Field       | Type   | Description           |
| ----------- | ------ | --------------------- |
| result.data | String | 임시저장 글 삭제 실패 |





## 4.댓글 API

| Members                      | Descriptions              |
| ---------------------------- | ------------------------- |
| GET /comments/list/{postid}  | 해당글의 댓글 전체 리스트 |
| POST /comments/register      | 댓글 작성                 |
| PUT /comments/modify         | 댓글 수정                 |
| DELETE /comments/dropComment | 댓글 삭제                 |





### 4.1 해당 글의 전체 댓글 GET /comments/list/{postid}

해당 글의 전체 댓글을 받아오는 API입니다.

​	`Request parameters`

| Field        | Type | Description |
| ------------ | ---- | ----------- |
| postid(path) | int  | 해당 글 id  |

​	`Response(Success)`

| Field       | Type          | Description          |
| ----------- | ------------- | -------------------- |
| result.data | List<Comment> | Comment객체의 리스트 |





### 4.2 댓글 작성 POST /comments/register

댓글 작성하는 API 입니다.
​	`Request parameters`

| Field    | Type   | Description                  |
| -------- | ------ | ---------------------------- |
| postid   | int    | 댓글을 쓰는 글의 id          |
| content  | String | 댓글 내용                    |
| userid   | int    | 글 작성하 사용자의 DB userid |
| nickname | String | 글 작성하는 사용자의 닉네임  |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | success     |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 4.3 댓글 수정 PUT /comments/modify

댓글 수정하는 API 입니다.


​	`Request parameters`

| Field    | Type   | Description                  |
| -------- | ------ | ---------------------------- |
| postid   | int    | 댓글을 쓰는 글의 id          |
| content  | String | 댓글 내용                    |
| userid   | int    | 글 작성하 사용자의 DB userid |
| nickname | String | 글 작성하는 사용자의 닉네임  |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | success     |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |





### 4.4 댓글 삭제 DELETE /comments/dropComment

댓글 삭제하는 API입니다.

​	`Request parameters`

| Field     | Type | Description |
| --------- | ---- | ----------- |
| commentId | int  | 댓글의 id   |

​	`Response(Success)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | success     |

​	`Response (Fail)`

| Field       | Type   | Description |
| ----------- | ------ | ----------- |
| result.data | String | fail        |
