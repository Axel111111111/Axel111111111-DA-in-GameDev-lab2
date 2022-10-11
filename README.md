# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
- Зубов Алексей Иванович
- РИ-210946

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
Знакомства с программными средствами для организции передачи данных между Google Cloud, Sheets, Python и Unity.

## Задание 1
### Реализовать совместную работу и передачу данных в связке Python - Google-Sheets – Unity.

Ход работы:

Шаг 1. В консоли Google Cloud подключил необходимые API сервисов (Drive & Sheets), создал сервисный аккаунт, создал API-ключ, выгрузил ключ сервисного аккаунта

![1 1](https://user-images.githubusercontent.com/49406824/195107626-71749b27-5ad2-4fff-87e4-1b6ff9057203.png)

Шаг 2. еализовать запись данных из скрипта на Python в Google Spreadsheets. Данные описывают изменение темпа инфляции на протяжении 11 отсчётных периодов, с учётом стоимости игрового объекта в каждый период.

![1 2](https://user-images.githubusercontent.com/49406824/195107649-cd2a1887-37da-43b4-b13b-772a6bd153de.png)

Шаг 3-4. Создать новый проект на Unity, который будет получать данные из Google Spreadsheets, в которую были записаны данные в предыдущем пункте. Написать функционал на Unity, в котором будет воспризводиться аудиофайл в зависимости от значения данных из таблицы.

![1 3](https://user-images.githubusercontent.com/49406824/195107694-c961b517-e0ec-4604-a125-28a898e64862.png)

## Задание 2
### Реализовать запись в Google-таблицу набора данных, полученных с помощью линейной регрессии из лабораторной работы № 1

Код реализации собственной загрузки данных с модели на Google Spreadsheets:

import gspread

import numpy as np


x = [3, 21, 22, 34, 54, 34, 55, 67, 89, 99]

x = np.array(x)

y = [2, 22, 24, 65, 79, 82, 55, 130, 150, 199]

y = np.array(y)


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


a = np.random.rand(1)

b = np.random.rand(1)


Lr = 0.000001


gc = gspread.service_account(filename='sonic-aria-344013-3a80acbbd350.json')

sh = gc.open("RegressionSheets")


old_loss = 0


for i in range(10):

    a, b = iterate(a, b, x, y, 100 * (i + 1))
    

    prediction = model(a, b, x)
    
    loss = loss_function(a, b, x, y)
    

    diff_loss = abs(loss - old_loss)
    
    old_loss = loss
    

    sh.sheet1.update(('A' + str(i + 1)), str(i + 1))
    
    sh.sheet1.update(('B' + str(i + 1)), str(loss))
    
    sh.sheet1.update(('C' + str(i + 1)), str(diff_loss))
    

Обучим итеративно модель 10 раз в шагом в 100 (100 итераций, 200 итераций, .... 1000 итераций). Посчитаем разницу в ошибок на каждой из 10 итераций (модуль разницы текущей loss и предыдущей loss). Загрузим значения в Google Spreadsheets.

![2 1](https://user-images.githubusercontent.com/49406824/195108931-2ed0b3d2-a582-4fe0-aada-696cecff8583.png)

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2
Изменяем диапазон данных в коде, чтобы одни подходили под диапазон loss.

Изменяем количество колонок в соответствии с новыми данными.

using System.Collections; using System.Collections.Generic; using UnityEngine; using UnityEngine.Networking; using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour { public AudioClip goodSpeak; public AudioClip normalSpeak; public AudioClip badSpeak; private AudioSource selectAudio; private Dictionary<string,float> dataSet = new Dictionary<string, float>(); private bool statusStart = false; private int i = 1;

## Выводы

Изучил и умею в Юнити создавать скрипты, для воспроизведения звуков в соответствии с генерирующимися данными. Повторил то, что было в безмолвном видео, но понял как это работает. Повторить по видео опять смогу, но и сам научился и что делаю знаю. Обучение по видео я воспринимаю отлично, и без звука лучше. Данный уровень программирования понимаю. Если есть непонимание, то либо что конкретно изучать в связи со своим непониманием знаю, либо разбираюсь сам, пока не пойму, сколько бы это времени не заняло. Смогу интегрировать в дальнейшем данный код и если его потеряю, то всегда смогу обратиться к материалам курса. Основы изучаю сам.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**
