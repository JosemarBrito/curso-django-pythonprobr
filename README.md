# Curso-django-pythonprobr

Repositório criado para aprendizagem com Framework Django na criação de paginas Web

Aplicação disponivel em https://pybritoscode.herokuapp.com/

Repositório no [Git-Hub](https://github.com/JosemarBrito/curso-django-pythonprobr)

Pagina de Criação [Django](https://pybritoscode.herokuapp.com/)


[![codecov](https://codecov.io/gh/JosemarBrito/curso-django-pythonprobr/branch/main/graph/badge.svg?token=B1P6KGBUKQ)](undefined)
[![Updates](https://pyup.io/repos/github/JosemarBrito/curso-django-pythonprobr/shield.svg)](https://pyup.io/repos/github/JosemarBrito/curso-django-pythonprobr/)
[![Python 3](https://pyup.io/repos/github/JosemarBrito/curso-django-pythonprobr/python-3-shield.svg)](https://pyup.io/repos/github/JosemarBrito/curso-django-pythonprobr/)


Instalado GitHub Action apartir do repositorio

[PythonPro-Website](https://github.com/pythonprobr/pythonpro-website/blob/master/.github/workflows/django_project.yml)

Utilizando Github/workflows

__________________________________________________________

Cofiguração no [Travic CI](https://travis-ci.org/signin)
=

language: python

dist: 
xenial

sudo: true

python:

    - 3.8

services:
    postgresql
    
addons:
    postgresql: '9.5'

env:
    global:
    
        - PIPENV_VENV_IN_PROJECT=1
        - PIPENV_IGNORE_VIRTUALENVS=1

install:

    - pip install pipenv
    - pipenv sync -d
    - cp contrib/env-sample .env
    - pipenv install flake8

before_script:

    - psql -c "CREATE DATABASE testdb;" -U postgres
script:

    - pipenv run flake8 .
    - pipenv run pytest --cov=pybrito

after_success:

    - pipenv run codecov
    
    
_________________________________________________

Cofiguração no [Github Action](https://github.com/features/actions)
=

name: Python application

on: [pull_request]

jobs:

  build:
  
    env:
    
      PIPENV_NO_INHERIT: 1
      PIPENV_IGNORE_VIRTUALENVS: 1
    runs-on: ubuntu-latest

    services:
      postgres:
        image: postgres:11.5
        env:
          POSTGRES_USER: postgres
          POSTGRES_PASSWORD: postgres
          POSTGRES_DB: postgres
        ports: ['5432:5432']

    steps:
    
    - uses: actions/checkout@v2
    - name: Set up Python 3.8
      uses: actions/setup-python@v1
      with:
        python-version: 3.8
    - name: Copying configurations
      run: |
        cp contrib/env-sample .env
    - name: Install dependencies
      run: |
        pip install pipenv
        pipenv sync --dev
    - name: Lint with flake8
      run: |
        pipenv run flake8 .
    - name: Test with pytest
      run: |
        pipenv run pytest pybrito --cov=pybrito
    - name: Posting Coverage
      env:
        CODECOV_TOKEN: "e120730f-f9fa-4915-8aac-78392909c37b"
      run: |
        pipenv run codecov  