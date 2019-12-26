

# 20190917_Static File & Media File

## 지난 시간 이어서 (댓글)

### 댓글 갯수 표현하기

```html
  {{ comments.count }}
  <!-- {{ comments | length }} -->
  {% for comment in comments %}
    <p><b>{{ comment.content }}</b> | 작성 : {{ comment.created_at }} | 수정 : {{ comment.updated_at }}</p>
  {% endfor %}
  <hr>
```

또는

```python
def detail(request, pk):
    # pk라는 id를 가진 글을 찾아와 보여줌
    post = Post.objects.get(pk=pk)

    # 해당 글에 달려있는 모든 댓글을 보여줌
    comments= post.comment_set.all()
    context = {
        'post': post,
        'comments': comments,
        # "num_of_comments": comments.count,
    }
    return render(request, 'posts/detail.html', context)
```

```html
  {% if comments.count != 0 %}
    <p><i>{{ comments.count }}개의 댓글이 있습니다.</i></p>
    <!-- {{ comments | length }} -->
  {% for comment in comments %}
    <p><b>{{ comment.content }}</b> | 작성 : {{ comment.created_at }} | 수정 : {{ comment.updated_at }}</p>
  {% endfor %}
  {% else %}
    <p><i>아직 댓글이 없어요 ㅠㅠ</i></p>
  {% endif %}
```

또는

```html

  {% for comment in comments %}
    <p><b>{{ comment.content }}</b> | 작성 : {{ comment.created_at }} | 수정 : {{ comment.updated_at }}</p>
    {% empty %}
      <p>아직 댓글이 없어요 ㅠㅠ</p>
  {% endfor %}
```



### 게시글 ordering / Model

- 게시글은 최신글이 가장 위에 떠야 하므로 reversed 해줘야 한다.
  
- 물론 views.py에서 직접 reversed해줘도 되지만 이는 views.py의 코드량을 늘려 가독성을 떨어트린다.
  
- Meta data : 어떤 것을 설명해지는 상위의 개념

  - Meta Class를 만들어 ordering이라는 메소드(?)를 이용해 reversed option을 설정하면 된다.
  - Meta Class는 하위 클래스의 개념이라기보단 class의 name space를 만드는 것이라고 생각하는 것이 좋다.

  ```python
  class Post(models.Model):
      title = models.CharField(max_length=100)
      content = models.TextField()
      created_at = models.DateTimeField(auto_now_add=True)
      updated_at = models.DateTimeField(auto_now=True)
  
      class Meta:
          ordering = ['-pk']
  ```

  - Meta Class는 DB에 영향을 주는 게 아니라 ORM을 건드리는 것이기 때문에 따로 migration을 할 필요가 없다.



### 이미지 업로드 기능 추가

```bash
pip install pillow
```

- pip로 깔면 꼭 settings.py INSTALLED_APPS에 추가해줘야 한다. > Pillow는 바로 안해줘도 되긴 함

- models.py에 새 column 추가

```python
class Post(models.Model):
    title = models.CharField(max_length=100)
    content = models.TextField()
    image = model.ImageField(blank=True)
    # blank를 True로 하면 nullable
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    class Meta:
        ordering = ['-pk']
```

- 새 칼럼 추가를 해야하는데...

  예전에 써뒀던 글들은 어떻게 해야 하는가?

  => 칼럼 추가시 Null값을 넣을지 아닐지 물어본다. 

  => 하지만 이는 매우 위험하고 짜증나는 작업이기 때문에 중요 정보가 있지 않는 한 DB를 날리고 새로 하는 것이 좋다.

  - 먼저 server를 꺼야 한다. (그래야 cache가 남지 않음)

  - migrations의 파일 삭제(init은 삭제하면 안돼!) 및 db.sqlite3 삭제



- new.html 수정

```html
{% extends 'base.html' %}

{% block body %}
<div class="container">
  <h1>새글쓰기</h1>

  <form action="{% url 'posts:create' %}" method="POST" enctype="multipart/form-data"> <!-- enctype을 추가해주고 -->
    <div class="form-group">
      {% csrf_token %}
      <label for="title">제목</label>
      <input type="text" class="form-control" id="title" name="title" placeholder="제목을 입력해 주세요.">
      <label for="content">내용</label>
      <textarea id="content" name="content" class="form-control" rows="10"></textarea>
      <input type="file" name="image" accept="image/*"> <!-- type이 file인 input 태그를 추가(accept는 filtering만 해줄 뿐 다른 확장자의 파일 업로드를 막지는 못함 / 이건 db단에서 control) -->
    </div>
    <button type="submit" class="btn btn-primary">글쓰기</button>
  </form>
    
</div>

{% endblock %}
```

- views.py의 create 메소드 수정

```python
def create(request):
    if request.method == 'POST':
        Post.objects.create(
            title = request.POST.get('title'), 
            content = request.POST.get('content'),
            image = request.FILES.get('image'), # FILES로 받아오면 된다.
        )
        return redirect('home')
    else:
        return render(request, 'posts/new.html')
```

- 여기까지 하면, 파일이 root folder에 올라가는 것을 볼 수 있다. 따라서, 
  - 저장되는 위치를 설정해줘야 한다.
  - 경로 전체를 load하여 DB에 저장하여 django앱이 알아서 그 주소를 찾아 주게 해야한다.
- 이를 수정하기 위해 일단, settings.py 수정 :

```python
# 1. MEDIA_ROOT 추가
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
# 앞으로 파일이 저장될 폴더를 지정

# 2. MEDIA_URL 추가
MEDIA_URL = '/media/' # 이것 추가
# 업로드된 파일의 주소(URL을 만들어줌) default : ''(빈 string)
```

- 메인 urls.py에 /media/에서 들어오는 파일 요청을 받을 수 있게 static이라는 메소드 이용

```python
from django.contrib import admin
from django.urls import path, include
from posts import views
from django.conf.urls.static import static # static을 쓰기 위해 import 먼저 하고
from django.conf import settings # 상대 경로를 지정하기 위해 import

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', views.index, name='home'),
    path('posts/', include('posts.urls')),
] += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
	# file serving을 위해 추가
    # settings.MEDIA_URL은 /media/를 가르키기 위함
    # document_root라는 키워드 인자에 settings.MEDIA_ROOT를 넣어 넘겨주기
    # list에 append 하듯이 써야 한다.
    	## urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)이라고 따로 써도 좋다.
```

- Pillow가 좋은 이유 : 같은 이름의 다른 file이 들어와도 알아서 random 값을 파일명에 추가해서 저장한다.

- detail.html 수정(DB에 저장된 image 주소 이용하여 사진 올리기)

```python
<img src="{{ post.image.url }}" alt="{{ post.image }}">
```



## Static

- favicon : 대표적인 static item 16*16짜리 이미지
  [https://www.favicon-generator.org](https://www.favicon-generator.org/) favicon 생성 사이트

- 파비콘 zip 파일 다운 후 media에 넣고

  base.html의 head 부분에 해당 링크를 복사해서 붙여넣는다.

```html
    <link rel="apple-touch-icon" sizes="57x57" href="/apple-icon-57x57.png">
    <link rel="apple-touch-icon" sizes="60x60" href="/apple-icon-60x60.png">
    <link rel="apple-touch-icon" sizes="72x72" href="/apple-icon-72x72.png">
    <link rel="apple-touch-icon" sizes="76x76" href="/apple-icon-76x76.png">
    <link rel="apple-touch-icon" sizes="114x114" href="/apple-icon-114x114.png">
    <link rel="apple-touch-icon" sizes="120x120" href="/apple-icon-120x120.png">
    <link rel="apple-touch-icon" sizes="144x144" href="/apple-icon-144x144.png">
    <link rel="apple-touch-icon" sizes="152x152" href="/apple-icon-152x152.png">
    <link rel="apple-touch-icon" sizes="180x180" href="/apple-icon-180x180.png">
    <link rel="icon" type="image/png" sizes="192x192"  href="/android-icon-192x192.png">
    <link rel="icon" type="image/png" sizes="32x32" href="/favicon-32x32.png">
    <link rel="icon" type="image/png" sizes="96x96" href="/favicon-96x96.png">
    <link rel="icon" type="image/png" sizes="16x16" href="/favicon-16x16.png">
    <link rel="manifest" href="/manifest.json">
    <meta name="msapplication-TileColor" content="#ffffff">
    <meta name="msapplication-TileImage" content="/ms-icon-144x144.png">
```

- 하지만 경로가 문제가 된다.

- 따라서 app 폴더의 하위로 static이라는 폴더를 만들고

  base.html 맨 위에 {% load static %}을 적어준다. (django-static files 문서 참고)

### Static 폴더  관리

- static file의 기준 : user가 조작할 일이 없는 파일
- assets : static 관련 folder들을 모두 저장해둘 폴더(메인 app 폴더 하위에 위치)

```python
STATICFILES_DIRS = [os.path.join(BASE_DIR, 'board', 'assets')]
# assets : static 폴더의 묶음이라 봐도 무방(global함)
```

```html
    <!-- Favicon-->
    <link rel="icon" type="image/png" sizes="16x16" href="{% static 'images/favicon-16x16.png' %}">
```

- media file : user가 올리는 파일



## 기타

- 알고리즘 위주의 공부보다는 활용 위주의 공부를 할 것!
  - 현업에 가면 알고리즘은 거의 쓰지 않는다. 그냥 로직만 공부하는 것일 뿐
  - 따라서 충분히 프레임워크를 활용하는 연습을 한다.
- Django는 file structure에 대해 관여하지 않는다. file structure는 따로 관리가 되고 이를 matching하는 알고리즘이 존재할 뿐
- Docker : 개발자로서 쓸 줄 안다고 하면 매우 우대 받을 것임
- either.io 컨셉의 팀 프로젝트 진행할 예정
- static files 관련 document https://docs.djangoproject.com/en/2.2/howto/static-files/
- django 페이지 중 번역이 되어 있지 않은 곳은 django github에 들어가서 django-docs-traslations repo에 contribute 할 기회이다! (open source contributor로서 활약할 수 있다!!)
  - forking을 하고 open source contribute 목록을 쭉 정리해보기!