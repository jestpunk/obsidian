# Pandas
#python #libs #pandas #prog #datascience  | [[Python]] [[Prog]] [[DataScience]]

---

## Content

^057a8f

1) [[Pandas#Pandas 057a8f Load data|Load data]]
2) [[Pandas#Pandas 057a8f Read data|Read data]]
3) [[Pandas#Pandas 057a8f Analize data|Analize data]]
4) [[Pandas#Pandas 057a8f Change data|Change data]]
6) [[Pandas#Pandas 057a8f Save data|Save data]]

---

## [[Pandas#^057a8f|Load data]]

^b86c5b

```Python
import pandas as pd
```


Чтобы получить объект данных, нужно считать его в зависимости от формата:
```Python
data = pd.read_csv('filename')
data = pd.read_excel('filename')
data = pd.read_parquet('filename')
...
```
> - **delimeter** - символ разделения

---

## [[Pandas#^057a8f|Read data]]

^b476ae

Чтобы посмотреть первые 5 строк таблицы, можно использовать
```Python
data[:5] #или
data.head(5)
```

Чтобы посмотреть заголовки, используем
```Python
data.columns
``` 

Взять столбец
```Python
data['Name']
```
Для нескольких столбцов нужно взять имена столбцов в лист:
```Python
data[['Name', 'Type', 'Value']]
```

Конкретные строки
```Python
data.iloc[3:7]
```
Можно взять не только конкретные строки, но и столбцы
```Python
data.iloc[3,7] #3 строка, 7 столбец
data.iloc[:, 4:9] #все строки, столбцы 4-8
```
Можно получить те записи, для которых выполнено некоторое условие
```Python
data.loc[data['Name'] == 'Nikita'] #одно условие
data.loc[(data['Name'] == 'Nikita') & (data['Age'] > 15) | (data['City'] == 'Moscow')]
#несколько
```

---

## [[Pandas#^057a8f|Analize data]]

^e90181

Таблица параметров нашей таблицы (среднее, минимум, квантили)
```Python
data.describe()
```

Отсортированная по какому-то полю таблица
```Python
data.sort_values('Name')
```
> Можно дать не поле, а список полей. Тогда сортировка будет лексикографической
> 
> - **axis**(0) - ось сортировки
> 
> - **key** - ключ сортировки
> 
> - **ascending**(True) - в возрастающем ли порядке
> 
> Если сортируем по нескольким полям, подаём параметры списком

---

## [[Pandas#^057a8f|Change data]]

^3da061

Добавить новый столбец можно простой инициализацией
```Python
data['newcolumn'] = list(range(100)) #создается столбец 'newcolumn'
```

Удалить кусок таблицы
```Python
data.drop()
```
>- **columns** - список удаляемый столбцов
>
>- **inplace**(False) - копия или inplace
>
> - **labels** - список названий столбцов для удаления
> 
> - **axis**(0) - направление удаления
> 
> Заметим, что ***labels, axis=1*** эквивалентно ***columns = labels***

Поменять местами столбцы
```Python
cols = list(data.columns.values)
data = data[[cols[3]] + [cols[-1]] + cols[:2]] #поставить столбцы в порядке 3,-1,0,1
```
> Засовывать в список нужно только единичные столбцы, так как срезы уже двумерные
---

## [[Pandas#^057a8f|Save data]]

^3ee113

Сохраняем дату
```Python
data.to_csv('filename.csv')
data.to_excel('filename.xlsx')
```

>- **index**(False) - добавлять ли в таблицу индексы строк
>
>- **sep** - разделяющий символ
