## practice1_basics_Devops
## practice1_basics_Devops

## Содержание
* [1. Проект: CI/CD pipeline для GitHub Actions](#Проект: CI/CD pipeline для GitHub Actions)
* [1.1 Описание проекта CI/CD Pipeline с использованием GitHub Actions](#Описание проекта CI/CD Pipeline с использованием GitHub Actions)
* [## 1.2 Описание целей и этапов pipeline.](#Описание целей и этапов pipeline.)
* [## 1.3 Таблица шагов и триггеров](#Таблица шагов и триггеров)
* [## 2 Ход работы ](#Ход работы)
* [## 2.1 Пример блока кода для конфигурационного файла ](# Пример блока кода для конфигурационного файла)

# 1. Проект: CI/CD pipeline для GitHub Actions
---
## 1.1 Описание проекта CI/CD Pipeline с использованием GitHub Actions

### CI/CD pipeline — это автоматизированная система непрерывной интеграции и доставки, которая позволяет:
#### 1. Автоматически тестировать код
#### 2. Собирать проект
#### 3. Деплоить изменения в продакшен

### Цели pipeline
#### - Обеспечение качества кода через автоматизированное тестирование
#### - Автоматизация процесса сборки проекта
#### - Безопасное развертывание в продакшен-среду
#### - Минимизация человеческого фактора при деплое

### Описание целей и этапов pipeline.
#### 1. Таблица с описанием каждого шага и триггеров.
#### 2. Пример конфигурационного файла workflow в блоке кода.
#### 3. Инструкции по добавлению секретов и активации.
#### 4. Обязательные элементы Markdown: 1. Заголовки, таблицы, блок кода, нумерованные и маркированные списки, ссылки, выделение текста, горизонтальные линии.


## 1.2 Описание целей и этапов pipeline.
---
#### - Клонирование репозитория
#### - Установка зависимостей
#### - Запуск тестов
#### - Сборка проекта
#### - Деплой на сервер

## 1.3 Таблица шагов и триггеров
---
|      Шаг       |	        Описание        |            Триггеры           | 
|:---------------|:------------------------:|------------------------------:|
|     Checkout   |	Клонирование репозитория|	Push в ветку main Dependencies|	
|   Dependencies | Установка зависимостей   |      Успешное клонирование    |
|      Tests     |        Запуск тестов     |Успешная установка зависимостей|
|      Build     |       Сборка проекта     | Успешное прохождение тестов   |
|      Deploy    |      Деплой на сервер    |       Успешная сборка         |


# 2 Ход работы 
---
## 2.1 Пример блока кода для конфигурационного файла
```name: CI-CD Pipeline on: push: branches: - main jobs: build-test: runs-on: ubuntu-latest steps: - name: Checkout code uses: actions/checkout@v3 - name: Install dependencies run: npm install - name: Run tests run: npm test - name: Build project run: npm run build deploy: needs: build-test runs-on: ubuntu-latest if: ${{ success() }} steps: - name: Checkout code uses: actions/checkout@v3 - name: Deploy via SSH uses: appleboy/ssh-action@master with: host: ${{ secrets.SERVER_HOST }} username: ${{ secrets.SERVER_USER }} key: ${{ secrets.SERVER_SSH_KEY }} script: | rsync -avz --delete ./build/ ${{ secrets.SERVER_USER }}@${{ secrets.SERVER_HOST }}:/var/www/html
```
## 2.3 Инструкции по настройке
#### - Добавление секретов
#### - Перейдите в настройки репозитория: Settings → Secrets and variables → Actions

#### - Добавьте следующие секреты:
#### 1. SERVER_HOST — IP-адрес сервера
#### 2. SERVER_USER — имя пользователя на сервере
#### 3. SERVER_SSH_KEY — приватный SSH-ключ
#### - Активация pipeline
#### 1.Создайте директорию .github/workflows в корне проекта
#### 2.Добавьте файл ci.yml с конфигурацией
#### 3.Сделайте коммит и пуш изменений в ветку main
