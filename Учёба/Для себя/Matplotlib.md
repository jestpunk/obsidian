# Matplotlib
#python #libs #matplotlib #prog |  [[Python]] [[Prog]]

---
## Content

^956c71

1) Easy plot [[Matplotlib#^ace8bd|Ъ]]
1) Charts [[Matplotlib#^53a45f|Ъ]]
4) Stack Plots [[Matplotlib#^931fc7|Ъ]]
5) Filling [[Matplotlib#^7c58aa|Ъ]]
6) Histograms [[Matplotlib#^57dde8|Ъ]]
7) Scatter plot [[Matplotlib#^94f7f4|Ъ]]
9) Subplot [[Matplotlib#^90c13b|Ъ]]

---
## Easy plot [[Matplotlib#^956c71|Ъ]]

^ace8bd

 
Импортим  
 
```Python
from matplotlib import pyplot as plt
```

Берём в качетсве данных массивы x и y (например, года и зарпалаты по годам)

```Python
ages_x = [18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55]

dev_y = [17784, 16500, 18012, 20628, 25206, 30252, 34368, 38496, 42000, 46752, 49320, 53200, 56000, 62316, 64928, 67317, 68748, 73752, 77232,
78000, 78508, 79536, 82488, 88935, 90000, 90056, 95000, 90000, 91633, 91660, 98150, 98964, 100000, 98988, 100000, 108923, 105000, 103117]
```

Записываем все параметры графика в plt.plot()

```Python
plt.plot(ages_x, dev_y, color='#444444', linestyle='--', label='All Devs')
```
>  https://matplotlib.org/stable/api/_as_gen/matplotlib.pyplot.plot.html
>  
> - **color** - цвет графика (заготовленный типа 'black' или hex) 
>
>  - **linestyle** - стиль графика (['-', '--', '-.', ':']) 
>  
>  - **label** позволяет обозвать график в легенде
>  
>  - **marker** - стиль отметки узловых точек (['.', ',', 'o', ...])
>  
>  - **scalex / scaley** - масштаб, заменяемый на 'log'
>    
>  - **alpha** - прозрачность в пределах [0;1]

Чтобы задать имя графика и обозначить оси, используем:
```Python
plt.xlabel('Ages')
plt.ylabel('Medfian Salary (USD)')
plt.title('Median Salary (USD) by age')
```
Чтобы отображать легенду, используем:
```Python
plt.legend()
```
>  **loc** - местоположение ('upper right', 'lower left' или (0.07, 0.05))
Для применения стиля используем:
```Python
plt.style.use('stylename')
```
>  https://tonysyu.github.io/raw_content/matplotlib-style-gallery/gallery.html

Для вывода вертикальной линии используем:
```Python
plt.axvline(value, color, label, ...)
```
Наконец, для вывода графика
```Python
plt.tight_layout()
plt.show()
```

---
## Charts [[Matplotlib#^956c71|Ъ]]

^53a45f

Чтобы нарисовать линейную диграмму, используем  вместо plt.plot() plt.bar():
```Python
plt.bar(x,y)
```
Для горизонтальной линейной диаграммы используем plt.hbar():
```Python
plt.hbar(x,y)
```
Для круговой диаграммы используем plt.pie()
```Python
plt.pie([list of values])
```
> - **labels** - массив наименований
> 
> - **wedgeprops** - словарь параметров (например {'edgecolor': 'black'})
> 
> - **colors** - массив цветов (слова или hex)
> 
> - **explode** - массив "отдалений" долей от центра в пределах [0;1]
>
> - **shadow** - будет ли тень (True/False)
>
> - **startangle** - начальный угол в градусах
---
## Stack plots [[Matplotlib#^956c71|Ъ]]

^931fc7

Для стакплота используем plt.stackplot():
```Python
plt.stackplot(x, y1, y2, y3, ..., )
```
> необязательные параметры передаются массивом, как и в plt.pie()
---
## Filling [[Matplotlib#^956c71|Ъ]]

^7c58aa

Для закраски области между графиками используем plt.fill_between():
```Python
plt.fill_between(x, y1, y2)
```
> - Если указаны только x и y1, закрашивает область под y1
> 
> - **alpha** - прозрачность
> 
> - **where** - логическое выражение, задающее область закраски
> 
> - **interpolate** - аккуратно закрашивать границу пересечения с where (True/False))
> 
> - **color** - цвет, как и в plt.plot()
---
## Histograms [[Matplotlib#^956c71|Ъ]]

^57dde8

Для гистограм используем plt.hist():
```Python
plt.hist(x)
```
> - **bins** - количество бинов или массив разделяющих значений
> 
> - **edgecolor** - цвет границы
> 
> - **log** - логарифмичный масштаб (True/False)
---
## Scatter plot [[Matplotlib#^956c71|Ъ]]

^94f7f4

Для scatter plot используем:
```Python
plt.scatter(x,y)
```
> - **s** - размер точек или массив размеров
> 
> - **c** - цвет точек или массив цветов
> 
>  - **cmap** - цветовая схема, в рамках которой будет колебаться цвет
> 
>  - **marker** - маркер
> 
> - **edgecolor** - цвет границ
> 
> - **alpha** - прозрачность
---
## Subplots [[Matplotlib#^956c71|Ъ]]

^90c13b

Чтобы вывести несколько графиков в одном окне (фигуре) или в нескольких, используется plt.subplots()
```Python
figure, plots = plt.subplots(nrows, ncols)
```
> - **plots** - массив (может быть двумерным) графиков
>
>  - **nrows, ncols** - количество подграфиков
>  
> - **sharex, sharey** - отвечает за то, одинаковая ли ось Х/Y у графиков

Теперь plt.show() покажет все "накопленные" фигуры

---
