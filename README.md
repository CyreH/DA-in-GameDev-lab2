# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Гусамов Артур Филаритович
- РИ210950
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Познакомиться с программными средствами для организции передачи данных между инструментами google, Python и Unity

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity.
Ход работы:
- В облачном сервисе google console подключить API для работы с google sheets и google drive.
1. Создать проект на console.google.com

![creatGC](https://user-images.githubusercontent.com/102403656/195144245-195b2a37-4f19-4a32-8d22-1b98fce6c6aa.png)

2. Подключить нужные API
3. Создать сервисный аккаунт

![image](https://user-images.githubusercontent.com/102403656/195144716-3232d2a8-ac21-4071-b617-23481ac58c73.png)

4. Создать ключ и сохранить его в python проэкт![image](https://user-images.githubusercontent.com/102403656/195145832-d14e44b9-4956-4ae9-99a9-01f62ab1b289.png)
5. Создать google таблицу и предоставить доступ сервиснуму аккаунту, созданного ранее![image](https://user-images.githubusercontent.com/102403656/195146307-3e868e90-8cea-4d00-94dd-c582690753ff.png)
6. Подключить библеотеки "gspread" и "numpy" и написать скрипт на Python

```py

import gspread
import numpy as np

gc = gspread.service_account(filename='unitydatasciense-365216-c7a229f4132b.json')
sh = gc.open('UnitySheets')
price = np.random.randint(2000, 10000, 11)
mon = list(range(1, 11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = ((price[i - 1] - price[i - 2]) / price[i - 2]) * 100
        tempInf = str(tempInf).replace('.', ',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i - 1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))

```



## Задание 2
### Пошагово выполнить каждый пункт раздела "ход работы" с описанием и примерами реализации задач
Ход работы:
- Произвести подготовку данных для линейной регрессии. Добавить библеотеки numpy и matplotlib для вычислений и отрисовки соответсвтенно. Добавить списки из случайных данных.

```py

import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline

x = [3,21,22,34,54,34,55,67,89,99]
x = np.array(x)
y = [2,22,24,65,79,82,55,130,150,199]
y = np.array(y)

plt.scatter(x,y)

```
![image](https://user-images.githubusercontent.com/102403656/191739931-8e749766-4a62-4d46-a6f8-97ffa67352b5.png)
- Определить связанные функции. Функция model: определяет модель линейной регрессии(wx+b). Функция loss_function: функция потерь среднеквадратичной ошибки. Функция optimize: метод градиентного спуска для нахождения частных производных w и b. Функция iterate: итерирует фунцию.

```py

def model(a, b, x):
    return a * x + b


def loss_function(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    return (0.5 / num) * (np.square(prediction - y)).sum()


def optimize(a, b, x, y):
    num = len(x)
    prediction = model(a, b, x)
    da = (1.0 / num) * ((prediction - y) * x).sum()
    db = (1.0 / num) * ((prediction - y).sum())
    a = a - Lr * da
    b = b - Lr * db
    return a, b


def iterate(a, b, x, y, times):
    for i in range(times):
        a, b = optimize(a, b, x, y)
    return a, b
    
```
- Начать итерацию
1. Инициализация и модель итеративной оптимизации

```py

a = np.random.rand(1)
print(a)
b = np.random.rand(1)
print(b)
Lr = 0.000001

a, b = iterate(a, b, x, y, 1)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)

```
![image](https://user-images.githubusercontent.com/102403656/191745965-fd21bd14-488e-46be-8589-bbb00f60da5d.png)

2. На второй итерации отображаются значения параметров, значения потерь и эффекты визуальзации после итерации

```py

a, b = iterate(a, b, x, y, 2)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)

```
![image](https://user-images.githubusercontent.com/102403656/191746524-947ca9f5-089b-42a1-8afd-46de2359ac59.png)

3. Третья итерация показывает значение параметров, значения потерть и визуализацию после итерации

```py

a, b = iterate(a, b, x, y, 3)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)

```
![image](https://user-images.githubusercontent.com/102403656/191747123-3f8838ce-f330-4ec9-be0e-21c9d160890b.png)

4. На четвертой итерации отображаются значения параметров, значения потерь и эффектов визуализации

```py

a, b = iterate(a, b, x, y, 4)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)

```
![image](https://user-images.githubusercontent.com/102403656/191747402-61263d18-80d1-4b73-a3f1-21ece8b38edb.png)

5. Пятая итерация показывает значение параметра, значение потерь и эффект визуализации после итерации

```py

a, b = iterate(a, b, x, y, 5)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)

```
![image](https://user-images.githubusercontent.com/102403656/191747792-47f49902-660c-499f-a0fc-c9994e0d71a9.png)

6. 10000-я итерация, показывающая значения параметров, потери и визуализацию после итерации

```py

a, b = iterate(a, b, x, y, 10000)
prediction = model(a, b, x)
loss = loss_function(a, b, x, y)
print(a, b, loss)
plt.scatter(x, y)
plt.plot(x, prediction)

```
![image](https://user-images.githubusercontent.com/102403656/191748095-c0f01d87-c24d-46dc-b952-daa9fbc4f9b5.png)



## Задание 3
### Должна ли величина loss стремиться к нулю при изменении исходных данных? Какова роль параметра Lr?
1. При увеличении количества итераций loss становится всё меньше(вне зависимости от исходных x,y)

![image](https://user-images.githubusercontent.com/102403656/192587403-22e049a0-2eab-45e6-8c40-dc99326cdcf6.png)
![image](https://user-images.githubusercontent.com/102403656/192587457-3f217e7e-6aad-4b9d-91c9-5c784a12e1da.png)
![image](https://user-images.githubusercontent.com/102403656/192587515-e4c67991-8d08-47ce-9edc-574754f30736.png)

- изменим значение x,y

![image](https://user-images.githubusercontent.com/102403656/192588676-960a5372-25fe-4d18-b314-6d9340d078b0.png)
![image](https://user-images.githubusercontent.com/102403656/192589383-d4d1bf5c-cbc4-4169-aa45-d0e9a7dc2ffa.png)
![image](https://user-images.githubusercontent.com/102403656/192589446-8a382e5a-70c2-4994-9bd6-2cd8a3b9ea13.png)
![image](https://user-images.githubusercontent.com/102403656/192589915-b935e6af-65d4-4e4d-8bbb-ea21498a7dc6.png)

2. Чем больше значение Lr, тем больше становятся значения x, y, так и a, b. Так же появилась вероятность того, что регрессия становится отрицательной

![image](https://user-images.githubusercontent.com/102403656/192594484-5a8ebf01-289d-46db-b3f7-33d94972b5c8.png)
![image](https://user-images.githubusercontent.com/102403656/192595153-cd29aeed-48e7-46e9-bcaf-939e8842f70b.png)
![image](https://user-images.githubusercontent.com/102403656/192595176-ef541bd6-9cd1-486c-ab3c-22a0093b13fa.png)





## Выводы

При выполнении лабораторной работы я освоил как:
- пользоваться Google.colab и Jupyter notebook. 
- открывать и писать базовые программы на Python и Unity.
- пользоваться github

| Plugin | README |
| ------ | ------ |
| Dropbox | [plugins/dropbox/README.md][PlDb] |
| GitHub | [plugins/github/README.md][PlGh] |
| Google Drive | [plugins/googledrive/README.md][PlGd] |
| OneDrive | [plugins/onedrive/README.md][PlOd] |
| Medium | [plugins/medium/README.md][PlMe] |
| Google Analytics | [plugins/googleanalytics/README.md][PlGa] |

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
