# 20191024_API Server

### 그라바타

- ko.gravatar.com

## 개요

- **SPA**(Single Page Application)
- MERN
- NoSQL : 모바일 시대가 오면서 데이터 저장량이 너무 많아지다 보니 관계형 DB로는 감당할 수 없게 됨
- REACT, Vue(우리가 곧 배울 것)
- 후에 할 프로젝트에서 Django는 서버를 만드는 정도
- 오늘은 API server / RESTful Server 만들기

## API Server 만들기

- 프로젝트 만든 후 rest framework 환경 설정

```git bash
pip install djangorestframework
```

-  https://www.django-rest-framework.org/  참고
- settings.py>INSTALLED_APPS에 "rest_framework" 추가

- 모델에 app을 추가할 때 musics.apps.MusicsConfig라고 할 수 있다. (레거시 코드)

### dummy data

- dummy data 추가 : 프로젝트 단에 fixtures/[프로젝트 이름] 폴더를 만들어 준다 >  json 파일을 저장 한다 > 다음 코드 작성

```git bash
$ python manage.py loaddata musics/dummy.json
```

- 기존 db의 data로 dummy를 만들고 싶으면 다음 코드를 쓴다.

```git bash
python manage.py dumpdata articles > dummy.json --indent 2
```

- dumping /  데이터의 직렬화 (serializing) <-> loading,

### API 만들기

- API도 RESTful하게 짜진 경우는 versioning이 들어간 경우가 많다.

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/v1/', include('musics.urls')), # versioning을 위해 version을 명시
]
```

```
from django.urls import path

app_name = 'musics'

urlpatterns = [
    path('musics/', views.music_list, name='music_list'),
]
```

- music app에 serializer.py 생성(dumping하기 위함)

```python
from rest_framework import serializers
from .models import Music

serializers.ModelSerializer
# forms.ModelForm과 비슷

class MusicSerializer(serializers.ModelSerializer):
    class Meta:
        model = Music
        fields = ('pk', 'title', 'artist_id', )
```

```python
from django.shortcuts import render
from .models import Music
from .serializers import MusicSerializer
from rest_framework.response import Response # json object를 관장한다.
from rest_framework.decorators import api_view

# Create your views here.
@api_view(['GET']) # CRUD 각각에 해당하는 method들을 쓸 수 있게 함
def music_list(request):
    musics = Music.objects.all()
    # return render() -> .html 페이지를 응답으로 보내주기
    # 우리는 현재 render를 하는 것이 아님
    serializer = MusicSerializer(musics, many=True) # parameter로 query set을 넣어주면 알아서 직렬화함
    # 동일한 유형의 data가 여러개 있다는 의미로 many를 true로
    return Response(serializer.data) # music 안의 data를 보내줄 것임

```

-  http://localhost:8000/api/v1/musics/?format=json 이라고 format을 지정해주면 json 파일로 나타남

### 용도에 따른 API

- program을 통해 용도에 맞게 사용자에게 api 보내는 것
-  https://github.com/axnsan12/drf-yasg 참고

```
pip install drf-yasg
```

```python
from django.urls import path
from . import views
from drf_yasg import openapi # 추가
from drf_yasg import get_schema_view # 추가

schema_view = get_schema_view(
    openapi.Info(
        title='Music API',
        default_version='v1',
    )
) # 추가

app_name = 'musics'

urlpatterns = [
    path('musics/', views.music_list, name='music_list'),
    path('musics/<int:music_pk>/', views.music_detail, name='music_detail'),
    path('docs/', schema_view.with_ui('redoc'), name="api_docs"), # 추가
    path('swagger/', schema_view.with_ui('swagger', name='api_swagger')), # 추가
]
```

- swagger는 원래 자바 기반이었지만, 유용하여 django에서도 쓸 수 있음



### RESTful API 의 Create

```python
class CommentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Comment
        fields = '__all__'
```

```python
@api_view(['POST'])
def comment_create(request, music_pk):
    serializer = CommentSerializer(data=request.data)
    if serializer.is_valid(raise_exception=True):
        serializer.save(msic_id=music_pk)
    return Response(serializer.data)
```





## 기타

- 레거시 코드

- admin에서 models 목록을 보고 싶으면 admin.py에 model을 import하고 admin.site.register([model name])을 작성하면 된다.

- **REST API URL 구조**(권장)

  Music : Comment = 1:N 일 때

  - Trailing slash가 없는 것이 REST의 정석

```
# music REST API
C 			POST		/musics
R(list)  	 GET		 /musics
R(detail) 	 GET 		 /muics/:pk
U 			PUT 		/musics/:pk
D 			DELETE 		/musics/:pk
# comment REST API < 매번 music_pk는 꼭 필요하다.
C 			POST		/musics/:pk/comments
R(list)  	 GET		 /musics/:pk/comments
R(detail) 	 GET 		 /muics/:pk/comments/:pk
U 			PUT 		/musics/:pk/comments/:pk
D 			DELETE 		/musics/:pk/comments/:pk
```

- payload / graphql
- facebook / Github 같은 경우는 GraphQL을 이용하여 동일 URL에 요청된 REST API를 보낸다. (Data 양이 방대해지면서)