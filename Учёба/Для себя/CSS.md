# CSS
#prog  #web #course |  [[Web]] [[Prog]] [[Course]]
- - -
&nbsp;
## Content
1) [[CSS#CSS Content Полезные ссылки | Полезные ссылки]]
2) [[CSS#CSS Content Свойства стилей | Свойства стилей]]
3) [[CSS#CSS Content Основы | Основы]]
4) [[CSS#CSS Content Селекторы | Селекторы]]
5) [[CSS#CSS Content Комбинирование селекторов | Комбинирование селекторов]]
6) [[CSS#CSS Content Псевдоклассовые селекторы| Псевдоклассовые селекторы]]
8) [[CSS#CSS Content Conflict resolution | Conflict resolution]]
9) [[CSS#CSS Content Text styling | Text styling]]
10) [[CSS#CSS Content Box model | Box model]]
11) [[CSS#CSS Content Positioning | Content positiononing]]

- - -
&nbsp;
## [[CSS#Content | Полезные ссылки]]
> [!hint]- Демонстрация стилизации одного и того же сайта через CSS
> 
> www.csszengarden.com

> [!hint]- Про семейства шрифтов
> 
> http://www.w3schools.com/cssref/css_websafe_fonts.asp

- - -
&nbsp;
## [[CSS#Content | Свойства стилей]]
```CSS
.examples {

/* ШРИФТЫ */
font-size: Размер шрифта (10px, 20px, 100px...);
font-family: Шрифт ("Times New Roman");
font-style: Стиль шрифта (italic...);
font-weight: Жирность (100, 200, ..., 800, 900, bold);
text-transform: Настройки текста (capitalize, lowercase...);
text-align: Выравнивание текста (center, justify, right...);
	
/* КОРОБКА */
width: Ширина контента (НЕ коробки) (10px, 20px, 100px...);
height: Высота контента (10px, 20px, 100px...);
color: Цвет (#0000ff, blue, red, yellow...);
background-color: Фоновый цвет(Всё, что вне контента);
padding: Прокладка (10px 10px 10px 10px или просто 10px);
border: Граница (5px solid black...);
margin: Отступ (10px 10px 10px 10px или просто 10px);
margin-top: Верхний отступ;
overflow: Как быть с переполнением (auto, hidden, scroll...);

float: Float-расположение (left, right);	
	
/* ЗАДНИЙ ФОН*/
background-color: Цвет фона;
background-img: Картинка фона (url("img.png"));
background-repeat: повторяется ли картинка (no-repeat, repeat-x);
background-position: Позиция изображения (right, top left);

}
```

&nbsp;
- - -
## [[CSS#Content | Основы]]
CSS (*Cascading Style Sheets*) ассоциирует стилистические правила с HTML-элементами. Сделать такое правило довольно несложно

Правило начинается с селектора, например, если селектор `p` – тег параграфа, то правило применится ко всем параграфам в документе.

Далее в фигурных скобках стоят свойства правила, состояшие из двух частей – свойства и его значения (структура типа dict).

Например, простейшее правило, красящее все  параграфы в синий, а так же задающее их размер и ширину, будет выглядеть так:
```CSS
p {
	color: blue;
	font-size: 20px;
	width: 200px;
}
```

Набор правил называется таблицей стилей, или Stylesheet

- - -
&nbsp;

Чтобы добавить таблицу стилей в свой html-документ, нужно засунуть ее внутрь тега `<style></style>`
```HTML
<style>
p {
	color: blue;
	width: 200px;
}
</style>

```

Либо можно указать стиль напрямую в объекте через параметр `style`
```HTML
<p style="color: blue; width: 200px;"> бла-бла </p>
```

Либо указать внешнюю таблицу стилей, чтобы применять ее к разным страницам сайта
```HTML
<LINK rel="stylesheet" href="style.css">
```
&nbsp;
> [!warning]- Важно
>
>  У каждого браузера есть свои минорные CSS настройки, поэтому лучше явно переопределять всё, что может понадобиться
- - -
&nbsp;
## [[CSS#Content | Селекторы]]

Прежде всего, надо уметь задавать селекторы – потому что по похожим правилам к HTML-объектам применяется поведение описанное через JS

Существует три типа селекторов: селекторы элемента, селекторы класса и селекторы id
&nbsp;

### 1) Element selector
Как было показано выше, это просто указание имени элемента, по которому мы находим совпадение

```CSS
p {
	color: blue;
	font-size: 20px;
	width: 200px;
}
```

Такой селектор применяет стиль ко всем элементам, имеющим указанный тег

- - -
&nbsp;


### 2) Class selector

Теперь мы создаем новый класс стиля (всегда начинающийся с точки), и точно так же описываем его параметры

```CSS
.fancyobject {
	color: blue;
	font-size: 20px;
	width: 200px;
}
```

Теперь каждый элемент, который мы хотим унаследовать от этого стиля, должен иметь атрибут `class="fancyobject"` (уже без точки):
```HTML
<p class="fancyobject">Fancy paragraph</p>
```
&nbsp;
> [!hint]- Совет
>
> Чтобы объект имел несколько классов, надо перечислять их через пробел
- - -
&nbsp;

### 3) ID selector

По аналогии, id селектор применятся к объекту, у которого соответствующее значение id. Теперь служебным символом будет решетка

```CSS
#object123 {
	color: blue;
	font-size: 20px;
	width: 200px;
}
```

Этот стиль будет точечно применен к объекту с атрибутом id равным object123

- - -
&nbsp;
### 4) Star selector

Очень просто – он выбирает каждый элемент и применяет стиль ко всем объектам. Отличие от прописывания стиля в `body` заключается в том, что звезда сработает, во-первых, даже если контент лежит вне `body`, а во-вторых если свойство не наследуется. Звезда **гарантирует**, что свойство применится ко всем объектам

```CSS
* {
	color: blue;
	font-size: 20px;
	width: 200px;
}
```

- - -
&nbsp;
## [[CSS#Content | Комбинирование селекторов]]

Приятно, что для простоты мы можем группировать несколько селекторов в одном правиле, позволяя не переписывать код много раз

```CSS
div, .blue {
	color:blue;
}
```

Этот стиль будет применен как ко всем объектам `<div>`, так и ко всем объектам класса `blue`

- - -
&nbsp;

А если мы хотим, чтобы стиль применялся только к объектам `<div>`, которые имеют класс `blue`, то есть построить логическое И, нам надо указать  эти селекторы подряд без пробелов

```CSS
p.big {
	font_size: 999px;
}
```

Этот стиль будет применен только к объектам `p` с классом `big` 
- - -
&nbsp;

Также можно сделать *Child selector*. Он пишется через знак `<` и выбирает элемент в завимости от его родителя

Например
```CSS
article > p {
	color: blue;
}
```
Будет применен к тем объектам `p`, которые лежат внутри `article` (то есть селектор читается справа налеко)

&nbsp;
> [!warning]- Важно
>
> `child selector` смотрит только на папу и не смотрит на дедушек
- - -
&nbsp;

Еще один вид селекторов – *Descendant selector*. Для него пишем несколько селекторов через пробел. Он работает так же, как и *Child selector*, только ищет не только папу, но и дедушек любой вложенности

```CSS
.bigred p {
	color: blue;
}
```

Применится ко всем `<p>`, которые лежат где-то внутри представителя класса `bigred`
- - -
&nbsp;

## [[CSS#Content | Псевдоклассовые селекторы]]
Хочется не только выбирать некоторые элементы для разного стиля, а еще и менять стиль в зависимости от действий пользователя (навел мышку, щелкнул). Для этого нам пригодятся псевдоклассы.

Чтобы сделать псевдоклассовый селектор, надо указать обычный селектор, которым посвящен предыдудущий раздел, и указать один из псевдоклассов через двоеточие.

Существует множество псевдоклассов, но мы рассмотрим пять:
```CSS
:link – работает, если объект является ссылкой
```
```CSS
:visited – работает, если мы уже посетили эту ссылку
```
```CSS
:hover – работает,  если пользователь навёл мышку
```
```CSS
:active – работает, если пользователь нажал ЛКМ, но еще не отпустил 
```
```CSS
:nth-child(k) – работает только для k-го ребенка по счёту (в роли k может быть, например odd или even)     
```

- - -
&nbsp;
### Примеры
```CSS
a:hover, a:active{
	color: purple;
}
```
- Работает для ссылок, на которые мы навелись либо нажали

&nbsp;

```CSS
header li:nth-child(3){
	color: purple;
}
```
- Работает для 3-го элемента списка, находящегося где-то внутри `header`

&nbsp;

```CSS
section > div:nth-chile(odd):hover{
	color: purple;
}
```
- Работает для всех четных `div`, которые прямые дети `section` и на которые мы навелись мышкой
- - -
&nbsp;

## [[CSS#Content | Conflict resolution]]
Каскадный алгоритм – это алгоритм, который в случае чего определяет, какой из стилей нужно использовать. Вокруг каскадных алгоритмов существует много понятий, поэтому, как обычно, рассмотрим основные
- - -
- **Origin precedence** – Если стили конфликтуют, выигрывает последний объявленный. 

- **Merge** – Если конфликта нет, стили объединяются

- **Inheritance** – Стиль объекта распространяется на всех его потомков

- **Sprecifity** – Выигрывает наиболее специфичный селектор  *(см. далее)*

- - -

### Специфичность

Специфичность определяется так:
> 1) Самым специфичным считается определение внутри объекта, то есть `style=`
> 
> 2) Далее выигрывает тот стиль,  который обращается к id
> 
> 3) Далее выигрывает тот стиль, который применяется по условию псевдокласса
> 
> 4) Далее выигрывает тот стиль, в котором больше совпадающих селекторов 

 Также после указания параметра стиля можно указать `!important`. Тогда автоматически выберется именно это значение параметра в обход всем приоритетам
 
```CSS
p {
	color: green !important;
}
```
- - -
&nbsp;

## [[CSS#Content | Text styling]]
### Относительные размеры
В качестве параметра `font-size` можно указывать, во-первых, количество процентов относительно стандартного размера текста, а во-вторых указывать множитель через величину `em`, которая будет изменять размер текста в данное количество раз НЕ переопределяя предыдущий размер, а просто умножая его

```HTML
<style>
body{
	font-size: 200%;
}
</style>

<div style = "font-size: 2em"> В 4 раза больше 
	<div style = "font-size: 5em"> В 20 раза больше </div>
</div>
```
- - -
&nbsp;

## [[CSS#Content | Box model]]
В HTML каждый элемент считается коробкой. Однако помимо самого контента, коробка состоит из прокладки (padding), границы (border) и отступов (margin) (считая от центра коробки). Ширина и высота объекта считаются по размерам всей коробки.

&nbsp;

![[Pasted image 20220428003455.png]]

&nbsp;

Например, если мы напишем обычное `<body>`, Яндекс браузер по умолчанию сделает для нас `margin=8px` для читаемости текста. Но мы можем это изменить
- - -
&nbsp;

Подлость в том, что значения `width` и `height` задают размер контента, а не коробки. То есть если мы вдруг захотим поменять стиль и изменить размер границ или отступа, у нас полетит разметка.

Для этого в CSS3 добавили `border-box` для параметра `box-sizing`. Тогда коробка будет стараться уместиться в значения `width` и `height`, а не контент внутри нее

```CSS
#box{
	width: 100;
	height: 300;
	box-sizing: border-box;
}
```

Этим параметром размеров пользуется многие фреймворки, поэтому лучше просто всего ставить именно его. 

Но для этого НЕ достаточно добавить этот параметр в `body`. Он не унаследуется,
так как это один из параметров, которые не распространяются на потомков.

Но для этих нужд отлично подойдет *Star selector*. Нам поможет следующий код:
```CSS
* {
	box-sizing: border-box;
}
```

К слову, нам не очень поможет наследовать что либо от `body`, потому что если настройки браузера по умолчанию изменяют стиль с указанием типа объекта, то они победят как более специфичные
- - -

&nbsp;

### Overflow
Если размер коробки фиксирован, и с учётом прокладки, границы и отступа контент не вмещается, он будет дейтсвовать в соотстветстсвии с параметром `overflow`. По умолчанию он равен `visible`, то есть контент будет просто рисоваться поверх разметки, поэтому нужно быть аккуратнее

- - -

&nbsp;

### Cumulative margins
Важное свойство отступов в том, что складываются они только по горизонтали. По вертикали они проваливаются друг в друга

&nbsp;
![[Pasted image 20220502012213.png]]
- - -
![[Pasted image 20220502012926.png]]
- - -
&nbsp;

## [[CSS#Content | Positioning]]

### Float
 Можно задать элементу параметр `float`, после чего он, во-первых, убирается из порядка расположения объектов (то есть на место, где раньше стоял объект, можно ставить новые объекты). Во-вторых на него перестаёт работать слияние отступов
 
```CSS
#floating-box {
	float: right;
}
```

Чтобы "включить" порядок расположения для объектов, нужно применить параметр `clear`, также указав направление

```CSS
#no-floating-box {
	clear: left;
}
```

Теперь у объекта ничего не должно плавать слева, и он уедет на другую строку в обратном случае
&nbsp;
- - -
&nbsp;

Например, если  мы хотим сделать структуру из двух колонок, можем запустить в плавание влево и вправо по объекту, а дальше для выравнивания по высоте применять `clear` и слева, и справа. Для этого есть удобный параметр `both`

```CSS
another-box {
	clear: both;
}
```

&nbsp;
> [!danger]- Блядь
>
>Я почти ничего не понял
- - -
&nbsp;

### Static positioning

Дефолтный способ позиционирования для всех элементов.  