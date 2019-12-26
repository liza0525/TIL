

# 20190916_DB

## 지난 시간 리뷰

### todo app 구성 후 telegram으로 알람 보내기

### 오늘의 명세

- Project : BOARD 
- App : posts

- Model Post
  - title
  - content
  - created_at & update_at은 옵션이 아니라 필수, 명세에 명시되지 않아도 꼭 만들 것
- url
  - /create
  - /detail/<num>
  - /index(또는 /list : 그러나 python의 list 메소드와 이름이 겹쳐 지양을 추천)
  - /update
  - /delete

### 해도해도 자꾸 잊는 것(그러므로, 암기할 것)

- templates 경로 설정 : setting.py의 TEMPLATES>DIRS에 os.join.path(BASE_DIR, 앱이름, templates)를 적는다
- templates render하는 순서는 urls 등록, views 메소드 생성, html 생성 순으로 한다(순서가 크게 중요하진 않으나 자꾸 까먹으니까 ㅠ)



- 

## DB Relation 

### 테이블 간의 관계

- 이 table들은 관계가 있다(대부분의 테이블들은 서로 관계가 있다)

```markdown
**table의 관계(4가지)**

- **서로 관계 없음**(생각보다 찾기 어려움)
- **1: 1 관계**(하나의 특정 record와 다른 테이블의 하나의 record가 관계 있을 때)(ex. 결혼)
  - 어느 record를 찾으면 그와 연결된 다른 record를 찾을 수 있다.
- **1:N 관계**(one to many 또는 many to one) (거의 모든 것! ex. 반-학생)
  - 우리가 만드려는 posts와 comment 테이블은 이 관계에 속한다고 보면 된다.
- **N:M 관계**(many to many) : 1:N관계를 알면 M:N은 표현할 수 있다.
```



### 1:N 관계

- 왜 table을 관계 맺어주는 것인가? 중복된 자료를 서로 분리하여 낭비를 줄일 수 있기 때문에
  - ex. 반 학생 테이블에 반 정보 테이블의 id를 저장할 수 있는 column을 추가해주면 각 테이블에서 데이터를 훨씬 편하게 관리할 수 있다!
- **1 has many** or **Many belongs to 1**
  - ex. 반이 1, 학생이 Many
  - 따라서 어떤 테이블이 포함되어 있고, 어떤 테이블이 포함하는지 관계를 알아봐야 한다.
  - 그걸 따진 후 N의 table에 attribute를 추가(명찰을 달아주는 것과 같다고 생각하면 쉽당)
- 보통 1:N 관계는 id 값을 이용하여 연결시킨다. (Primary Key-Foreign Key)



### 댓글 붙이기

- detail.html을 수정한다.

- 댓글은 어느 글의 달려있는 글이라는 것을 염두 -> 어떻게 DB를 구성해야 할지 고민해보자

- models.py에 Comment 클래스를 적는다. => posts, commets 두개의 테이블이 있다고 생각한다.

  - 여기에선 comment가 many의 입장이므로 여기에 posts의 key값(Foreign Key)을 받아오는 column을 추가한다.

  ```python
  class Comment(models.Model):
      content = models.CharField(max_length=300)
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
      post = models.ForeignKey(Post, on_delete=models.CASCADE) 
      # 실제로는 DB에 post라는 이름으로 만들지 않을 것이다.
      # ForeignKey의 첫번째 인자는 연결할 table(class)을 쓴다.
      # on_delete : 소속된 table의 값이 없어졌을 때, 어떻게 할 것인지
      #			보통은 소속된 table의 값이 없어지면, 
      #			연관된 값을 다 없애는 게 관례다. 따라서 CASCADE를 적는다.
  ```

  ```python
  models.OneToOne() # 1:1 관계
  models.ManyToMany() # N:M 관계
  ```



###  python manage.py shell 이용

- pip install django-extensions
- settings.py > INSTALLED_APPS 에 'django_extensions' 추가



## 기타

- 후에 fake Instagram을 만들 예정 : 페이스북이나 카카오, 구글 로그인 붙여볼 것이고, 인스타그램을 온전히 따라해볼 것임