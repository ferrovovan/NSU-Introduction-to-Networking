# Задание 2

## 2.1. Подключение к удаленному серверу через ssh БЕЗ ПАРОЛЯ
1) Создаём связку ключей
```shell
ssh-keygen -t rsa -b 4096 -C "комментарий"
```
Будем считать дальше, что назвали ключ `id_rsa` (соответсвенно имя нужно ввести `/home/$USER/.ssh/id_rsa`).

2) Подключаемся к серверу через пароль
```shell
ssh  $USER@$ADDRESS
```
Вводим пароль.
Настраиваем окружение
```shell
mkdir .ssh
```

3) Оставляем открытый ключ
Возвращаемся в клиент (`exit`) и передаём открытый ключ
```shell
scp ~/.ssh/id_rsa.pub $USER@$ADDRESS:~/.ssh/
```
Снова подключаемся к серверу и добавляем открытый в подтверждённые ключи.
```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```
4) Подключение
```
ssh  $USER@$ADDRESS -i ~/.ssh/id_rsa
```
(`-i` это файл индефикации)

## 2.2. ПУШ В удаленный репозиторий через ssh БЕЗ ПАРОЛЯ
1) Создаём связку ключей
```
 ssh-keygen -t rsa -b 4096 -C "комментарий"
```
Назовём `git-key_rsa`.
2) Заходим на страницу своего пользователя и добавляем публичный ключ
https://github.com/settings/keys
`cat ~/.ssh/git-key.rsa`
3) Добавление в ssh агент:
```
  eval "$(ssh-agent -s)"
  ssh-add ~/.ssh/git-key_rsa
```
4) Теперь можно склонировать репозиторий
```
git clone git@github.com:$USER/$REPO_NAME.git
```
5) Нужно установить авторство
```
git config --global user.name "Ваше Имя"
git config --global user.email "your_email@example.com"
```
6) Теперь можно оставить коммит и отправить изменения.

