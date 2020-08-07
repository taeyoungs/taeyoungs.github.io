---
layout: post
title: "Django + AWS EB"
date: Thu August 6 2020 22:20:20 GMT+0900
author: Jang Taeyoung
categories: python
tags: python django awseb
cover: "/assets/python.png"
---

> 나를 위한 Django + AWS Elastic Beanstalk 개념 정리

<br />

## Elastic Beanstack (EB)

<hr />

- 환경 설정까지 된 서버를 만들어주는 서비스

git commit 된 사항들을 가지고 가기 때문에 eb 연결 후에 eb deploy로 commit 된 사항들을 반영시켜야한다.

[Django 애플리케이션을 Elastic Beanstalk에 배포](https://docs.aws.amazon.com/ko_kr/elasticbeanstalk/latest/dg/create-deploy-python-django.html)

1. pip3 install awsebcli <br />
   ⇒ eb로 작업하기 위한 cli 설치, pip3를 통해 전역으로 설치해준다.
2. eb에 사용할 user 생성 <br />
   ⇒ IAM 서비스로 이동하여 새로운 유저를 생성해주자.
3. 원래는 관리자 권한을 주면 안되지만 학습하는 과정이니 관리자 권한으로 진행 <br />
   ⇒ AdministratorAccess
4. eb init으로 eb 기본 설정 시작 <br />
   ⇒ 유저 생성할 때 만들어진 id, key 값 입력 <br />
   ⇒ SSH는 서버에 직접 들어가서 파일을 열어보거나 직접 작업하는 경우에 서버 연결을 위한 설정

<br />

### .ebextensions 폴더

<hr />

- django.config

엘라스틱 빈스텍 안에 있는 django 환경 설정 정보

- packages.config

엘라스틱 빈스텍 안에서 필요한 소프트웨어 정보

⭐ manage.py runserver는 개발 모드에서만 사용하는 명령어, 프로덕션 모드에서는 wsgi.py라는
명령어를 사용해서 코드를 실행한다.

따라서, django.config 파일에 wsgi.py 파일이 어디에 존재하는지 알려줘야 한다.

1. git commit 후 eb create django-env 실행 <br />
   ⇒ 가상 환경 만들기 시작, 인스턴스를 대신 만들어준다고 생각하면 된다.

<br />

### requirements.txt

<hr />

히로쿠나 AWS와 같은 서비스를 이용하게 될 경우에 설치할 환경 설정들이 적혀있는 txt 파일이며 node에서 package.json과 같은 역할을 담당한다.

AWS 공식 문서를 참고하여 requirements.txt를 만드는 명령어를 사용해도 되고 python module을 사용해도 된다. <br />
내 경우에는 [pipenv-to-requirements](https://pypi.org/project/pipenv-to-requirements/) 모듈을 이용해서 requirements.txt를 만들었다.

<br /><br />

## PostgresQL

<hr />

프로덕션 모드에서 사용하게 될 DB이며, sqlite3는 개발 모드에서만 사용한다. <br />
⇒ env 파일에 DEBUG 변수를 이용하여 개발 모드일 경우와 프로덕션 모드일 경우를 구별하자. <br /><br />

EB는 env 파일을 사용하지 않기 때문에 (개발 모드일 경우에만 사용, gitignore에 포함된 파일) Software 설정에 들어가서 필요한 변수와 값들을 설정해준다. 서버에 필요한 python package들은 requirements.txt를 통하면 되지만 지금은 서버 자체에 postgres 소프트웨어가 필요한 상황이다.

- packages.config

위 파일로 EB에 설치되어야할 소프트웨어에 대한 정보를 작성하며, 이 파일 또한 git commit + eb deploy로 서버의 환경 설정을 업데이트 해주자.

[PostgresQL Django Setup Docs](https://docs.djangoproject.com/en/3.0/ref/settings/#std:setting-HOST)

<br />

### 무한 로딩이 되는 경우

<hr />

환경 설정을 모두 완료하고도 무한 로딩이 생긴다면 RDS 측에서 EB와 소통하지 못하는 상황이다. 보통 제품을 만들 때 DB를 서버 내에 집어넣지 않고 분리하기 때문에 생기는 경우인데, RDS 측 보안 그룹에 EB를 허용해주고 있지 않기 때문이다. 이와 같은 상황을 해결하기 위해서 RDS 보안 그룹에서 EB를 허용해주고 다시 접속해주도록 하자.

<br />

### 데이터베이스 마이그레이션 구성 파일 추가

<hr />

사이트가 업데이트될 때 실행할 '.ebextensions' 스크립트에 명령을 추가할 수 있으며 이를 통해 데이터베이스 마이그레이션을 자동으로 생성할 수 있다. 이런 작업이 필요한 이유는 makemigrations로 생성된 파일들이 EB에서 migrate 되지 않기 때문이다.

**애플리케이션을 배포할 때 마이그레이션 단계를 추가하려면**

```python
container_commands:
    01_migrate:
        command: "django-admin.py migrate"
        leader_only: true
   option_settings:
     aws:elasticbeanstalk:application:environment:
       DJANGO_SETTINGS_MODULE: config.settings
```

이 구성 파일은 애플리케이션을 시작하기 전에 배포 프로세스 중에 `django-admin.py migrate` 명령을 하게 된다. 명령에서 `leader_only: true`를 지정하면 여러 인스턴스에 배포할 때 한 번만 실행된다.

<br />

++

```python
02_compilemessages:
    command: "django-admin.py compilemessages"
```

django.mo가 gitignore 파일로 인해 git에 저장되지 않는다. 따라서, 이를 EB에서 작업하도록 커맨드를 넣어줘야 하며, EB에는 gettext 소프트웨어가 없기 때문에 packages.config 파일에 gettext 소프트웨어에 대한 설정 정보를 작성해줘야 한다.

<br /><br />

## Sentry

<hr />

DEBUG false인 상황에서 오류가 발생할 경우에 해당 오류를 추적하여 보여주고 이메일로 오류를 정리해서 보내주는 서비스이다. (무료 + 유료)

`settings.py`

```python
# Sentry

if not DEBUG:

    sentry_sdk.init(
        dsn=os.environ.get("SENTRY_URL"),
        integrations=[DjangoIntegration()],
        # If you wish to associate users to errors (assuming you are using
        # django.contrib.auth) you may enable sending PII data.
        send_default_pii=True,
    )
```

<br /><br />

## S3 for static files

<hr />

AWS EB에 배포할 때마다 새롭게 서버를 만들기 때문에 서버 내에 static file이나 유저 정보를 넣어놓을 경우 초기화된다. 따라서, 따로 이 정보들을 저장하고 있는 서버가 필요하다. <br />
⇒ AWS S3 서버

[django-storages](https://django-storages.readthedocs.io/en/latest/backends/amazon-S3.html)

<br />

### Amazon S3 Setting

<hr />

```python
if not DEBUG:

    DEFAULT_FILE_STORAGE = "config.custom_storages.UploadStorage"
    STATICFILES_STORAGE = "config.custom_storages.StaticStorage"
    AWS_ACCESS_KEY_ID = os.environ.get("AWS_ACCESS_ID")
    AWS_SECRET_ACCESS_KEY = os.environ.get("AWS_SECRET_KEY")
    AWS_STORAGE_BUCKET_NAME = "airbnb-clone-youngslog"
    AWS_S3_OBJECT_PARAMETERS = {"CacheControl": "max-age=86400"}
    AWS_AUTO_CREATE_BUCKET = True
    AWS_BUCKET_ACL = "public-read"

    AWS_S3_CUSTOM_DOMAIN = f"{AWS_STORAGE_BUCKET_NAME}.s3.amazonaws.com"
    STATIC_URL = f"https://{AWS_S3_CUSTOM_DOMAIN}/static/"
```

<br />

### ⭐⭐ green & blue 운영

<hr />

서버를 배포할 때 배포하는 과정에서 앱이 잠시 죽어있는 경우가 생긴다. 이때, 서비스가 중지되는 것을 방지하기 위해서 2가지 서버를 만든다. 하나는 기존 서버이고 하나는 새로운 환경으로 배포되서 생성된 서버이다. 새로운 환경으로 서버가 생성 완료되면 url만 옮겨서 잠시라도 서버가 죽는 경우가 없도록 방지하는 운영 방식을 말한다.

<br /><br />
