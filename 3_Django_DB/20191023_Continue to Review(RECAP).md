# 20191023_Continue to Review(RECAP)

- SQL에서 조건을 걸 때 WHERE를 쓴다면, ORM에서는 get 또는 filter를 쓴다.
  - get : 하나 뽑을 때, 없으면 error가 뜬다.
  - filter : 여러 개 뽑을 때, 없으면 빈 query set이 나온다.
- 좋아요 목록 : with를 이용하여 article.like_users.all()을 한 변수에 저장(Caching)

```html
<!-- 생략 -->
  {% with likers=article.like_users.all %}
  <p>좋아요 목록 : 
    {%for liker in likers %}
      {{ liker }} 
    {% endfor %}
  </p>
  {% endwith %}
<!-- 생략 -->
```



## Profile 페이지 만들기



## Follow 기능 만들기

- model.py(accounts)

```python
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.conf import settings

# Create your models here.
class User(AbstractUser):
    followers = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name="followings") # related_name = 역참조 값
```



## Hashtag 기능 만들기

- Many-to-many



## 기타

- 1:N 관계에서 Foreign Key는 N이 될 model에다가 설정해준다.

  ex. User-Articles 관계에서, Article이 User의 키를 참조해야 하므로 Article 모델에 User의 id값을 Foreign Key로 사용한다.

- 기술 면접 

  - ORM의 장점은?
  - lazy loading이 뭔지?  http://raccoonyy.github.io/using-django-querysets-effectively-translate/ 참고
  - with 같은 경우는 사실 ORM에 기댄다면 안 써도 되지만, 구현 위주로 수업 진행을 위해 쓴 것!

- 알고리즘 책 추천 : CLRS