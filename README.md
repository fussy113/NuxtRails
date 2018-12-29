## nekuraApp

### 概要

### 仕様

* Docker version 18.09.0, build 4d60db4
* docker-compose version 1.23.2, build 1110ad01
* Docker Container
  * db
    * mysql Ver 14.14 Distrib 5.7.24
  * backend
    * ruby 2.5.3p105
    * node 8.12.0
    * Rails 5.2.2
  * frontend
    * node 8.12.0
    * npm 6.4.1
    * yarn 1.9.4
    * Nuxt.js v2.3.4

### 動作まで(development)

```

git clone https://github.com/fussy113/nekuraApp.git PATH

cd PATH

cp .env.example .env

docker-compose build

docker-compose up -d db

docker-compose run --rm frontend yarn install

docker-compose run --rm backend bundle install

docker-compose run --rm backend rails db:migrate

docker-compose up backend frontend

```
