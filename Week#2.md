# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #2 выполнил(а):
- Ежова Алисса Дмитриевна
- РИ-230931
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * |  |
| Задание 2 | * |  |
| Задание 3 | * |  |
| Задание 4 | * |  |
| Задание 5.1 | * |  |
| Задание 5.2 | # |  |
| Задание 5.3 | # |  |

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
- Ход реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Ход реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Ход реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться передавать в Unity данные из Google Sheets с помощью Python.


## Задание 1
### Настройка доступ к google-таблице по api
Ход работы:
- Создание нового проекта в Google Cloud Console
- Переход в панель управления созданного проекта
- Переход в Google Drive API, используя текстовый поиск
- Подключение API
- Переход в раздел «Учетные данные» и создание учетных данных для ключа API
- Сохранение файла с ключом в формате JSON на своем компьютере
- Создать Google-таблицу и доступ к ней для редактирования с созданного сервисного аккаунта
![image](https://github.com/user-attachments/assets/d8c1cb75-98c7-442e-b02b-6c7fdeab84d5)
![image](https://github.com/user-attachments/assets/85ee6392-0b0d-4794-ac61-336ff6cf6d33)

## Задание 2
###  Генерация данных с помощью Python и их передача в google таблицу

Ход работы:
- Запуск Jupyter Notebook, создание нового .ipynb-файл. Созданный файл и скачанный ранее .json-файл с ключами оказались в одной директории
- Реализация передачи данных в созданную ранее google-таблицу в .ipynb файле на языке Python. Реализация на Python рассчитывает темп инфляции для экономической модели
```py
import gspread
import numpy as np
gc = gspread.service_account(filename='unitydatascience-400014-da50a8398902.json')
sh = gc.open("UnityWorkshop2")
price = np.random.randint(2000, 10000, 11)
mon = list(range(1,11))
i = 0
while i <= len(mon):
    i += 1
    if i == 0:
        continue
    else:
        tempInf = ((price[i-1]-price[i-2])/price[i-2])*100
        tempInf = str(tempInf)
        tempInf = tempInf.replace('.',',')
        sh.sheet1.update(('A' + str(i)), str(i))
        sh.sheet1.update(('B' + str(i)), str(price[i-1]))
        sh.sheet1.update(('C' + str(i)), str(tempInf))
        print(tempInf)
```
- Запуск .ipynb-файл.
![image](https://github.com/user-attachments/assets/e62f70df-6825-4857-831b-b9456e52d80c)
![image](https://github.com/user-attachments/assets/890435d0-2c36-4438-b28a-22aaed1672e5)

  
## Задание 3
### Создание ключа API для передачи данных из google-таблицы в Unity

- Создание API key для получения данных из таблицы и обработки их в Unity

## Задание 4
### Новый проект на Unity
- Создание нового 3D проект на Unity
- Добавление в проект файлы jsonPackage и soundPackage
- Создание на сцене Unity пустой GameObject и подключение к нему .cs скрипт, в котором описывается подключение к google-таблице по API и воспроизведение звуков в игре в соответствии со считываемыми значениями
```py
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using SimpleJSON;

public class NewBehaviourScript : MonoBehaviour
{
    public AudioClip goodSpeak;
    public AudioClip normalSpeak;
    public AudioClip badSpeak;
    private AudioSource selectAudio;
    private Dictionary<string,float> dataSet = new Dictionary<string, float>();
    private bool statusStart = false;
    private int i = 1;

    // Start is called before the first frame update
    void Start()
    {
        StartCoroutine(GoogleSheets());
    }

    // Update is called once per frame
    void Update()
    {
        if (dataSet["Mon_" + i.ToString()] <= 10 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioGood());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] > 10 & dataSet["Mon_" + i.ToString()] < 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioNormal());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }

        if (dataSet["Mon_" + i.ToString()] >= 100 & statusStart == false & i != dataSet.Count)
        {
            StartCoroutine(PlaySelectAudioBad());
            Debug.Log(dataSet["Mon_" + i.ToString()]);
        }
    }

    IEnumerator GoogleSheets()
    {
        UnityWebRequest curentResp = UnityWebRequest.Get("https://sheets.googleapis.com/v4/spreadsheets/1CEjtCsGKu_fHyNkAAnhjj7Op5AObTqc0w-XotEF8bTI/values/Лист1?key=AIzaSyASnu9QXtSW-cHLv__q0mYZNbbhJUFUgdM");
        yield return curentResp.SendWebRequest();
        string rawResp = curentResp.downloadHandler.text;
        var rawJson = JSON.Parse(rawResp);
        foreach (var itemRawJson in rawJson["values"])
        {
            var parseJson = JSON.Parse(itemRawJson.ToString());
            var selectRow = parseJson[0].AsStringList;
            dataSet.Add(("Mon_" + selectRow[0]), float.Parse(selectRow[2]));
        }
    }

    IEnumerator PlaySelectAudioGood()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = goodSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioNormal()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = normalSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(3);
        statusStart = false;
        i++;
    }
    IEnumerator PlaySelectAudioBad()
    {
        statusStart = true;
        selectAudio = GetComponent<AudioSource>();
        selectAudio.clip = badSpeak;
        selectAudio.Play();
        yield return new WaitForSeconds(4);
        statusStart = false;
        i++;
    }
}
```
- Настройка инспектора свойства объекта GameObject
- Запуск сцены. 
![image](https://github.com/user-attachments/assets/3545d345-ac10-4ec7-9de9-b962514aeb9b)
![image](https://github.com/user-attachments/assets/5f89a369-a3d1-4083-89a6-1eedda72fcc8)
![image](https://github.com/user-attachments/assets/c7eaaa90-582a-4457-aed0-15a60d3c0311)

## Задание 5.1
###  Переменная "HP" в игре "СПАСТИ РТФ: Выживание"

Роль: Переменная "НР" (Health Points - очки здоровья) в игре про зомби играет ключевую роль, определяя выживание игрока. Она представляет собой количество здоровья, которое может выдержать игрок, прежде чем погибнуть.

Условия изменения/появления:
- Начальное значение: В начале игры игрок имеет 30 НР.
- Потеря НР: Игрок теряет ХП от атак зомби. Количество теряемого НР зависит от уровня зомби и его атаки.
- Увеличение НР:
    - Повышение уровня: При повышении уровня игрока доступна модификация, увеличивающая максимальное НР на 10 единиц.
    - Восстановление НР: При повышении уровня доступна модификация, восстанавливающая НР во время сражения. 
    - Предметы: В игре могут быть предметы, которые временно восстанавливают НР игрока.

Диапазон допустимых значений:
- Минимальное: 0 (игрок погибает).
- Максимальное: Определяется начальным значением НР (30) + количество выбранных модификаций, увеличивающих максимальное НР (10 единиц за каждую модификацию).

Экономическая модель игры:

Ресурсы:
   НР: Начальное значение 30, может увеличиваться на 10 с каждой модификацией.
     - Модификации: Увеличение максимального нр, восстановление нр в бою.
   Золото: Получается за убийство зомби и в конце каждой волны.
     - Расходы: На новое оружие и патроны.
   Опыт: Набирается за убийство зомби.
     - Уровень: Когда опыт достигает определённого порога, игрок повышает уровень и выбирает модификации.
Зомби:
    Урон: Увеличивается на каждом уровне (начальный урон 10 хп за удар).
    Количество: Увеличивается с каждым уровнем.
    Типы зомби: Обычные зомби и Боссы.
Боевые волны:
    Каждая волна представляет собой уровень с определённым числом зомби.
    После каждой волны игрок получает возможность покупки новых ресурсов (оружие, патроны) за золото.

Игровой процесс:
    Игрок сражается с зомби, получая урон и зарабатывая опыт и золото.
    Если нр достигает 0, игра заканчивается, и начинается заново.

Действия:
 Получение опыиа: Игрок получает опыт и золото, убивая враждебных существ.
 Повышение уровня: Игрок тратит опыт, чтобы повысить свой уровень.
 Покупка модификаций: Игрок тратит золото, чтобы купить модификации, которые повышают его характеристики, в том числе НР.
 Использование предметов: Игрок использует найденные или купленные предметы, чтобы получить преимущество в бою, например, восстановить НР.

Место НР в экономической модели: НР не является ресурсом, который можно приобрести или потратить. Оно влияет на экономическую модель игры, так как определяет, как долго игрок может оставаться в игре и получать ресурсы. Чем больше НР у игрока, тем больше времени у него есть для выполнения задач и получения золота, тем больше опыта он может получить и тем больше модификаций он может купить. 

## Выводы

В ходе выполнения лабораторной работы мы научились передавать данные из Google Sheets в Unity с использованием Python и настроенного API. Реализованы генерация данных на Python, их передача в Google Sheets, а также использование этих данных в игровом движке Unity для передачи звука в соответствии с экономической моделью динамики.

## Powered by Ежова.А.Д

**Dokira**
