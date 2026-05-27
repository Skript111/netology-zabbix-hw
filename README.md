# Домашнее задание к занятию "`Домашнее задание к занятию «GitLab`" - `Шичков Евгений`


### Инструкция по выполнению домашнего задания

   1. Сделайте `fork` данного репозитория к себе в Github и переименуйте его по названию или номеру занятия, например, https://github.com/имя-вашего-репозитория/git-hw или  https://github.com/имя-вашего-репозитория/7-1-ansible-hw).
   2. Выполните клонирование данного репозитория к себе на ПК с помощью команды `git clone`.
   3. Выполните домашнее задание и заполните у себя локально этот файл README.md:
      - впишите вверху название занятия и вашу фамилию и имя
      - в каждом задании добавьте решение в требуемом виде (текст/код/скриншоты/ссылка)
      - для корректного добавления скриншотов воспользуйтесь [инструкцией "Как вставить скриншот в шаблон с решением](https://github.com/netology-code/sys-pattern-homework/blob/main/screen-instruction.md)
      - при оформлении используйте возможности языка разметки md (коротко об этом можно посмотреть в [инструкции  по MarkDown](https://github.com/netology-code/sys-pattern-homework/blob/main/md-instruction.md))
   4. После завершения работы над домашним заданием сделайте коммит (`git commit -m "comment"`) и отправьте его на Github (`git push origin`);
   5. В личном кабинете прикрепите и отправьте ссылку на решение в виде md-файла в вашем Github.
   6. Любые вопросы по выполнению заданий спрашивайте в разделе “Вопросы по заданию” в личном кабинете.
   
Желаем успехов в выполнении домашнего задания!
   
### Дополнительные материалы, которые могут быть полезны для выполнения задания

1. [Руководство по оформлению Markdown файлов](https://gist.github.com/Jekins/2bf2d0638163f1294637#Code)

---

### Задание 1

`Приведите ответ в свободной форме........`

1. `Разверните GitLab локально, используя Vagrantfile и инструкцию, описанные в этом репозитории.`
**Развёртывание GitLab**  
   - Склонирован репозиторий `https://github.com/netology-code/sdvps-materials.git`
   - В папке `gitlab` выполнен `vagrant up` с переменной `VAGRANT_EXPERIMENTAL="disks"`
   - В файл `C:\Windows\System32\drivers\etc\hosts` добавлена запись:  
     `192.168.56.10 gitlab.localdomain gitlab`
   - После успешного запуска GitLab доступен по адресу `http://192.168.56.10`
2. `Создайте новый проект и пустой репозиторий в нём.`
**Создание проекта и пустого репозитория**  
   - В веб-интерфейсе GitLab создан проект `my-project` (тип `Blank project`, без инициализации README).
3. `Зарегистрируйте gitlab-runner для этого проекта и запустите его в режиме Docker. Раннер можно регистрировать и запускать на той же виртуальной машине, на которой запущен GitLab.`
**Регистрация и запуск gitlab-runner**  
   - Внутри виртуальной машины (`vagrant ssh`) выполнен запуск регистрации раннера:
     
```bash
sudo docker run -ti --rm --name gitlab-runner-register \
  --network host \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest register
```

   URL: http://192.168.56.10
Регистрационный токен: взят из настроек проекта (Settings → CI/CD → Runners → New project runner)
Executor: docker
Образ по умолчанию: alpine:latest
Раннер запущен командой:
```bash
sudo docker run -d --name gitlab-runner --restart always \
  --network host \
  -v /srv/gitlab-runner/config:/etc/gitlab-runner \
  -v /var/run/docker.sock:/var/run/docker.sock \
  gitlab/gitlab-runner:latest
```
В разделе Settings → CI/CD → Runners проекта появился раннер со статусом online (зелёный индикатор).

`При необходимости прикрепитe сюда скриншоты
![fork-репозитория](zad1/fork.png)
![git clone](zad1/git_clone.png)
![Запуск GitLab](zad1/gitlab-ip.png)
![Пароль GitLab](zad1/gitlab-password.png)
![Runner запущен](zad1/runner-started.png)
---

### Задание 2

`Приведите ответ в свободной форме........`

1. `Запушьте репозиторий на GitLab, изменив origin. Это изучалось на занятии по Git.`
**Выполнение:**
```
git remote set-url origin http://192.168.56.10/root/my-project.git
```
#Замена Origin , после проверяем:
```
git remote -v
```
Результат:
![Origin](zad2/origin.png)

Запушил на GitLab
Результат:
![push](zad2/push.png)

2. `Создайте .gitlab-ci.yml, описав в нём все необходимые, на ваш взгляд, этапы.`
**Выполнение:**
В корне репозитория создан файл `.gitlab-ci.yml`.
Файл добавлен в Git и запушен в GitLab.
После пуша GitLab автоматически запустил pipeline.
Оба этапа (`test` и `build`) завершились успешно.

**Содержимое `.gitlab-ci.yml`:**
```yaml
stages:
  - test
  - build

test_job:
  stage: test
  script:
    - echo "Running tests..."
    - echo "All tests passed!"

build_job:
  stage: build
  script:
    - echo "Building application..."
    - echo "Build complete!"
  artifacts:
    paths:
      - build/
```
Скриншот Pipeline
![Pipline](zad2/Pipline.png)
![Origin](zad2/push.png)

---
