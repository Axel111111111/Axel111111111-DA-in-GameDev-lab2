# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2
"СБОР, ОБРАБОТКА И ВИЗУАЛИЗАЦИЯ ТЕСТОВОГО НАБОРА ДАННЫХ."
Выполнил:
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

![image](https://user-images.githubusercontent.com/49406824/195111259-487d97e1-c16c-4437-aabe-be7ca114c4ff.png)
![image](https://user-images.githubusercontent.com/49406824/195111408-cce75cca-ca4d-42ee-b7fd-53bcba01b98a.png)

Обучим итеративно модель 10 раз в шагом в 100 (100 итераций, 200 итераций, .... 1000 итераций). Посчитаем разницу в ошибок на каждой из 10 итераций (модуль разницы текущей loss и предыдущей loss). Загрузим значения в Google Spreadsheets.

![2 1](https://user-images.githubusercontent.com/49406824/195108931-2ed0b3d2-a582-4fe0-aada-696cecff8583.png)

## Задание 3
### Самостоятельно разработать сценарий воспроизведения звукового сопровождения в Unity в зависимости от изменения считанных данных в задании 2

Bзменил код скрипта, отвечающий за воспроизведение звуков, в зависимости от данных.

       void Update()
    {
        if (dataSet.Count == 0 || dataSet.Count <= i){
            return;
        }
        
        if (dataSet["Mon_" + i.ToString()] <= 200 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 200 & dataSet["Mon_" + i.ToString()] < 1000 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
        
        if (dataSet["Mon_" + i.ToString()] >= 1000 & statusStart == false & 1 != dataSet.Count){
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1uGsokpbfe9YWQUjgl2YnhgCUdCIHzmnJGHz_dhqLyhQ/values/Лист1?key=AIzaSyAX8ysCLFqIPEyPKk8-n1CFU4pRrW5gpiQ");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[1]));  // selectRow[2] -> selectRow[1]
        }
    }

![3](https://user-images.githubusercontent.com/49406824/195114145-d64ac4e7-7e70-4548-a134-fe11998a6ac4.png)

## Выводы

В ходе данной лабораторной работы я научился создавать скрипты в Unity, для воспроизведения звуков в соответствии с генерирующимися данными. Повторил действия, 
которые были  показаны в видео и разобрался как это работает. Научился связывать PyCharm с Google Sheets через Google Console и связывать Google Sheets с Unity.
Реализовал запись в Google-таблицу  данных из лабораторной работы № 1 и разработал сценарий воспроизведения звуковогосопровождения в Unity в зависимости от 
изменения считанных данных в задании 2.

**BigDigital Team: Denisov | Fadeev | Panov**
