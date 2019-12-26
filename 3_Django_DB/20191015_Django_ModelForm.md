# 20191015_Django_ModelForm

### ipython

- 장고랑 상관없는 python의 interaction

```bash
pip install ipython
```

- views.py에 `from IPython import embed` 추가

- `embed()` : 이 코드가 있는 곳에서 잠시 멈춤*(freeze!)*

- runserver 돌린 후 validation debugging하면 shell plus가 알아서 켜짐

- shell plus에서 실습할 것

  - `form.is_valid()` : 방금 날려준 data가 유효한지 확인

  - `form.cleaned_data` 

    - 유효성 검사를 통과한 깔끔한 data의 dictionary를 확인할 수 있음
    - validation에서 실패해도 넘어온 data는 cleaned_data에 저장이 됨

  - `form.as_p()`

    `form.as_table()`

    `form.as_ul()`

## Form 수정

```python
from django import forms

class ArticleForm(forms.Form):
    # title = forms.CharField(max_length=50)
    # content = forms.CharField(widget=forms.Textarea)

    title = forms.CharField(
        max_length=20,
        label='제목',
        help_text='제목은 20자 이내로 써주세요.', # 제약을 넣는 경우가 많음
        widget=forms.TextInput(
            attrs={
                'class': 'form-control my-content',
                'placeholder': '제목을 입력해주세요.',
            }
        )
    )
    
    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={
                'class': 'form-control my-content',
                'placeholder': '내용을 입력해주세요.',
                'rows': 5,
                # cols는 grid가 우선순위에 있어 지정하지 않는 것이 좋다.
            }
        )
    )
```



### error handling

- development error page는 배포하면 안되기 때문에 server log로 남기고, 사용자에게는 잘 포장된(?) error page를 보여줘야 한다.
- db를 확인해도 없는 data에 대한 요청이 들어왔을 때 `get_object_404` 이용

```python
from django.shortcuts import render, redirect, get_object_or_404
from django.http import Http404

## (생략)

def detail(request, article_pk):
    try: # "에러가 날 것 같은 코드"가 있을 땐 무조건 try를 써서 error 검사를 해봐야 한다.
        article = Article.objects.get(pk=article_pk)
    except Article.DoesNotExist: # except 부문에 정확하게 어떤 경우에 에러를 발생시킬지 지정할 수 있다. (여기서는 models.DoesNotExist)
        raise Http404('해당하는 id의 글이 존재하지 않습니다.') # 에러를 발생 시켜줘야 한다. Http404 객체를 쓰면 된다.
    # article = get_object_or_404(pk=article_pk)를 비슷하게 4줄로 쓴 것
    # 최대한 get_object_or_404 메소드 쓰도록... 한줄이니까!

    context ={
        'article': article,
    }
    return render(request, 'articles/detail.html', context)
```



### CRUD 실습

- 수정 삭제는 최대한 POST로 보내줄 것

### delete

```html
<form action="{% url 'articles:delete' article.pk %}" method="POST">
  {% csrf_token %}
  <input type="submit" value="삭제">
</form>
```

- 삭제는 무조건 POST로 받을 때만 지워지게 해야 함 > 아니면 주소창에서 직접 쳐서도 지울 수 있기 때문...
- 그 외에는 detail로 보내게 한다.

```python
def delete(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    
    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    else:
        return redirect(article) # get_absolute_url를 지정했으므로 객체만 불러도 detail로 간다.
```



### update

```html
{% extends 'base.html' %}
{% load bootstrap4 %}

{% block body %}
<div class="container">
  <h1>수정하기</h1>
  <form action="{% url 'articles:update' article.pk %}" method="POST">
    {% csrf_token %}
    {% bootstrap_form form %}
    <button name="submit">제출</button>
  </form>
</div>
{% endblock %}
```

```python
def update(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'POST':
        form = ArticleForm(request.POST)
        if form.is_valid():
            article.title = form.cleaned_data.get('title')
            article.content = form.cleaned_data.get('content')
            article.save()
            return redirect(article)
        
    form = ArticleForm(initial={
        'title': article.title,
        'content': article.content,
    }) # 초기값을 설정해주려면 initial(또는 data)을 dict 값으로 지정해주면 된다.
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```



## Model Form

- Model과 Form의 각 기능을 갖고 있음

### ModelForm 설정하기

```python
from .models import Article

class ArticleForm(forms.ModelForm): #ModelForm을 지정
    class Meta: # 현재의 Model Form이 어떤 정보를 갖고 있는지 쥐고 있다.
        model = Article
        fields = '__all__' # 해당 모델의 모든 field를 가져오게 된다.
```

```python
def create(request):
    if request.method == "POST":
        form = ArticleForm(request.POST)
        if form.is_valid():
            return redirect(article)
        else:
            return redirect('articles:create')

    else:
        form = ArticleForm()
        context = {
            'form': form,
        }
        return render(request, 'articles/create.html', context
```

```python
def update(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    if request.method == 'POST':
        form = ArticleForm(request.POST, instance=article) # 1-1 여기를 먼저 수정하고(instance를 article로 지정)
        if form.is_valid():
            form.save() # 1-2 form.save()를 하면 update 효과가 있다.
            return redirect(article)
    form = ArticleForm(request.POST, instance=article)
    context = {
        'article': article,
        'form': form,
    }
    return render(request, 'articles/update.html', context)
```



### widget

- field customizing

```python
class ArticleForm(forms.ModelForm): #ModelForm을 지정
    class Meta: # 현재의 Model Form이 어떤 정보를 갖고 있는지 쥐고 있다.
        model = Article
        # fields = '__all__' # 해당 모델의 모든 field를 가져오게 된다.
        fields = ('title', 'content',) # __all__을 쓰기보단 명시적으로 해야 친절한 코드

    # 각 필드의 커스터마이징을 하고 싶으면 따로 따로 하면 된다.
    title = forms.CharField(
        max_length=20,
        label='제목',
        help_text='제목은 20자 이내로 써주세요.',
        widget=forms.TextInput(
            attrs={
                'class': 'form-control my-content',
                'placeholder': '제목을 입력해주세요.',
            }
        )
    )

    content = forms.CharField(
        label='내용',
        widget=forms.Textarea(
            attrs={
                'class': 'form-control my-content',
                'placeholder': '내용을 입력해주세요.',
                'rows': 5,
            }
        )
    )
```



## Remove 시 405 Error 처리

- Method not allowed
- decorator를 쓰면 된다.
- https://docs.djangoproject.com/en/2.2/topics/http/decorators/

```python
from django.views.decorators.http import require_POST # 보통은 POST만

@require_POST
def delete(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)

    if request.method == 'POST':
        article.delete()
        return redirect('articles:index')
    else:
        return redirect(article)
```





## 기타

- settings.py의 INSTALLATION_APP에 `django-extension'` 추가 및 pip install 후 `python manage.py shell_plus` 사용 가능
- https://developer.mozilla.org/ko/docs/Learn/Server-side/Django/Forms