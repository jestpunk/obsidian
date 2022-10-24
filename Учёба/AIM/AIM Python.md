# OZON::Python

#python #ozon #prog #course  | [[Python]] [[Ozon]] [[Course]] [[Prog]]

---

## Content

^988180

1) [[AIM Python#^344e11|Venv]]
2) [[AIM Python#^8c4e67|Аргументы]]
3) [[AIM Python#^578e7e|Packing]]
4) [[AIM Python#^53b053|Iterable protocol]]
5) [[AIM Python#^da519c|Generator]]
6) [[AIM Python#^361826|Проверка]]
7) [[AIM Python#Ozon Python2 988180 Ленивые генераторы|Ленивые генераторы]]
8) [[AIM Python#Ozon Python2 Content Context Manager|Context manager]]
9) [[AIM Python#Ozon Python2 Content Testing|Testing]]
10) [[AIM Python#Ozon Python2 Content Decorator|Decorator]]

---
&nbsp;
## [[AIM Python#^988180|Venv]]

^344e11

Мы не можем ставить несколько одинаковых пакетов разных версий, поэтому для более точного набора и контроля версия пакетов, нужно иметь несколько хранилищ пакетов - виртуальные окружения

Делается это с помощью `venv` (есть в std питона)
```python
$ python3 -m venv <path> # запустить виртуальное окружение
$ source <path>/bin/activate # 
```
> - `activate`  добавляет `<path>` в `PATH`
> - `bin` хранит бинари питона

Командой `which` можно узнать, какие реализации питона у нас есть:
```python
$ which -a python3
```

В `PATH` лежит набор и порядок папок, в которых мы ищем бинари
```Python
echo $PATH
```

В новом окружении будет pip и python и больше, по сути, ничего

---

Но как сделать так чтобы при запуске `python3` мы обращались не к системному питону, а к новому виртуальному окружению? Для этого нужно поднять его в приоритете
```python
source <path>/bin/activate
```
> - `source` исполняет весь движ только для текущей версии консоли и применять все изменения здесь (поэтому без него активация произойдет в другой, вызванной скриптом консоли)
> 
> - После активации для отмены изменений используется команда `deacticate`

После этого в `PATH` бинарь нового питона стоит выше системного. вызов `python3` и `pip3` будут работать с новой экосистемой

### Итого
- Всегда есть ✔
- Прямолинейный и простой ✔
- Не мусорит в файлах ✔
- Не менеджер пакетов, лишь изолирует окружение ❗
- Можно взорвать если делать много `activate` ❗

---
&nbsp;
## [[AIM Python#^988180|Аргументы]]

^8c4e67

Чтобы вывести список локальных переменных (и в частности аргументов функции), можно восользоваться функцией `locals()`

Можно задать, в каком виде функция должна получать аргументы:
```Python
def named_func(*, a, b, default = 3)# после * идут только именованные аргументы (слева как угодно)
```
> - Обычно это делается для функций, которые обновляются, и для новых версий функции не нужно думать об обратной совместимости - новый аргумент может быть вызван только по явному имени, если он идёт после *
> 
> - Чтобы не сломать совместимость, нужно сделать новый аргумент еще и необязательным

```Python
def pos_func(a, /) # аргумент можно передать только позиционно
```
> - это делается для того, чтобы оставить за собой возможность поменять имя аргумента - оно нигде не фигурирует. Но для этого надо быть уверенным что оно нам надо.

---
&nbsp;
## [[AIM Python#^988180|Packing]]

^578e7e

 Допустим у нас есть декоратор, которому даются параметры, и внутри него мы хотим вызвать функцию с теми же параметрами
```python
def wrapper(<parameters>):
	func(<parameters>) 
```

Вместо `<parameters>` используются `args, kwargs`
```Python
def wrapper(*args, **kwargs):
```
> - `args` - это кортеж позиционных аргументов
> - `kwargs` - это словарь именованых аргументов типа {имя : значение}

Ньюанс может быть в следующем: если нашей функции на вход подаются `args, kwargs` и она пробрасывает их другой фукнкции, её набор `args, kwargs` будет отличаться
```Python
def func(*args, **kwargs):
	print("func", locals())
	
def wrapper(*args, **kwargs):
	func(args, kwargs)
	
wrapper(1,2,3, named = "blabla")
```
> - У функции `wrapper` : `args` == (1,2,3) `kwargs` == {named : "blabla"}
> - У функции `func` : `args` == ((1,2,3), {named : "blabla"}) `kwargs` == {} 

Это происходит потому что питон отправляет запакованные аргументы как один набор позиционных аргументов. Для корректной работы перед отправкой аргументы нужно распаковать
```Python
def func(*args, **kwargs):
	print("func", locals())
	
def wrapper(*args, **kwargs):
	func(*args, **kwargs) # всё передастся куда надо
	
wrapper(1,2,3, named = "blabla")
```
> - Получается, что операторы */** занимаются и распаковкой, и запаковкой - зависит от контекста 

---

 А что если кроме `args` и `kwargs` мы хотим использовать другие аргументы?
 
Питон умный и сам мэтчит все аргументы, который совпадают по имени, если это возможно 👍

---

Звёздочки используются не только в функциях - с помощью них можно делать быструю конкатенацию:
```python
list1 = [1,2,3,4]
list2 = ['a', 'b', 'c', 'd']

list3 = [*list1, *list2] # list3 == [1,2,3,4,'a','b','c','d']
```
 
 Также с помощью распаковки массива можно контролировать "необязательные" аргументы/возвращаемые значения
 ```Python
def func(add_parameter = False):
	result = [1,2,3, *(['param1', 'param2'] if add_parameter else [])]
	return result
# вернёт [1,2,3] или [1,2,3,'param1','param2']
```

При этом важно запомнить что `*` это распаковка списков, а `**` это распаковка словарей, и это опять же можно делать не только в сигнатуре функции

---
&nbsp;
## [[AIM Python#^988180|Iterable protocol]]

^53b053

Известно, что для `iterable` объектов можно, например, использовать цикл for. Но чем характеризуется этот тип объектов под капотом?
```Python
for value in iterable:
	...
```
 Этот код - простой синтаксический сахар вокруг следующей конструкции:
```Python
iterator = iter(iterable)

while True:
	try:
		value = next(iterator)
	except StopIteration:
		break
```

Формально протокол итерируемых объектов гласит, что у нас должен быть определен магический **dunder-метод** (double underscore) `__iter__`
```Python
class MyIterable:
	def __iter__(self): -> Iterator # получить итератор
```

> - Теперь `iter(MyIterable)` эквивалентно `MyIterable.__iter__()`
> - Заметим, что набор dunder-методом строго фиксирован и придумывать что-то своё нельзя

---
А в итераторе должен быть определён другие dunder-методы:
```Python
class Iterator:
	def __next__(self) -> data_type
	def __iter__(self) -> copy_of_itself
```
> - Теперь `next(Iterator)` эквивалентно `iterator.__next__()`

Получается что процесс итерации двухступенчат - занимается итерацией не объект, а создаваемый им  представитель класса итераторов.

Но зачем нам усложнять программу и вводить дополнительный класс? Представим, что итерация происходит в самом классе:
```Python
class MyList:
	def __next__(self):
		self.iteration_index += 1
		return self.data[self.iteration_index]
```

Тогда при запуске этого кода:
```Python
for i in MyList:
	for j in MyList:
		...
```
Начнётся каша с `iteration_index`. Поэтому нам нельзя чтобы во-первых состояние итерации хранилось в единственном экземляре, а во-вторых чтобы к нему имели доступ все и сразу.

Все эти проблемы решаются введением отдельного класса "экземпляра итерации", которым и является `Iterator`

Новый вопрос - зачем в итераторе нужен метод `__iter__`? Потому что разработчики хотели, чтобы и сами итераторы можно было использоваться в цикле `for`. Но если мы возьмем итаратор от итератора, он вернет себя.

---
&nbsp;
## [[AIM Python#^988180|Generator]]

^da519c

Муторно каждый раз писать все dunder-методы, чтобы сделать объект итерируемым. В частности для этого существуют генераторные функции
```Python
def func(): # обычная функция
	return 5

def gen(): # генератор
	yield 5 
```
Что такое генератор? Это итератор, написанный особенным образом. команда `yield` выполняет функцию `return`, но может происходить множество раз.
```Python
def gen():
	print("before")
	yield 1
	print("after")
	yield 2
	print("last")
	yield 3

for element in gen:
	print(el)

# > before
# > 1
# > after
# > 2
# > last
# > 3
```

> - То есть генератор это функция, которая запоминает своё состояние и положение исполнения после выхода, и мы можем обратиться к ней еще раз для возвращения нового объекта
>   
>  - По сути это снова синтаксический сахар над тем, что мы писали выше. Просто в формате функции воспринимать это гораздо удобнее
>  
>  - Эта функция по умолчанию итерируемая, и следующий элемент для нее есть следующее возвращаемое значение `yield`


А что если мы захотим написать `yield` внутри определения `__iter__` итерируемого объекта? Всё будет хорошо 👍
```Python
class MyIterable:
	def __iter__(self) # должна вернуть итератор
		yield 1
		yield 2
```
> - После вызова `__iter__` создаётся итератор с написанным нами телом (читай генератор). У генератора тоже определен метод `__next__` - следующий возвращаемым `yield`-ом объект - который он и будет выводить
> 
> - В текущем примере `__iter__` - это генераторная функция. То есть такая, которая возаращает генератор

---

### Пример
В библиотеке `itertools` есть конструкция `chain`, которая позволяет итерироваться последовательно по нескольким итерируемым объектам
```Python
from itertools import chain
chain(a,b,c,...)
```
Код, реализующий эту структуру, будет выглядеть как-то так:
```Python
class MyChain:
	def __init__(self, *iterables):
		self._iterables = iterables
	def __iter__(self):
		for iterable in self._iterables:
			for value in iterable:
				yield value
```
Теперь можно вытворять такие штуки:
```Python
for value in MyChain([1,2,3], 'abc', [7,8,9]):
	print(value)
	
# > 1 2 3 a b c 7 8 9
```

---

Также для структуры
```Python
for value in iterable:
	tield value
```

Придумали сокращенный способ записи
```Python
yield from iterable
```

Это не просто синтаксический сахар - структура нужна для асинхронности

---
&nbsp;
## [[AIM Python#^988180|Проверка]]

^361826

Как можно проверить, что твой объект действительно итерабелен?

Есть `collections.abc` (abstract based classes), в которой лежит и `Iterable`. Можем вызвать для него `isinstance`

```Python
from collections.abc import Iterable

class  MyIterable:
	...

class TrueIterable:
	def __iter__(self):
		yield 1
	
isinstance(MyIterable, Iterable) # > False
isinstance(TrueIterable, Iterable) # > True
```
Но есть способ надежнее. Если попробовать унаследоваться от объекта нашего типа,  интерпретатор наругается на наличие сугубо абстрактного класса `__iter__`
```Python
MyIterable() # Error
```

---
&nbsp;
## [[AIM Python#^988180|Ленивые генераторы]]
Если мы итерируемся по списку несколько раз, итерация в точности повторяется, так как список объект постоянный
```Python
l = [1,2,3,4]

for element in l:
	print(l)
for element in l:
	print(l)

# > 1 2 3 4 1 2 3 4
```
 Но в питоне есть функции `map` и `filter`
 > - `map` принимает на вход функцию, которая будет применена к итерабельному объекту
```Python
iterable = map(lambda x: x*x, l)
```
Особенность `map` в том, что ее можно вызвать только единожды
```Python
list(iterable) # > [1, 4, 9, 16]
list(iterable) # > []
```
Таким образом, генераторы бывают ленивыми и могут не поддерживать вызов второй раз

---

### try/except
Есть множество задач, где мы работаем с ресурсами, которые мы хотим отдать назад или утилизировать (например, открытый файл, подключение к базе данных или выделенная память)

Например, есть обёртка над файлом:
```Python
class FileWrapper():
	def __init__(self, path, mode = 'rt'):
		self._file = open(path, mode)
		
	def read(self, size = -1, /):
		return self._file.read(size)
	
	def readline(self):
		return self._file.readline()
	
	def close(self):
		self._file(close)
	
	def __iter__(self):
		line = self.readline()
		while line:
			yield line
			line = self.readline()
```
Это небезопасно, так как при исключении в работе с файлом файл не закрывается.

---

Напишем в классе среди прочего "обработчик" функции чтения:
 ```Python
class FileWrapper():
	def __init__(self, path, mode = 'rt'):
		self._file = open(path, mode)
		self._read_cnt = 0 # добавили поле
		
	def _on_read(self): # добавили обработчик
		self._read_cnt += 1
		if self._read_cnt >= 2:
			raise ValueError("too much")
		
	def read(self, size = -1, /):
		self._on_read() # вызываем обработчик на каждую попытку чтения
 		return self._file.read(size)
	
	def readline(self):
		return self._file.readline()
	
	def close(self):
		self._file(close)	
		
	def __iter__(self):
		line = self.readline()
		while line:
			yield line
			line = self.readline()
 ```
Теперь после того, как будет прочтена вторая строка, вылезет ошибка.

---

Что делать, если наша структура выдала нам ошибку и программа закрылась досрочно? Обмазаться `try / except`
```Python
f = FileWrapper('./file.txt')
try:
	for line in f:
		pass
finally:
	f.close()		
```
Теперь наш файл закроется в любом случае 👍

---

### Минусы try/except
Напишем какой-нибудь генератор, последовательно выделяющий два ресурса и гарантирующий нам, что эти ресурсы будут закрыты
```Python
def some_generator():
	resource = SomeResource()
	try:
		yield from source1
		resource2 = SomeResource2()
		try:
			yield from list('abcdef')
		finally:
			resource2.close()
	finally:
		resource.close()		
```

Удобнее вместо этой колбасы написать конструкцию с `with`
```Python
def some_generator():
	with SomeResource() as resourcs:
		yield from source1
		
		with SomeResource2() as resource2:
			yield from list('abcdef')
```

> ☝   Под капотом всё работает следующим образом:
> 
> ---
> 
> При конструкции 
> ```Python
> with obj as value:
>	 somecode
> ```
> Последовательно выполняются три операции:
> - `value = obj.__enter__()`
> - `somecode`
> - `value.__exit__()`
> 
> `__enter__` и `__exit__` - магические методы объекта. Эти методы присущи контекстным менеджерам

---
&nbsp;
## [[AIM Python#Content|Context Manager]]

Контекстным менеджером называется класс, для которого определены следующие методы
```Python
def MyContextManager:
	def __enter__(self):
		print('Enter')
		return ('Hello')
	def __exit__((self, exc_type, exc_value, traceback): pass
		print('Exit')
		return( locals())
```
> - `enter` и `exit` вызываются, когда мы заходим в конструкцию с помощью конструкции `with` и, например, на выходе содержат информацию о возможной ошибке, которую далее можно обработать
> 
> - Возвращаемое значение `__exit__` проверяется на истинность. Если мы захотим вернуть `False` или ничего не вернуть, выдастся ошибка

Проверочный код:
```Python
with MyContextMamager() as value:
	print('value = ', value)
	
# Enter
# value = Hello
# Exit
# {'self': <__main__.MyContextManager object at 0x7asdf9282>, 'exc_type':None, 'exc_value':None, 'traceback':None}
```
> -  В `exit` ничего не передалось (все параметры равны `None`), потому что не выпала никакая ошибка

---

### Сценарии использования

- Файлы

```Python
data  = open('./file.txt').read() # BAD

with open('./file.t xt') as f: #✔️
	data = f.read()
```

- Инструментирование (например, захотели посчитать время работы)
```Python
with object.SOME_LONG_WORK.measure(): # GOOD
	blabla
	
object.SOME_LONG_WORK.update_time()
```

- В целом, любые операции, которые имеют конкретную точку вывода, где надо либо подвести итоги, либо что-то оменить, удобно обернуть контекстным менеджером

---
&nbsp;
## [[AIM Python#Content|Testing]]


Существует три типа тестов:

> -  ```unit test``` - проверяет работу одной функции или блока программы
> -  ```integration test``` -  проверяет работу нескольких блоков в связке
> - ```end-to-end``` - проверяет полный путь пользователя в программе

 В питоне существует два основных фреймворка для тестов - стандартный вшитый инструмент для unit-тестов (всегда есть, но топорный) и ```pytest```, который позволяет сократить объем работы
 
 После запуска команды ```pytest PATH``` в определенной директории ищутся питонячьи файлы с названием ```test_*.py```.
 
 Тестом считается любой класс, называющийся ```class Test*``` и любая глобальная функция, начинающайся на ```def test_*```  (в классе тоже может быть объявлена тестирующая функия - таким образом класс это своеобразная "папка" для группировки тестов)
 
 > -  ```pytest -v PATH``` выведет больше информации

&nbsp;



### unittest.mock :

Пусть у нас есть функция, которую мы хотим протестировать, а внутри нее вызывается функция, делающая тяжелые вычисления, и ее мы прогонять не хотим – как нам быть?

```Python

def HARDFUNCTION():
	sleep(999999)
	return 1
	
def important_function():
	result = 0
	try:
		result = HARDFUNCTION()
	except:
		...
	return result

```

Использовать **mock** (от английского заглушка) и **patch**

```Python
from unittest.mock import mock

m - Mock()
```

При вызове любого ее поля оно создается и считается количество вызовов:

```Python
m.blabla
m.blabla.first.second.third

m.f()

m.value = 100500

m.f.call_count # -> 1
```

> 👆 В документации есть много других крутых методов у полей мока! 

Также у мока есть поле ```return_value```, которое он вернет, при вызове, и атрибут ```side_effect()```, которому можно передать exception, который можно протестировать (или даже итерабл, содержащий значения / ошибки по порядку)

Здесь важно, что в ```return_value``` не получится зарейзить ошибку или передать список – это обычное возвращаемое значение

&nbsp;

Если mock уже поживший, а мы хотим использовать его заново, можно использовать ```m.reset_mock()``` – метод удалит все накопленные счётчики и записи использования

&nbsp;


### patch

**patch** – это и декоратор, и контекстный менеджер, в который передается строка, содержащая полный путь до функции (например, ```"__main__.HARDFUNCTION"```) и возвращает мок.

Например, следующий код подставляет вместо ```HARDFUNCTION``` мок :
```Python
from unittest.mock import patch 

with patch("__main__.HARDFUNCTION") as m:
	value = 10
	m.return_value = data
	assert important_function() == 10

```

Также патч может быть вызван как декоратор к тестирующей функции :
```Python
@patch("__main__.HARDFUNCTION")
def test(m):
	m.return_value = 666
	...
```

Заметим, что подмена происходит только в рамках контекстного менеджера (или декоратора, как увидим позднее) – во всех остальных местах функция останется нетронутой

---
&nbsp;
## [[AIM Python#Content|Decorator]]

Смоделируем пример: пусть мы хотим замерять время работы программы, тогда:

```Python
import time

def some_long_operation(): # наша долгая операция
	start_time = time.time()
	time.sleep(3)
	end_time = time.time()
	print("duration is", end_time - start_time)
```

Но мы же не будем каждый раз дописывать эти строки при объялении функции. Надо придумать обёртку, которую можно всегда подключить:

```Python
def timeit(func, *args, **kwargs):
	start_time = time.time()

	func(*args, **kwargs)
	
	end_time = time.time()
	print("duration is", end_time - start_time)

-> timeit(some_long_operation, 5)	
```

Симпатично. Но мы потеряли возможность просто вызывать функцию, приходится возиться с timeit.
Одним из решений будет:
```Python
def timeit(func, *args, **kwargs):
	def wrapper():
		start_time = time.time()

		result = func(*args, **kwargs)

		end_time = time.time()
		print("duration is", end_time - start_time)
		return result
	return wrapper

# теперь можно делать так
-> some_long_function = timeit(some_long_function, 5)
```

Но теперь у функции зафиксированы аргументы. Теперь делаем так:
```Python
def timeit(func):
	def wrapper(*args, **kwargs):
		start_time = time.time()

		result = func(*args, **kwargs)

		end_time = time.time()
		print("duration is", end_time - start_time)
		return result
	return wrapper

# наконец-то умеем делать так
-> some_long_function = timeit(some_long_function)
```

Теперь всё работает по красоте. Именно над этой структрой и есть синтаксический сахар в виде декоратора:

```Python
@timeit
def some_long_function():
	...

# эквивалентно

some_long_function = timeit(some_long_function) 
#, где timeit принимает функцию и возвращает ее обернутую версию 
```
&nbsp;

> ### ☝ Замечание :
> 
> Декораторы исполняются снизу вверх, оборачивая функцию всё новым функционалом:
> ```Python
> @last_decorator
> @mid_decorator
> @first_decorator
> def f(x):
> 	pass
> ```

&nbsp;

Благодаря гибкости питона мы можем развить идею декоратора и сделать ее более абстрактной - ведь по сути генератор это объект, получающий на вход что-то (а это значит только то что это callable объект от одного аргумента) и возращающий что-то. Попробуем записать это в виде класса:

```Python
class timeit:
	def __init__(self, func): # timeit(func)
		self._func = func
		
	def __call__(self, *args, **kwargs):
		start_time = time.time()
		
		result = self._func(*args, **kwargs)
		
		end_time = time.time()
		print("duration is", end_time - start_time)
		
		return result
		
# Теперь опять же работает
@timeit
def f(x):
	pass
```

В чем разница? В каких случаях использовать фукнцию, а в каких класс? Первая очевидная разница заключается в том, что функция полностью инкапсулирует в себе всю логику и поля, и к ее внутренним полям невозможно обратиться (у функции своя область видимости), а методы класса можно получить.

Мы можем декорировать не только функции, но и классы (а почему нет?). Но для правильной работы и возвращать должны класс:
```Python
def set_field(cls):
	cls.field = 0
	return cls # <-- возвращаем класс
	
@set_field
class foo:
	...

# Теперь в классе есть поле field = 0 
```

&nbsp;


> ### Dataclasses :
> Пусть есть какой-то класс "хранилка" имеющий только поля и не имеет функций (например студент с именем, телефоном и группой 🤪). 
> 
> Для такой мелочи нам лень постоянно определять для него ```__str__```, ```__repr__```, ```__eq__``` и так далее. Для этого существует библиотека декораторов dataclasses:
> 
> ```Python
> import dataclasses
> 
> @dataclasses.dataclass
> class Student:
> 	name: str = ""
> 	phone: int = 0
> 	group: int = 0
> ```
> 
> По сути под капотом этого декоратора лежит что-то такое:
> ```Python
> def dataclass(cls):
> 	def __init__(self, *args, **kwargs):
> 		# Внутренняя магия
> 	
> 	cls.__init__ = __init__
> 	
> 	return class
> ```
> 
> То есть переопределение методов класса

&nbsp;


Существуют декораторы с аргументом, но это значит лишь то, что при вызове его как оболочки мы присваиваем ему какие-то аргументы:
```Python
@deco(arg = 1)
def func():
	...
	
# равносильно
func = deco(arg = 1)(func)
```

&nbsp;


> ###  Staticmethod, classmethod :
> 
> С помощью декоратора ```@staticmethod``` любой вызов функции (и как функции класса, и как метода конкретного представителя) будет идти вместе с  обязательным аргументом ```self```
> 
> Аналогично, с помощью декоратора ```@classmethod``` любой вызов функции (и как функции класса, и как метода конкретного представителя) будет идти вместе с обязательным аргументом ```self```
> 
> Заметим, что использовать ```@staticmethod``` почти никогда не нужно, потому что зачастую в процессе написания программы оказывается, что внутренние методы класса нет-нет да и пригождаются.
>
> - Заметим очевидное: ```@classmethod``` может вызывать ```@staticmethod```, но не наоброт  

&nbsp;


Есть еще одна проблема – когда мы объявляем декоратор, теперь вместо мета-данных исходной функции у нас имеются метаданные обёртки

```Python
def decorator(func):
	def wrapper(*args, **kwargs):
		print('starting')
		return func(*args, **kwargs)
	return wrapper
	
def func(x):
	'''This function is cool'''
	print(x)

help(func) # -> func, "This function is cool"
help(decorator(func)) # wrapper, ""
```
 
Поэтому надо пользоваться встроенным в ```functools``` декоратором ```wraps```:

```Python
import functools

def decorator(func):
	@functools.wraps(func) # засунуть в метаданные wrapper метаданные func
	def wrapper(*args, **kwargs):
		print('starting')
		return func(*args, **kwargs)
	return wrapper
	
def func(x):
	'''This function is cool'''
	print(x)

help(func) # -> func, "This function is cool"
help(decorator(func)) # func, "This function is cool"
```
