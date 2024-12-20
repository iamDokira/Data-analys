# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #4 выполнил(а):
- Ежова Алиса Дмитриевна
- РИ-230931
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
Реализовать принцип работы перцептрона, визуализировать ход его обучения и результаты работы.

## Задание 1
## в проекте Unity реализовать перцептрон, который умеет производить вычисления:
## - OR | дать комментарии о корректности работы
## - AND | дать комментарии о корректности работы
## - NAND | дать комментарии о корректности работы
## - XOR | дать комментарии о корректности работы

В Unity мы реализовали несколько объектов для каждого оператора. При 8 эпохах обучения операторы OR, AND и NAND работалли корректно тк они линейно разделимы. Оператор XOR не обладает таким свойством как разделимость, поэтому правильных ожидаемых значений мы не получили.

## Задание 2
### Построить графики зависимости количества эпох от ошибки  обучения. Указать от чего зависит необходимое количество эпох обучения
![Chart Title](https://github.com/user-attachments/assets/c823c375-5726-4a59-bfd7-a94a68be7fc1)
Был построен график, где по оси Х отображены эпохи
Для разных операторов требуется разное количесво времени для обучения, достижения стабильно верного результата и построения четкой зависимости

#Задание 3
#Построить визуальную модель работы перцептрона на сцене Unity

Визуальная модель была реализована на сцене Юнити, где цвет объектов меняется в завимимости от исходных входных значений 
## Выводы

Мы реализовали прецептон и выявили зависимость количества эпох от ошибки  обучения

## Powered by Dokira

**BigDigital Team: Denisov | Fadeev | Panov**