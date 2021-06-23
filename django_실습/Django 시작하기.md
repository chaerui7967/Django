# Django 시작하기



## Django 버전 확인 / 설치확인

```cmd
python -m django --version
```

 Django가 설치 되었다면, 설치된 Django의 버전을 확인할 수 있습니다. 만약 설치가 제대로 되지 않았다면, 《No module named django》와 같은 에러가 발생합니다.



## 프로젝트 만들기

```cmd
django-admin startproject mysite
```

현재 디렉토리에서 `mysite`라는 디렉토리를 생성할 것입니다. 만약 이 명령이 동작하지 않는다면, [django-admin을 실행하는 데 문제가 있습니다.](https://docs.djangoproject.com/ko/3.2/faq/troubleshooting/#troubleshooting-django-admin)

> 디렉토리 확인
>
> ```cmd
> mysite/
>     manage.py
>     mysite/
>         __init__.py
>         settings.py
>         urls.py
>         asgi.py
>         wsgi.py
> ```
>
>  이렇게 디렉토리가 생성되야 정상

- `manage.py`: Django 프로젝트와 다양한 방법으로 상호작용 하는 커맨드라인의 유틸리티 입니다. `manage.py` 에 대한 자세한 정보는 [django-admin and manage.py](https://docs.djangoproject.com/ko/3.2/ref/django-admin/) 에서 확인할 수 있습니다. cmd 창에 `python manage.py`입력 시 사용가능한 메소드 등이 나옵니다.
- `mysite/` 디렉토리 내부에는 프로젝트를 위한 실제 Python 패키지들이 저장됩니다. 이 디렉토리 내의 이름을 이용하여, (`mysite.urls` 와 같은 식으로) 프로젝트의 어디서나 Python 패키지들을 임포트할 수 있습니다.
- `mysite/__init__.py`: Python으로 하여금 이 디렉토리를 패키지처럼 다루라고 알려주는 용도의 단순한 빈 파일입니다. 
- `mysite/settings.py`: 현재 Django 프로젝트의 환경 및 구성을 저장합니다. [Django settings](https://docs.djangoproject.com/ko/3.2/topics/settings/)에서 환경 설정이 어떻게 동작하는지 확인할 수 있습니다.
- `mysite/urls.py`: 현재 Django project 의 URL 선언을 저장합니다. Django 로 작성된 사이트의 《목차》 라고 할 수 있습니다. [URL dispatcher](https://docs.djangoproject.com/ko/3.2/topics/http/urls/) 에서 URL 에 대한 자세한 내용을 읽어볼 수 있습니다.
- `mysite/asgi.py`:  [ASGI를 사용하여 배포하는 방법](https://docs.djangoproject.com/ko/3.2/howto/deployment/asgi/) 
- `mysite/wsgi.py`: 현재 프로젝트를 서비스하기 위한 WSGI 호환 웹 서버의 진입점입니다. [WSGI를 사용하여 배포하는 방법](https://docs.djangoproject.com/ko/3.2/howto/deployment/wsgi/)를 읽어보세요.



## 개발 서버

```cmd
python manage.py runserver
```

현재는 아무 앱을 설치하지 않았기 때문에 로켓 그림의 빈 홈페이지가 나옵니다.

> 포트 변경법
>
> 기본적으로, [`runserver`](https://docs.djangoproject.com/ko/3.2/ref/django-admin/#django-admin-runserver) 명령은 내부 IP 의 8000 번 포트로 개발 서버를 띄웁니다.
>
> 만약 이 서버의 포트를 변경하고 싶다면, 커맨드라인에서 인수를 전달해주면 됩니다. 예를들어, 이 명령은 포트를 8080 으로 서버를 시작할 것입니다.
>
> ```
> python manage.py runserver 8080
> ```
>
> 서버의 IP를 변경하려면 포트와 함께 전달하십시오. 예를 들어, 사용 가능한 모든 공용 IP를 청취하려면 (이는 Vagrant를 실행 중이거나 네트워크의 다른 컴퓨터에서 작업하고 싶을 때 유용합니다) 다음을 사용하십시오.
>
> ```
> python manage.py runserver 0:8000
> ```



## 앱 만들기

 앱을 생성하기 위해 `manage.py`가 존재하는 디렉토리에서 다음의 명령을 입력합니다.

```
$ python manage.py startapp polls
```

`polls`라는 디렉토리가 생기고, 이걸 펼쳐놓으면 아래와 같습니다.

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```



## 뷰 작성

```
<polls/views.py>

from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
```

Django에서 가장 간단한 형태의 뷰입니다. 뷰를 호출하려면 이와 연결된 URL 이 있어야 하는데, 이를 위해 URLconf가 사용됩니다.

polls 디렉토리에서 URLconf를 생성하려면, `urls.py`라는 파일을 생성해야 합니다. 정확히 생성했다면, 앱 디렉토리는 다음과 같이 보일 겁니다.

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    urls.py
    views.py
```



《polls/urls.py》 파일에는 다음과 같은 코드가 포함되어 있습니다.

```
from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```



다음 단계는, 최상위 URLconf 에서 `polls.urls` 모듈을 바라보게 설정합니다. `mysite/urls.py` 파일을 열고, `django.urls.include`를 import 하고, `urlpatterns` 리스트에 [`include()`](https://docs.djangoproject.com/ko/3.2/ref/urls/#django.urls.include) 함수를 다음과 같이 추가합니다.

《mysite/urls.py》

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]
```

[`include()`](https://docs.djangoproject.com/ko/3.2/ref/urls/#django.urls.include) 함수는 다른 URLconf들을 참조할 수 있도록 도와줍니다. Django가 함수 [`include()`](https://docs.djangoproject.com/ko/3.2/ref/urls/#django.urls.include)를 만나게 되면, URL의 그 시점까지 일치하는 부분을 잘라내고, 남은 문자열 부분을 후속 처리를 위해 include 된 URLconf로 전달합니다.

[`include()`](https://docs.djangoproject.com/ko/3.2/ref/urls/#django.urls.include)에 숨은 아이디어 덕분에 URL을 쉽게 연결할 수 있습니다. polls 앱에 그 자체의 URLconf(`polls/urls.py`)가 존재하는 한, 《/polls/》, 또는 《/fun_polls/》, 《/content/polls/》와 같은 경로, 또는 그 어떤 다른 root 경로에 연결하더라도, 앱은 여전히 잘 동작할 것입니다.



## 데이터베이스 설치

`mysite/settings.py` 파일을 열어보세요. 이 파일은 Django 설정을 모듈 변수로 표현한 보통의 Python 모듈입니다.

기본적으로는 SQLite을 사용하도록 구성되어 있습니다. 만약 데이터베이스를 처음 경험해보거나, Django에서 데이터베이스를 한번 경험해 보고 싶다면, SQLite가 가장 간단한 방법입니다. SQLite는 Python에서 기본으로 제공되기 때문에 별도로 설치할 필요가 없습니다. 그러나 실제 프로젝트를 시작할 때에는, 나중에 데이터베이스를 교체하느라 골치 아파질 일을 피하기 위해서라도 PostgreSQL 같이 좀 더 확장성 있는 데이터베이스를 사용하는 것이 좋습니다.

다른 데이터베이스를 사용해보고 싶다면, 적절한 [데이터베이스 바인딩](https://docs.djangoproject.com/ko/3.2/topics/install/#database-installation)을 설치하고, 데이터베이스 연결 설정과 맞게끔 [`DATABASES`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-DATABASES) `'default'` 항목의 값을 다음의 키 값으로 바꿔주세요.

- [`ENGINE`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-DATABASE-ENGINE) – `'django.db.backends.sqlite3'`, `'django.db.backends.postgresql'`, `'django.db.backends.mysql'`, 또는 `'django.db.backends.oracle'`. 그외에 [서드파티 백엔드](https://docs.djangoproject.com/ko/3.2/ref/databases/#third-party-notes) 참조.
- [`NAME`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-NAME) – The name of your database. If you’re using SQLite, the database will be a file on your computer; in that case, [`NAME`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-NAME) should be the full absolute path, including filename, of that file. The default value, `BASE_DIR / 'db.sqlite3'`, will store the file in your project directory.

SQLite 를 데이터베이스로 사용하지 않는 경우, [`USER`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-USER), [`PASSWORD`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-PASSWORD), [`HOST`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-HOST) 같은 추가 설정이 반드시 필요합니다. 

> mysql 같은경우는 
>
> ```python
> 'default': {
> 
> ​    'ENGINE': 'django.db.backends.mysql',
> 
> ​    'NAME': 'tip',
> 
> ​    'USER': 'root',
> 
> ​    'PASSWORD': '1234',
> 
> ​    'HOST': 'localhost',
> 
> ​    'PORT': '3306',
> 
>   }
> }
> ```
>
>  와 같이 작성합니다.



이 파일의 윗쪽에 있는 [`INSTALLED_APPS`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-INSTALLED_APPS) 에 대해 언급하자면, 이 파일은 현재 Django 인스턴스에서 활성화된 모든 Django 어플리케이션들의 이름이 담겨 있습니다. 앱들은 다수의 프로젝트에서 사용될 수 있고, 다른 프로젝트에서 쉽게 사용될 수 있도록 패키징하여 배포할 수 있습니다.

기본적으로는, [`INSTALLED_APPS`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-INSTALLED_APPS)는 Django와 함께 딸려오는 다음의 앱들을 포함합니다.

- [`django.contrib.admin`](https://docs.djangoproject.com/ko/3.2/ref/contrib/admin/#module-django.contrib.admin) – 관리용 사이트. 곧 사용하게 될 겁니다.
- [`django.contrib.auth`](https://docs.djangoproject.com/ko/3.2/topics/auth/#module-django.contrib.auth) – 인증 시스템.
- [`django.contrib.contenttypes`](https://docs.djangoproject.com/ko/3.2/ref/contrib/contenttypes/#module-django.contrib.contenttypes) – 컨텐츠 타입을 위한 프레임워크.
- [`django.contrib.sessions`](https://docs.djangoproject.com/ko/3.2/topics/http/sessions/#module-django.contrib.sessions) – 세션 프레임워크.
- [`django.contrib.messages`](https://docs.djangoproject.com/ko/3.2/ref/contrib/messages/#module-django.contrib.messages) – 메세징 프레임워크.
- [`django.contrib.staticfiles`](https://docs.djangoproject.com/ko/3.2/ref/contrib/staticfiles/#module-django.contrib.staticfiles) – 정적 파일을 관리하는 프레임워크.



setting을 다 변경했다면 

```cmd
python manage.py migrate
```

명령어를 통해 데이터베이스와 연결해줍니다.



## 모델만들기

본질적으로, 모델이란 부가적인 메타데이터를 가진 데이터베이스의 구조(layout)를 말합니다.

투표 페이지를 만들기 위해서

polls/models.py 에 다음을 넣어줍니다.

```
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

데이터베이스의 각 필드는 [`Field`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.Field) 클래스의 인스턴스로서 표현됩니다. [`CharField`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.CharField) 는 문자(character) 필드를 표현하고, [`DateTimeField`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.DateTimeField) 는 날짜와 시간(datetime) 필드를 표현합니다. 이것은 각 필드가 어떤 자료형을 가질 수 있는지를 Django 에게 말해줍니다.

각각의 [`Field`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.Field) 인스턴스의 이름(`question_text` 또는 `pub_date`)은 기계가 읽기 좋은 형식(machine-friendly format)의 데이터베이스 필드 이름입니다. 이 필드명을 Python 코드에서 사용할 수 있으며, 데이터베이스에서는 컬럼명으로 사용할 것입니다.

[`Field`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.Field) 클래스의 생성자에 선택적인 첫번째 위치 인수를 전달하여 사람이 읽기 좋은(human-readable) 이름을 지정할 수도 있습니다. 이 방법은 Django 의 내부를 설명하는 용도로 종종 사용되는데, 이는 마치 문서가 늘어나는 것 같은 효과를 가집니다. 만약 이 선택적인 첫번째 위치 인수를 사용하지 않으면, Django 는 기계가 읽기 좋은 형식의 이름을 사용합니다. 이 예제에서는, `Question.pub_date` 에 한해서만 인간이 읽기 좋은 형태의 이름을 정의하겠습니다. 그 외의 다른 필드들은, 기계가 읽기 좋은 형태의 이름이라도 사람이 읽기에는 충분합니다.

몇몇 [`Field`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.Field) 클래스들은 필수 인수가 필요합니다. 예를 들어, [`CharField`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.CharField) 의 경우 [`max_length`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.CharField.max_length) 를 입력해 주어야 합니다. 이것은 데이터베이스 스키마에서만 필요한것이 아닌 값을 검증할때도 쓰이는데, 곧 보게 될것입니다.

또한 [`Field`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.Field) 는 다양한 선택적 인수들을 가질 수 있습니다. 이 예제에서는, [`default`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.Field.default) 로 하여금 `votes` 의 기본값을 0 으로 설정하였습니다.

마지막으로, [`ForeignKey`](https://docs.djangoproject.com/ko/3.2/ref/models/fields/#django.db.models.ForeignKey) 를 사용한 관계설정에 대해 설명하겠습니다. 이 예제에서는 각각의 `Choice` 가 하나의 `Question` 에 관계된다는 것을 Django 에게 알려줍니다. Django 는 다-대-일(many-to-one), 다-대-다(many-to-many), 일-대-일(one-to-one) 과 같은 모든 일반 데이터베이스의 관계들를 지원합니다.



## 모델 활성화

앱을 현재의 프로젝트에 포함시키기 위해서는, 앱의 구성 클래스에 대한 참조를 [`INSTALLED_APPS`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-INSTALLED_APPS) 설정에 추가해야 합니다. `PollsConfig` 클래스는 `polls/apps.py` 파일 내에 존재합니다. 따라서, 점으로 구분된 경로는 `'polls.apps.PollsConfig'`가 됩니다. 이 점으로 구분된 경로를, `mysite/settings.py` 파일을 편집하여 [`INSTALLED_APPS`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-INSTALLED_APPS) 설정에 추가하면 됩니다. 이는 다음과 같이 보일 것입니다.

mysite/settings.py

```
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

다음 셋팅이 끝났다면 poll 앱과 데이터베이스와 연결합니다.

```cmd
python manage.py makemigrations polls
```

`makemigrations` 을 실행시킴으로서, 당신이 모델을 변경시킨 사실과(이 경우에는 새로운 모델을 만들었습니다) 이 변경사항을 *migration*으로 저장시키고 싶다는 것을 Django에게 알려줍니다.

migration들을 실행시켜주고, 자동으로 데이터베이스 스키마를 관리해주는 [`migrate`](https://docs.djangoproject.com/ko/3.2/ref/django-admin/#django-admin-migrate) 명령어가 있습니다. 이 명령을 알아보기 전에 migration이 내부적으로 어떤 SQL 문장을 실행하는지 살펴봅시다. [`sqlmigrate`](https://docs.djangoproject.com/ko/3.2/ref/django-admin/#django-admin-sqlmigrate) 명령은 migration 이름을 인수로 받아, 실행하는 SQL 문장을 보여줍니다.



```cmd
 python manage.py sqlmigrate polls 0001
```

다음과 비슷한 결과를 보실 수 있습니다. (가독성을 위해 결과물을 조금 다듬었습니다)

```cmd
BEGIN;
--
-- Create model Question
--
CREATE TABLE "polls_question" (
    "id" serial NOT NULL PRIMARY KEY,
    "question_text" varchar(200) NOT NULL,
    "pub_date" timestamp with time zone NOT NULL
);
--
-- Create model Choice
--
CREATE TABLE "polls_choice" (
    "id" serial NOT NULL PRIMARY KEY,
    "choice_text" varchar(200) NOT NULL,
    "votes" integer NOT NULL,
    "question_id" integer NOT NULL
);
ALTER TABLE "polls_choice"
  ADD CONSTRAINT "polls_choice_question_id_c5b4b260_fk_polls_question_id"
    FOREIGN KEY ("question_id")
    REFERENCES "polls_question" ("id")
    DEFERRABLE INITIALLY DEFERRED;
CREATE INDEX "polls_choice_question_id_c5b4b260" ON "polls_choice" ("question_id");

COMMIT;
```

이제, [`migrate`](https://docs.djangoproject.com/ko/3.2/ref/django-admin/#django-admin-migrate) 를 실행시켜 데이터베이스에 모델과 관련된 테이블을 생성해봅시다.

```cmd
python manage.py migrate
```

[`migrate`](https://docs.djangoproject.com/ko/3.2/ref/django-admin/#django-admin-migrate) 명령은 아직 적용되지 않은 마이그레이션을 모두 수집해 이를 실행하며(Django는 `django_migrations` 테이블을 두어 마이그레이션 적용 여부를 추적합니다) 이 과정을 통해 모델에서의 변경 사항들과 데이터베이스의 스키마의 동기화가 이루어집니다.

**모델의 변경을 만드는 세 단계의 지침**

- (`models.py` 에서) 모델을 변경합니다.
- [`python manage.py makemigrations`](https://docs.djangoproject.com/ko/3.2/ref/django-admin/#django-admin-makemigrations)을 통해 이 변경사항에 대한 마이그레이션을 만드세요.
- [`python manage.py migrate`](https://docs.djangoproject.com/ko/3.2/ref/django-admin/#django-admin-migrate) 명령을 통해 변경사항을 데이터베이스에 적용하세요.



## API 활용

Python 쉘을 실행하려면 다음의 명령을 입력합니다.

```cmd
 python manage.py shell
```

```
from polls.models import Choice, Question  # Import the model classes we just wrote.

# 아직 questions 을 넣지 않았기 때문에 빈 쿼리 셋이 나온다
>>> Question.objects.all()
<QuerySet []>

# Create a new Question.

>>> from django.utils import timezone
>>> q = Question(question_text="What's new?", pub_date=timezone.now())
# 세이브 하지 않으면 데이터베이스에 반영되지 않는다.
>>> q.save()

# id를 확인하면 현재 넣은 q값의 id값을 확인할수 있다.
>>> q.id
1
>>> q.question_text
"What's new?"
>>> q.pub_date
datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=<UTC>)

# 한개 더 넣어보자
>>> q.question_text = "What's up?"
>>> q.save()

# 지금가지 넣었던 쿼리셋 모두를 보여준다.
>>> Question.objects.all()
<QuerySet [<Question: Question object (1)>]>
```

`<Question: Question object (1)>`은 이 객체를 표현하는 데 별로 도움이 되지 않습니다. (`polls/models.py` 파일의) `Question` 모델을 수정하여, [`__str__()`](https://docs.djangoproject.com/ko/3.2/ref/models/instances/#django.db.models.Model.__str__) 메소드를 `Question`과 `Choice`에 추가해 봅시다.

polls/models.py

```
from django.db import models

class Question(models.Model):
    # ...
    def __str__(self):
        return self.question_text

class Choice(models.Model):
    # ...
    def __str__(self):
        return self.choice_text
```

당신의 모델에 [`__str__()`](https://docs.djangoproject.com/ko/3.2/ref/models/instances/#django.db.models.Model.__str__) 메소드를 추가하는것은 객체의 표현을 대화식 프롬프트에서 편하게 보려는 이유 말고도, Django 가 자동으로 생성하는 관리 사이트 에서도 객체의 표현이 사용되기 때문입니다.

이 모델에 커스텀 메소드 또한 추가해봅시다:

polls/models.py

```
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)
```

`import datetime`은 Python의 표준 모듈인 [`datetime`](https://docs.python.org/3/library/datetime.html#module-datetime) 모듈을, `from django.utils import timezone`은 Django의 시간대 관련 유틸리티인 [`django.utils.timezone`](https://docs.djangoproject.com/ko/3.2/ref/utils/#module-django.utils.timezone)을 참조하기 위해 추가한 것입니다. 

변경된 사항을 저장하고, `python manage.py shell`를 다시 실행해보세요.

```cmd
>>> from polls.models import Choice, Question

>>> Question.objects.all()
<QuerySet [<Question: What's up?>]>

# id값으로도 부를수 있다.
>>> Question.objects.filter(id=1)
<QuerySet [<Question: What's up?>]>
>>> Question.objects.filter(question_text__startswith='What')
<QuerySet [<Question: What's up?>]>

>>> from django.utils import timezone
>>> current_year = timezone.now().year
>>> Question.objects.get(pub_date__year=current_year)
<Question: What's up?>


>>> Question.objects.get(id=2)
Traceback (most recent call last):
    ...
DoesNotExist: Question matching query does not exist.

>>> Question.objects.get(pk=1)
<Question: What's up?>

>>> q = Question.objects.get(pk=1)
>>> q.was_published_recently()
True


# id 1번값에 대한 choice값 입력
>>> q = Question.objects.get(pk=1)

>>> q.choice_set.all()
<QuerySet []>

# Create three choices.
>>> q.choice_set.create(choice_text='Not much', votes=0)
<Choice: Not much>
>>> q.choice_set.create(choice_text='The sky', votes=0)
<Choice: The sky>
>>> c = q.choice_set.create(choice_text='Just hacking again', votes=0)

# Choice objects have API access to their related Question objects.
>>> c.question
<Question: What's up?>

>>> q.choice_set.all()
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>
>>> q.choice_set.count()
3

>>> Choice.objects.filter(question__pub_date__year=current_year)
<QuerySet [<Choice: Not much>, <Choice: The sky>, <Choice: Just hacking again>]>

# choice 값 삭제, 값 삭제는 워크벤치에서도 가능하다
>>> c = q.choice_set.filter(choice_text__startswith='Just hacking')
>>> c.delete()
```



## 관리자 생성

```
python manage.py createsuperuser
```

원하는 username 을 입력하고 엔터를 누르세요

```
Username: admin
```

그런 다음 원하는 이메일 주소를 입력하라는 메시지가 표시됩니다.

```
Email address: admin@example.com
```

마지막으로, 암호를 입력하세요. 암호를 두번 물어보게 되는데, 두번째 입력하는 암호를 올바로 입력했는지를 확인하기 위한 암호입니다.

```
Password: **********
Password (again): *********
Superuser created successfully.
```

http://127.0.0.1:8000/admin/ 을 통해서 관리자 페이지로 접속할 수 있다.



### 관리 사이트에서 polls 변경 가능하게 만들기

polls/admin.py

```
from django.contrib import admin

from .models import Question

admin.site.register(Question)
```

해당 파일에서 값을 넣어준다.



## 뷰 추가

`polls/views.py` 에 뷰를 추가해 봅시다. 이 뷰들은 인수를 받기 때문에 조금 모양이 다릅니다.

polls/views.py

```
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)
```

다음의 [`path()`](https://docs.djangoproject.com/ko/3.2/ref/urls/#django.urls.path) 호출을 추가하여 이러한 새로운 뷰를 `polls.urls` 모듈로 연결하세요.

polls/urls.py

```
from django.urls import path

from . import views

urlpatterns = [
    # ex: /polls/
    path('', views.index, name='index'),
    # ex: /polls/5/
    path('<int:question_id>/', views.detail, name='detail'),
    # ex: /polls/5/results/
    path('<int:question_id>/results/', views.results, name='results'),
    # ex: /polls/5/vote/
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```

사용자가 웹사이트의 페이지를 요청할 때, 예로 《/polls/34/》를 요청했다고 하면, Django는 `mysite.urls` 파이썬 모듈을 불러오게 됩니다. [`ROOT_URLCONF`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-ROOT_URLCONF) 설정에 의해 해당 모듈을 바라보도록 지정되어 있기 때문입니다. `mysite.urls`에서 `urlpatterns`라는 변수를 찾고, 순서대로 패턴을 따라갑니다. `'polls/'`를 찾은 후엔, 일치하는 텍스트(`"polls/"`)를 버리고, 남은 텍스트인 `"34/"`를 〈polls.urls〉 URLconf로 전달하여 남은 처리를 진행합니다. 거기에 `'<int:question_id>/'`와 일치하여, 결과적으로 `detail()` 뷰 함수가 호출됩니다.

```
detail(request=<HttpRequest object>, question_id=34)
```

`question_id=34` 부분은 `<int:question_id>` 에서 왔습니다. 괄호를 사용하여 URL 의 일부를 《캡처》하고, 해당 내용을 keyword 인수로서 뷰 함수로 전달합니다. 문자열의 `:question_id>` 부분은 일치되는 패턴을 구별하기 위해 정의한 이름이며, `<int:` 부분은 어느 패턴이 해당 URL 경로에 일치되어야 하는 지를 결정하는 컨버터입니다.



## 뷰에 기능 넣기

각 뷰는 두 가지 중 하나를 하도록 되어 있습니다. 요청된 페이지의 내용이 담긴 [`HttpResponse`](https://docs.djangoproject.com/ko/3.2/ref/request-response/#django.http.HttpResponse) 객체를 반환하거나, 혹은 [`Http404`](https://docs.djangoproject.com/ko/3.2/topics/http/views/#django.http.Http404) 같은 예외를 발생하게 해야합니다.

Django에 필요한 것은 [`HttpResponse`](https://docs.djangoproject.com/ko/3.2/ref/request-response/#django.http.HttpResponse) 객체 혹은 예외입니다.

Django 자체 데이터베이스 API를 사용해봅시다. 새로운 `index()` 뷰 하나를 호출했을 때, 시스템에 저장된 최소한 5 개의 투표 질문이 콤마로 분리되어, 발행일에 따라 출력됩니다.


polls/views.py

```
from django.http import HttpResponse

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    output = ', '.join([q.question_text for q in latest_question_list])
    return HttpResponse(output)

# Leave the rest of the views (detail, results, vote) unchanged
```

여기 몇가지 문제가 있습니다. 뷰에서 페이지의 디자인이 하드코딩 되어 있다고 합시다. 만약 페이지가 보여지는 방식을 바꾸고 싶다면, 이 Python 코드를 편집해야만 할 겁니다. 

우선, `polls` 디렉토리에 `templates`라는 디렉토리를 만듭니다. Django는 여기서 템플릿을 찾게 될 것입니다.

프로젝트의 [`TEMPLATES`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-TEMPLATES) 설정은 Django가 어떻게 템플릿을 불러오고 렌더링 할 것인지 기술합니다. 기본 설정 파일은 [`APP_DIRS`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-TEMPLATES-APP_DIRS) 옵션이 `True`로 설정된 `DjangoTemplates` 백엔드를 구성합니다. 관례에 따라, `DjangoTemplates`은 각 [`INSTALLED_APPS`](https://docs.djangoproject.com/ko/3.2/ref/settings/#std:setting-INSTALLED_APPS) 디렉토리의 《templates》 하위 디렉토리를 탐색합니다.

템플릿에 다음과 같은 코드를 입력합니다.

polls/templates/polls/index.html

```
{% if latest_question_list %}
    <ul>
    {% for question in latest_question_list %}
        <li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
    {% endfor %}
    </ul>
{% else %}
    <p>No polls are available.</p>
{% endif %}
```

이제, 템플릿을 이용하여 `polls/views.py`에 `index` 뷰를 업데이트 해보도록 하겠습니다.


polls/views.py

```
from django.http import HttpResponse
from django.template import loader

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    template = loader.get_template('polls/index.html')
    context = {
        'latest_question_list': latest_question_list,
    }
    return HttpResponse(template.render(context, request))
```

이 코드는 `polls/index.html` 템플릿을 불러온 후, context를 전달합니다. context는 템플릿에서 쓰이는 변수명과 Python 객체를 연결하는 사전형 값입니다.

브라우저에서 《/polls/》 페이지를 불러오면,  작성한 《What’s up》 질문이 포함된 리스트가 표시됩니다. 표시된 질문의 링크는 해당 질문에 대한 세부 페이지를 가리킵니다.

### render()

 Django는 이런 표현을 쉽게 표현할 수 있도록 단축 기능(shortcuts)을 제공합니다. `index()` 뷰를 단축 기능으로 작성하면 다음과 같습니다.

polls/views.py

```
from django.shortcuts import render

from .models import Question


def index(request):
    latest_question_list = Question.objects.order_by('-pub_date')[:5]
    context = {'latest_question_list': latest_question_list}
    return render(request, 'polls/index.html', context)
```

모든 뷰에 적용한다면, 더 이상 [`loader`](https://docs.djangoproject.com/ko/3.2/topics/templates/#module-django.template.loader)와 [`HttpResponse`](https://docs.djangoproject.com/ko/3.2/ref/request-response/#django.http.HttpResponse)를 임포트하지 않아도 됩니다. (만약 `detail`, `results`, `vote`에서 stub 메소드를 가지고 있다면, `HttpResponse`를 유지해야 할 것입니다.)

[`render()`](https://docs.djangoproject.com/ko/3.2/topics/http/shortcuts/#django.shortcuts.render) 함수는 request 객체를 첫번째 인수로 받고, 템플릿 이름을 두번째 인수로 받으며, context 사전형 객체를 세전째 선택적(optional) 인수로 받습니다. 인수로 지정된 context로 표현된 템플릿의 [`HttpResponse`](https://docs.djangoproject.com/ko/3.2/ref/request-response/#django.http.HttpResponse) 객체가 반환됩니다.



## 404 에러

polls/views.py

```
from django.http import Http404
from django.shortcuts import render

from .models import Question
# ...
def detail(request, question_id):
    try:
        question = Question.objects.get(pk=question_id)
    except Question.DoesNotExist:
        raise Http404("Question does not exist")
    return render(request, 'polls/detail.html', {'question': question})
```

여기 새로운 내용이 추가되었습니다. 뷰는 요청된 질문의 ID 가 없을 경우 [`Http404`](https://docs.djangoproject.com/ko/3.2/topics/http/views/#django.http.Http404) 예외를 발생시킵니다.

polls/templates/polls/detail.html

```
{{ question }}
```



### get_object_or_404()

만약 객체가 존재하지 않을 때 [`get()`](https://docs.djangoproject.com/ko/3.2/ref/models/querysets/#django.db.models.query.QuerySet.get) 을 사용하여 [`Http404`](https://docs.djangoproject.com/ko/3.2/topics/http/views/#django.http.Http404) 예외를 발생시키는것은 자주 쓰이는 용법입니다. Django에서 이 기능에 대한 단축 기능을 제공합니다. `detail()` 뷰를 단축 기능으로 작성하면 다음과 같습니다.

polls/views.py

```
from django.shortcuts import get_object_or_404, render

from .models import Question
# ...
def detail(request, question_id):
    question = get_object_or_404(Question, pk=question_id)
    return render(request, 'polls/detail.html', {'question': question})
```

[`get_object_or_404()`](https://docs.djangoproject.com/ko/3.2/topics/http/shortcuts/#django.shortcuts.get_object_or_404) 함수는 Django 모델을 첫번째 인자로 받고, 몇개의 키워드 인수를 모델 관리자의 [`get()`](https://docs.djangoproject.com/ko/3.2/ref/models/querysets/#django.db.models.query.QuerySet.get) 함수에 넘깁니다. 만약 객체가 존재하지 않을 경우, [`Http404`](https://docs.djangoproject.com/ko/3.2/topics/http/views/#django.http.Http404) 예외가 발생합니다.

또한, [`get_object_or_404()`](https://docs.djangoproject.com/ko/3.2/topics/http/shortcuts/#django.shortcuts.get_object_or_404) 함수처럼 동작하는 [`get_list_or_404()`](https://docs.djangoproject.com/ko/3.2/topics/http/shortcuts/#django.shortcuts.get_list_or_404) 함수가 있습니다. [`get()`](https://docs.djangoproject.com/ko/3.2/ref/models/querysets/#django.db.models.query.QuerySet.get) 대신 [`filter()`](https://docs.djangoproject.com/ko/3.2/ref/models/querysets/#django.db.models.query.QuerySet.filter) 를 쓴다는 것이 다릅니다. 리스트가 비어있을 경우, [`Http404`](https://docs.djangoproject.com/ko/3.2/topics/http/views/#django.http.Http404) 예외를 발생시킵니다.



## 템플릿 시스템 사용

polls/templates/polls/detail.html

```
<h1>{{ question.question_text }}</h1>
<ul>
{% for choice in question.choice_set.all %}
    <li>{{ choice.choice_text }}</li>
{% endfor %}
</ul>
```

템플릿 시스템은 변수의 속성에 접근하기 위해 점-탐색(dot-lookup) 문법을 사용합니다. 예제의 `{{ question.question_text }}` 구문을 보면, Django는 먼저 `question` 객체에 대해 사전형으로 탐색합니다. 탐색에 실패하게 되면 속성값으로 탐색합니다. (이 예에서는 속성값에서 탐색이 완료됩니다만) 만약 속성 탐색에도 실패한다면 리스트의 인덱스 탐색을 시도하게 됩니다.

[`{% for %}`](https://docs.djangoproject.com/ko/3.2/ref/templates/builtins/#std:templatetag-for) 반복 구문에서 메소드 호출이 일어납니다. `question.choice_set.all`은 Python에서 `question.choice_set.all()` 코드로 해석되는데, 이때 반환된 `Choice` 객체의 반복자는 [`{% for %}`](https://docs.djangoproject.com/ko/3.2/ref/templates/builtins/#std:templatetag-for)에서 사용하기 적당합니다.



## 템플릿에서 하드코딩된 URL 제거하기

`polls/index.html` 템플릿에 링크를 적으면, 이 링크는 다음과 같이 부분적으로 하드코딩된다는 것을 기억하세요.

```
<li><a href="/polls/{{ question.id }}/">{{ question.question_text }}</a></li>
```

이러한 강력하게 결합되고 하드코딩된 접근방식의 문제는 수 많은 템플릿을 가진 프로젝트들의 URL을 바꾸는 게 어려운 일이 된다는 점입니다. 그러나, `polls.urls` 모듈의 [`path()`](https://docs.djangoproject.com/ko/3.2/ref/urls/#django.urls.path) 함수에서 인수의 이름을 정의했으므로, `{% url %}` template 태그를 사용하여 url 설정에 정의된 특정한 URL 경로들의 의존성을 제거할 수 있습니다.

```
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

이것이 `polls.urls` 모듈에 서술된 URL 의 정의를 탐색하는 식으로 동작합니다. 다음과 같이 〈detail〉 이라는 이름의 URL 이 어떻게 정의되어 있는지 확인할 수 있습니다.

```
...
# the 'name' value as called by the {% url %} template tag
path('<int:question_id>/', views.detail, name='detail'),
...
```

만약 상세 뷰의 URL을 `polls/specifics/12/`로 바꾸고 싶다면, 템플릿에서 바꾸는 것이 아니라 `polls/urls.py`에서 바꿔야 합니다.

```
...
# added the word 'specifics'
path('specifics/<int:question_id>/', views.detail, name='detail'),
...
```



## URL의 이름공간 정하기

 예를 들어, `polls` 앱은 `detail`이라는 뷰를 가지고 있고, 동일한 프로젝트에 블로그를 위한 앱이 있을 수도 있습니다. Django가 `{% url %}` 템플릿태그를 사용할 때, 어떤 앱의 뷰에서 URL을 생성할지 알 수 있을까요?

정답은 URLconf에 이름공간(namespace)을 추가하는 것입니다. `polls/urls.py` 파일에 `app_name`을 추가하여 어플리케이션의 이름공간을 설정할 수 있습니다.

polls/urls.py

```
from django.urls import path

from . import views

app_name = 'polls'
urlpatterns = [
    path('', views.index, name='index'),
    path('<int:question_id>/', views.detail, name='detail'),
    path('<int:question_id>/results/', views.results, name='results'),
    path('<int:question_id>/vote/', views.vote, name='vote'),
]
```

이제, `polls/index.html` 템플릿의 기존 내용을

polls/templates/polls/index.html

```
<li><a href="{% url 'detail' question.id %}">{{ question.question_text }}</a></li>
```

아래와 같이 이름공간으로 나눠진 상세 뷰를 가리키도록 변경합니다.

polls/templates/polls/index.html

```
<li><a href="{% url 'polls:detail' question.id %}">{{ question.question_text }}</a></li>
```

