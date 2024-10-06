# Домашнее задание к занятию «Сетевое взаимодействие в K8S. Часть 1»

### Цель задания

В тестовой среде Kubernetes необходимо обеспечить доступ к приложению, установленному в предыдущем ДЗ и состоящему из двух контейнеров, по разным портам в разные контейнеры как внутри кластера, так и снаружи.

------

### Чеклист готовности к домашнему заданию

1. Установленное k8s-решение (например, MicroK8S).
2. Установленный локальный kubectl.
3. Редактор YAML-файлов с подключённым Git-репозиторием.

------

### Инструменты и дополнительные материалы, которые пригодятся для выполнения задания

1. [Описание](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/) Deployment и примеры манифестов.
2. [Описание](https://kubernetes.io/docs/concepts/services-networking/service/) Описание Service.
3. [Описание](https://github.com/wbitt/Network-MultiTool) Multitool.

------

### 1. Создать Deployment и обеспечить доступ к контейнерам приложения по разным портам из другого Pod внутри кластера

1. Создаём новый namespace, пишем манифест Deployment приложения, состоящего из двух контейнеров (nginx и multitool), с количеством реплик 3 шт.

Применяем манифест и проверяем результат:

![1](https://github.com/user-attachments/assets/f2f719d4-a676-4c98-8b2e-e9da0eb4e67c)

Deployment создан, количество реплик равно трём.

2. Пишем манифест Service, который обеспечит доступ внутри кластера до контейнеров приложения из п.1 по порту 9001 — nginx 80, по 9002 — multitool 8080.

Применем манифест и проверяем результат:

![2](https://github.com/user-attachments/assets/32a4b898-ac87-43b6-a152-cd64d8042ecb)

Сервис создан и слушает порты 9001 и 9002.

3. Пишем манифест отдельного Pod, с приложением multitool и запускаем его:

![3](https://github.com/user-attachments/assets/cb146821-f439-4b38-8097-ea1aa6df1e1a)

Из пода `multitool` проверяем доступность приложений в подах по одному из IP адресов:

![3-1](https://github.com/user-attachments/assets/0f2bcc22-d95c-4379-bb91-186a9d7ed96b)

Приложения в подах доступны.

4. С помощью `curl` проверяем доступность подов по доменному имени сервиса:

![4](https://github.com/user-attachments/assets/e1726cef-62f4-4668-806c-ce240e088321)

Nginx отвечает через сервис по порту 9001, multitool отвечает по порту 9002.

5. Ссылки на манифесты:
   
   [Манифест Deployment](https://github.com/PatKolzin/kuber-1.4/blob/main/src/deployment.yml)

   [Манифест Service](https://github.com/PatKolzin/kuber-1.4/blob/main/src/service.yml)

   [Манифест Pod](https://github.com/PatKolzin/kuber-1.4/blob/main/src/pod.yml)

### 2. Создать Service и обеспечить доступ к приложениям снаружи кластера

1. Создаём отдельный Service приложения из Задания 1 с возможностью доступа снаружи кластера к nginx, используя тип NodePort:

![1nodeport](https://github.com/user-attachments/assets/1f3565ef-64fe-42f8-a97e-ef4eb0d167aa)

Сервис с именем nodeport-service и внешними портами 30001 и 30002 создан.

2. С помощью `curl` проверяем, доступны ли приложения из подов по внешним портам:

![1-1nodeport](https://github.com/user-attachments/assets/7d87f5a1-0095-40fd-b5e3-5c851fa8f228)

Приложения доступны по локальному IP ноды, на порту 30001 отвечает nginx, на порту 30002 отвечает multitool.

3. Ссылка на [Манифест Service-NodePort](https://github.com/PatKolzin/kuber-1.4/blob/main/src/service-nodeport.yml)
