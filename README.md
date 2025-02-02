# «Введение в Terraform»  - Абрамов Сергей

### Цели задания

1. Установить и настроить Terrafrom.
2. Научиться использовать готовый код.

------

### Чек-лист готовности к домашнему заданию

1. Скачайте и установите **Terraform** версии ~>1.8.4 . Приложите скриншот вывода команды ```terraform --version```.
2. Скачайте на свой ПК этот git-репозиторий. Исходный код для выполнения задания расположен в директории **01/src**.
3. Убедитесь, что в вашей ОС установлен docker.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. Репозиторий с ссылкой на зеркало для установки и настройки Terraform: [ссылка](https://github.com/netology-code/devops-materials).
2. Установка docker: [ссылка](https://docs.docker.com/engine/install/ubuntu/). 
------
### Внимание!! Обязательно предоставляем на проверку получившийся код в виде ссылки на ваш github-репозиторий!
------

![ter1](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter1.jpg)

### Задание 1

1. Перейдите в каталог [**src**](https://github.com/netology-code/ter-homeworks/tree/main/01/src). Скачайте все необходимые зависимости, использованные в проекте. 

### Решение

![ter2](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter2.jpg)

![ter3](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter3.jpg)


2. Изучите файл **.gitignore**. В каком terraform-файле, согласно этому .gitignore, допустимо сохранить личную, секретную информацию?(логины,пароли,ключи,
токены итд)

### Решение

В данном файле указывается , что информация хранится в personal.auto.tfvars

![ter4](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter4.jpg)


3. Выполните код проекта. Найдите  в state-файле секретное содержимое созданного ресурса **random_password**, пришлите в качестве ответа конкретный ключ и его значение.

### Решение

![ter5](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter5.jpg)

На скриншоте выделен ключ и его значение.

![ter6](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter6.jpg)


4. Раскомментируйте блок кода, примерно расположенный на строчках 29–42 файла **main.tf**.
Выполните команду ```terraform validate```. Объясните, в чём заключаются намеренно допущенные ошибки. Исправьте их.

### Решение

Ошибки

![ter7](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter7.jpg)

Исправлены ошибки

![ter8](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter8.jpg)


5. Выполните код. В качестве ответа приложите: исправленный фрагмент кода и вывод команды ```docker ps```.

### Решение

```
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"

  ports {
    internal = 80
    external = 9090
  }
}

```

![ter9](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter9.jpg)

6. Замените имя docker-контейнера в блоке кода на ```hello_world```. Не перепутайте имя контейнера и имя образа. Мы всё ещё продолжаем использовать name = "nginx:latest". Выполните команду ```terraform apply -auto-approve```.
Объясните своими словами, в чём может быть опасность применения ключа  ```-auto-approve```. Догадайтесь или нагуглите зачем может пригодиться данный ключ? В качестве ответа дополнительно приложите вывод команды ```docker ps```.

### Решение

Заменил имя контенера в блоке кода 

```
resource "docker_container" "nginx" {
  image = "nginx:latest"
  name  = "hello_world"

```
```
terraform plan

```
![ter10](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter10.jpg)

```
terraform apply -auto-approve

```
![ter11](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter11.jpg)

Ключ -auto-approve автоматически подтверждает все изменения без моего подтверждения и предварительного просмотра изменений.


7. Уничтожьте созданные ресурсы с помощью **terraform**. Убедитесь, что все ресурсы удалены. Приложите содержимое файла **terraform.tfstate**.

### Решение

```
terraform destroy -auto-approve

```

![ter12](https://github.com/smabramov/Introduction-to-Terraform/blob/9befb62bfe1ce2306f98d738c03f078c028dbe35/jpeg/ter12.jpg)


8. Объясните, почему при этом не был удалён docker-образ **nginx:latest**. Ответ **ОБЯЗАТЕЛЬНО НАЙДИТЕ В ПРЕДОСТАВЛЕННОМ КОДЕ**, а затем **ОБЯЗАТЕЛЬНО ПОДКРЕПИТЕ** строчкой из документации [**terraform провайдера docker**](https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs).  (ищите в классификаторе resource docker_image )

### Ответ

Docker-образ nginx:latest не был удален, потому что Terraform управляет контейнерами, а не образами. Образы остаются в системе, чтобы их можно было использовать повторно.

https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs/resources/image

keep_locally (логическое значение) Если значение true, то изображение Docker не будет удалено при операции destroy. Если значение false, то изображение будет удалено из локального хранилища docker при операции destroy.

https://docs.comcloud.xyz/providers/kreuzwerker/docker/latest/docs/resources/image

Строчка из документации представлена на скриншоте ниже:

keep_locally (Boolean) If true, then the Docker image won't be deleted on destroy operation. If this is false, it will delete the image from the docker local storage on destroy operation.

