# Предположение о характере взаимосвязи факторов и результата.

Воспользуемся датасетом с сайта : **https://huggingface.co/datasets/SIH/palmer-penguins**

Проведем анализ полученных данных и получим датасет, состоящий из столбцов: **bill_length_mm, bill_depth_mm,	flipper_length_mm,	body_mass_g,	sex,	year**, где sex заменим на 1 - female и 0 - male. Также уберем пустые значения, чтобы построить более точную линейную модель.

Для того, чтобы проверить данные на наличие выбросов построим **boxplot** для каждого значения столбца с полом пингвина.

<img width="495" alt="Снимок экрана 2024-05-12 в 15 20 54" src="https://github.com/arlinrus/penguins-/assets/111064731/7c80abac-0c1d-41f6-9c03-a468b66bc1ac">

Убедившись, что для KPA нету противоречий, перехоим к анализу.

## Анализ корреляции

### Проверим данные на нормальность распределения по тесту Шапиро Уилка, если значения отклоняются от 0,5 в меньшую сторону, то надо проверять на нормальность распределения.

Для начала про логорифмируем данные, для более точного и плавного построения графика.

Получим, что данные, имеют нормальное распределения по тесту.

Вычислим коэффициент корреляции Пирсона(линейный коэффициент корреляции)

1)Между переменными 'bill_length_mm' и 'flipper_length_mm' существует сильная положительная корреляция (коэффициент корреляции примерно равен 0.65).

2)Между переменными 'bill_length_mm' и 'body_mass_g' также существует сильная положительная корреляция (коэффициент корреляции примерно равен 0.59).

3)Между переменными 'bill_depth_mm' и 'flipper_length_mm' существует умеренная отрицательная корреляция (коэффициент корреляции примерно равен -0.58).

4)Между переменными 'bill_depth_mm' и 'body_mass_g' также существует умеренная отрицательная корреляция (коэффициент корреляции примерно равен -0.48).


![download](https://github.com/arlinrus/penguins-/assets/111064731/f33fd365-ec39-4450-bbb7-67b243a2c07d)

![download](https://github.com/arlinrus/penguins-/assets/111064731/e134e41e-1e8e-4d04-a625-a9af8deb70b5)

![download](https://github.com/arlinrus/penguins-/assets/111064731/201d65c9-fa96-4308-8b27-d5de2aaa208c)

![download](https://github.com/arlinrus/penguins-/assets/111064731/7a6e85f0-b995-4305-a76c-7a275bc1228a)



### Вычислим коэффициенты b1 и b0 для уравнения линейной регрессии:
Сравним данные длины и шлубины клюа, где глубина - зависимая переменная.  

```
x1 = pgns['bill_length_mm']
y1 = pgns['bill_depth_mm']

mean_x = sum(x1)/len(x1)
mean_y = sum(y1)/len(y1)

covariance = sum((xi-mean_x)*yi-mean_y for xi, yi in zip(x1, y1))
variance = sum((xi - mean_x)**2 for xi in x1)

b1 = covariance / variance
b0 = mean_y - b1*mean_x
```

<dl>
  <dt>Таким образом получим следующие рзультаты:
</dt>
  <dd>b1 - наклон линии:  -0.6580097035844675
    
b0 - интерсепт 46.112549410303366

Уравнение линейной регрессии: y = 46.11 + -0.66*x1</dd>
</dl>

### Рассчитаем объясненную и необъясненную дисперсию
SSR - регрессионный анализ

SSE - ошибка суммы
```
x2 = np.array(pgns['bill_length_mm'])
y2 = np.array(pgns['bill_depth_mm'])

y_pred = b0 + b1*x1

y_mean = np.mean(y2)

#Объясненная дисперсия SSR(регресионный анализ)
SSR = np.sum((y_pred - y_mean)**2)

#Необъясненная дисперсия SSE(ошибка суммы квадратов)
SSE = np.sum((y2 - y_pred)**2)

sum_desp = int(SSR)+ int(SSE)

```
<dl>
  <dt>Получим следующие результаты</dt>
  <dd>Объясненная дисперсия:  4298.98422197287
    
Необъясненная дисперсия:  4510.710026383096
Сумма:  8808</dd>
</dl>

### Коэффициент детерминации
```
r_2 = SSR / sum_desp 

```
⋅⋅В 34.2% случаев изменения х приводят к изменению y.
Другими словами - точность подбора уравнения регрессии низкая.
Остальные 65.8% изменения Y объясняются факторами, не учтенными в модели.

***Коэффициент детерминации показывает обратную связь между зависимой переменной (y) и независимой переменной (x), увеличение x связано с уменьшением значения  y :  0.4880772277444221***

### Оценка значимости уравнения в целом(F - критерий Фишера)

где F - критерий для проверки гипотезы H0: MSR = MSE

```
n = len(np.array(pgns['flipper_length_mm']))
k = 1
#Дисперсия на 1 степень свободы, факторная - msr
#Дисперсия на 1 степень свободы, остаточная - mse
msr = SSR/k
mse = SSE/(n-k-1)

F = msr/mse

if F > 3.86:
  print(f'F - критерий Фишера: {F}, модель значимая')
else:
  print(f'F - критерий Фишера: {F}, модель Не значимая')
```
<dl>
  <dt>Получим следующие результаты</dt>
  <dd>
    F - критерий Фишера: 315.4633681061561.
    Так как 315 > 3.86, то модель признается значимой
  </dd>
</dl>

### Проведем оценку значимости параметра(t-критерий Стьюдента) - определяет статистическую значимость различий средних величин.
Для начала найдем SSX - сумма квадртов отклонений среднего значения от x
```
x1 = np.array(pgns['bill_length_mm'])
x1_mean = x1.mean()
SSX = np.sum((x1-x1_mean)**2)
```
<dl>
  <dt>Получим следующие результаты</dt>
  <dd>
  9928.902702702702
  </dd>
</dl>

Далее найдем стандартную ошибку оценка Syx
```
x_values = np.array(pgns['bill_length_mm'])
x_mean = np.array(x_values)
#Стандартное отклонение остатков
S_E = np.sqrt(SSR/(333-2))

#Стандартная ошибка оценка
S_YX = S_E * np.sqrt(1-(1/333)-((x_values-x_mean)**2)/SSX)
```
<dl>
  <dt>Получим следующие результаты</dt>
  <dd>
    
    Стандартное отклонение остатков:  3.6038684410088146

    Стандартная ошибка оценки: 3.59845316 - это оценка того, как значение статистика критерия меняется от выборки к выборке.
  </dd>
</dl>

### Оценка существенности ошибки корреляции 
Показывает корреляционная связь
```
R_XY = np.sqrt(r_2)
```

---
Таким образом мы рассмотрели данные о длине, глубине клюва пингвина, длину его крыльев, пол и др. выборки. Постарались выявить зависимость между 2 переменнами 


