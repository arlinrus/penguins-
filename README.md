# Предположение о характере взаимосвязи факторов и результата.

## Данные и гипотезы исследования:

Воспользуемся датасетом с сайта : **https://huggingface.co/datasets/SIH/palmer-penguins**

В данном датасете содержаться сведения об Антарктических пингвинах

![penguins_map](https://github.com/arlinrus/penguins-/assets/111064731/c888f21a-f49c-4579-9c0e-1e3d6ec6373f)

Случайных событий - 344

species - вид пингвина (качественная переменная)

**island** - остров, где находятся определенная порода пингвина (качественная переменная)

**bill_length_mm** - длина клюва пингвина (непрерывные количественные)

**bill_depth_mm** - глубина ключа пингвина (непрерывные количественные)

**flipper_length_mm** - длина крыла пингвина (непрерывные количественные)

**body_mass_g** - масса тела пингвина (непрерывные количественные)

**sex** - пол пингвина (качественная переменная)

**year** - возраст пингвина (количественная переменная)

Проведем работу над датасетоим и полуичм следующие столбцы: **dlina_kluva,  glubina_kluva,  dlina_krila,  body_mass_g,  sex,  year**, где sex заменим на 1 - female и 0 - male. Также уберем пустые значения, чтобы не было лишних выбросов.

Отсортируем данные по определенному региону - **Bescoe** и породе пингвина- **Gentoo**. Таким образом, количество случайных событий получилось 119.

## Описательная статистика и проверка допущения о нормальности.
Для того, чтобы проверить данные на наличие выбросов построим **boxplot** для каждого значения столбца с полом пингвина.

![Unknown](https://github.com/arlinrus/penguins-/assets/111064731/4de9d1a4-ec91-4d55-97ce-04be9c603640)

![Unknown-1](https://github.com/arlinrus/penguins-/assets/111064731/e50c3361-7d82-4a24-878a-3146cef77a9d)

![Unknown-2](https://github.com/arlinrus/penguins-/assets/111064731/bcd8d9e8-83d5-43a7-a420-5e3b76e0010e)

![Unknown-3](https://github.com/arlinrus/penguins-/assets/111064731/e4cec844-f85e-48f4-b42d-bbe709d6d87b)

Можем заметить, что есть выбросы при построении 'boxplot' у длины клюва.

*Произведем описательную статистику с помощью джаспа и Python*

<сюда вставить картинку с джаспа>

Проверим данные с помощью теста Шапиро Уилка, где можем заметить, что при alpha равной 0.13 данные имеют нормальное распределение ==> нулевую гипотезу не отвергаем

```
['dlina_kluva', 'glubina_kluva', 'dlina_krila', 'body_mass_g', 'sex', 'year']
[True, True, True, True, True, True]

```

<ins>Убедившись, что для KPA нету противоречий, переходим к анализу.</ins>

## Анализ корреляяции

<mark>Рассмотрим матрицу корреляции, которая показывает связь между данными: </mark>

![Unknown-4](https://github.com/arlinrus/penguins-/assets/111064731/1b6d6afb-1087-4c4e-a2b1-6c9ec4d3798b)

Заметим сильную корреляционную связь между **длиной крыла и массой тело пингвина** , а также слабую корреляционную связь между **длиной клюва и глубиной клюва**.

## Построим гистограмму распеределения

![Unknown-5](https://github.com/arlinrus/penguins-/assets/111064731/829103c6-02bd-48da-89c3-43bbd7c82765)

![Unknown-6](https://github.com/arlinrus/penguins-/assets/111064731/02e2f09d-b184-4111-9400-de3c8f1574e1)

![Unknown-7](https://github.com/arlinrus/penguins-/assets/111064731/6cccc173-9c02-4665-8c5c-0338f5801b84)


![Unknown-8](https://github.com/arlinrus/penguins-/assets/111064731/17bd3f6a-a273-44b9-8e8c-a9583f2d64a8)




### Проведем анализ взаимосвязи между длиной крыла и массой тела пингвина

![Unknown-9](https://github.com/arlinrus/penguins-/assets/111064731/829d140a-e8cb-4d28-bc65-7567b47653a4)


По графику можем заметить не очень сильную корреляцию, но для того, чтобы убедиться в этом проведем следующие исследованияя. 


### Вычислим коэффициенты b1 и b0 для уравнения линейной регрессии:
Сравним данные длины клюва и массы тела, где длинна - зависимая переменная.  

```
x1 = gentoo['body_mass_g']
y1 = gentoo['dlina_krila']

mean_x = sum(x1)/len(x1)
mean_y = sum(y1)/len(y1)

covariance = sum((xi-mean_x)*(yi-mean_y) for xi, yi in zip(x1, y1))
variance = sum((xi - mean_x)**2 for xi in x1)

b1 = covariance / variance
b0 = mean_y - b1*mean_x
```

<dl>
  <dt>Таким образом получим следующие рзультаты:
</dt>
  <dd>b1 - наклон линии:   0.009340925526910201
    
b0 - интерсепт  169.66721958565057

Уравнение линейной регрессии: y = 169.67 + 0.0093*x1</dd>
</dl>

### Ошибка аппроксимации - показывает отклонение реальных данных от фактических.


**Её мы решили высчитать с помощью данных из таблицы Excel**


**https://docs.google.com/spreadsheets/d/11jl65RG5bq_B9jB2fL5kEbNQmXPClRFpGG5gp--5yoE/edit#gid=1066452339**

<dl>
  <dt>Получим следующие результаты</dt>
  <dd>
    Ошибка аппроксимации получилась: 
  </dd>
</dl>

### Визуализируем график

![Unknown](https://github.com/arlinrus/penguins-/assets/111064731/561ff00f-7be3-426e-abdf-5d3c6d8f51a8)

### Рассчитаем множественную модель(корреляция) - показывает, насколько хорошо комбинация независимых переменных объясняет вариацию зависимой переменной.

```
a = gentoo['body_mass_g']
b = gentoo['dlina_krila']

corr_coef = np.corrcoef(a,b)
mult_corr = corr_coef[-1: 1]
mult_corr = np.sqrt(np.prod(mult_corr**2))

```

### Рассчитаем объясненную и необъясненную дисперсию
SSR - регрессионный анализ

SSE - ошибка суммы
```
x2 = np.array(gentoo['body_mass_g'])
y2 = np.array(gentoo['dlina_krila'])

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
  <dd>Объясненная дисперсия:  4867205.448317983
    
Необъясненная дисперсия:  4871657.176679962

Сумма:  9738862</dd>
</dl>

### Коэффициент детерминации
```
r_2 = SSR / sum_desp 

```
⋅⋅В 43% случаев изменения х приводят к изменению y.
Другими словами - точность подбора уравнения регрессии низкая.
Остальные 57% изменения Y объясняются факторами, не учтенными в модели.

***Коэффициент детерминации показывает обратную связь между зависимой переменной (y) и независимой переменной (x), увеличение x связано с уменьшением значения  y :   0.4304430075025573***

### Оценка значимости уравнения в целом(F - критерий Фишера) -- ЭТО НУЖНО ПРОСМОТРЕТЬ И РАССЧИТАТЬ ЗАНОВО

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
x1 = np.array(gentoo['dlina_krila'])
x1_mean = x1.mean()
SSX = np.sum((x1-x1_mean)**2)
```
<dl>
  <dt>Получим следующие результаты</dt>
  <dd>
  5117.411764705881
  </dd>
</dl>

Далее найдем стандартную ошибку оценка Syx
```
x_values = np.array(gentoo['dlina_krila'])
x_mean = np.array(x_values)

#Стандартное отклонение остатков
S_E = np.sqrt(SSR/(333-2))
print('Стандартное отклонение остатков: ', S_E)
#Стандартная ошибка оценка
S_YX = S_E * np.sqrt(1-(1/333)-((x_values-x_mean)**2)/SSX)
```
<dl>
  <dt>Получим следующие результаты</dt>
  <dd>
    
    Стандартное отклонение остатков:  2.5359985775515517

    Стандартная ошибка оценки: 2.53218791 - это оценка того, как значение статистика критерия меняется от выборки к выборке.
  </dd>
</dl>


### Оценка существенности ошибки корреляции 
Показывает корреляционная связь
```
R_XY = np.sqrt(r_2)
```
<d1>
  <dt>Оценка существенности коэффициента корреляции(RXY):  0.65608155552687 </dt>
  <dd>
    Связь средняя
  </dd>



### Гомоскедантичность остатков

Гомоскедастичность остатков в линейной регрессии означает, что дисперсия остатков (разниц между фактическими значениями зависимой переменной и предсказанными значениями, полученными с помощью модели) остается const (неизменной) по всем значениям независимой переменной. Другими словами, это означает, что разброс ошибок модели одинаков для всех уровней независимой переменной.

вставить картинку


---
Таким образом мы рассмотрели данные о длине, глубине клюва пингвина, длину его крыльев, пол и др. выборки. Постарались выявить зависимость между 2 переменнами 


