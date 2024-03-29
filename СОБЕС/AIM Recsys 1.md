# Рекомендательные системы
#ml #math #ozon #course | [[ml]] [[ozon]] [[math]] [[course]]
Курс AIM

- - -

&nbsp;

## Введение
*Релевантность* – мера того, насколько хорошо товар удовлетворяет потребности клиента **на данный момент** (время это тоже важный параметр релевантности)
Чем выше релевантность предлагаемой нами штуки, тем мы большие молодцы  и тем чаще её купят

В широком смысле рекомендательной системой является много что. Но в нашем более строгом смысле рекомендательные системы строят вокруг
1) *Контента*
	Фильмы, музыка, книги
2) *Товаров*
	Товары)))
3) *Cобытия*
	Концерты, экскурсии, мероприятия

&nbsp;

### Данные о пользователе
Для того чтобы что-то рекомендовать, нужно знать о клиенте какие-то данные. *Сбор данных* бывает *явный*. Это когда покупатель явно показал нам своё предпочтение (закинул товар в избранное, купил его, поставил оценку, предпочел один товар другому) и *неявный*, когда мы строим догадки. Например, будет полезна история просмотра товаров, история корзины, в целом лог поведения. Так же хорошей подсказкой могут быть ip-адрес или параметры экрана пользователя (пользователи айфонов, компьютеров и андроидов хотят разные вещи)

Что в целом мы можем узнать о клиенте? Как говорилось, можем по тем или иным *признакам* знать
1) Устройство, браузер, размер экрана
2) Регион, локацию
3) Пол
4) Возраст

Из *поведенческих факторов* могут быть полезны
1) Просмотренные страницы
2) Время, проведенное на сайте и на тех или иных товарах
3) Клики

Ну и самое очевидное – *обратная связь*
1) Рейтинг
2) Отзывы
3) Жалобы
4) Лайки
5) Покупки

&nbsp;

### Товары и рекомендации
А что насчёт товаров? Есть разные типы рекомендуемых позиций
1) *Заменители* (alternative) – вместо сникерса советовать марс
2) *Сопутствующие товары* (cross sell) – к телефону можно советовать чехол
3) *Бандлы* – объединить чипсы, колу и сигареты в один набор (надо понимать что с чем объединять)
4) *Более дорогие товары* (Up sell) – предлагать айфон после просмотра xiaomi
5) *Популярные товары* (best sellers) – просто велика вероятность покупки

Способ создания рекомендаций бывает *персональный* или *неперсональный*. Также рекомендации могут долго и дотошно высчитываться в кеше, а потом советоваться, это *оффлайн* рекомендация, либо расчитываться на ходу, то есть *онлайн*

Также есть различные *каналы рекомендации* – по почте мы будем советовать по-другому, чем на сайте

&nbsp;

Рекомендации бывают нескольких видов
1) *Коллаборативная фильтрация* – основывается на поведении похожих пользователей
2) *Основанная на контенте* – использовать информацию о продукте (название, жанр фильма, режиссер) и строим какую-нибудь оптимизирующую модель типа регрессии
3) *Основанная на знаниях* – используем знания из реальной жизни, например из физического смысла очевидно, что к телефону нужно советовать чехол. Как правило более ручной подход, потому что требует в алгоритм введения множества бытовых понятий
4) *Гибридные*

&nbsp;


### Формализация
 пусть $U$ – множество пользователей, $I$ – множество товаров/страниц/новостей, а $r_{ui}$ – пользователь совершил действие с товаром (просмотрел, прослушал, купил, сохранил)

Для каждого пользователя мы должны выдать оценку ресурсам $r_{ui}$ и выдать ранжированный список рекомендуемых товаров, а для каждого товара найти похожие с ним

Этот подход поиска близости можно обобщить как на анализ текстов, так и на изображения, и в общем виде, если понимать под $U$ – объекты, под $I$ – субъекты, а под $R$ – матрицу близости одного к другому, задача всегда стоит в восстановлении этой матрицы $R$ и поиске похожим объектов/субъектов

Какие тривиальные алгоритмы приходят в голову?
1) Советовать популярные товары, хиты продаж
	Чтобы выдать популярные фильмы, можно ввести некоторую *меру популярности*, например среднюю оценку, или медиану, или взвешенное среднее по числу голосов или что-то еще

1) Недавно покупали/смотрели
   Чтобы выдать недавно просмотренные фильм, просто отсекаем, скажем, последнюю неделю, и в рамках нее смотрим частоту просмотра (можно с учётом рейтинга)

3) Дать экспертам вручную сопоставить зависимости
	Для эксперта ничего придумывать не надо, он всё сопаставит сам (наушники к телефону, чипсы к пиву, картриджы к принтеру)

1) Полезные с точки зрения бизнеса товары (с большой маржой)
	Берём бизнес метрику и сортируем по ней

1) Рекомендовать брошенные корзины и незаконченные покупки
	Для каждого пользователя считаем популярность (давность, время просмотра) товара и сортируем по ней

1) Метч по рейтингу или **"пользователи покупавшие этот товар часто покупают"**
	> [!summary]- Метч по рейтингу
	> 
	>  Для пользователя $u_0$, если он покупал товар $i_0$:
	>  
	>  1. Находим $U(i_0) = \{u \in U\;|\;r_{ui_0)}>0,\;u \neq u_0\}$ – множество пользователей, также покупавших этот товар
	>  
	>  3. Находим $I(i_0) = \{i \in I\;|\;sim(i,i_0)\geq s\}$ – все товары, близкие к $i_0$ по мере $sim$ на множестве $U(i_0)$
	>  
	>  5. Сортируем эти товары по мере сходства
	>  
	>  Как выбрать меру сходства товаров $sim$? классический подход следующий $$sim(i, i_0) = \frac{|U(i_0)\cap U(i)|}{|U(i_0) \cup U(i)|}$$
	>  
	>  Какие у этого могут быть минусы? Во-первых, такие рекомендации тривиальны и не учитывают интересы пользователя (всего лишь одну покупку). А еще нам нужно хранить всю матрицу $R$, что в некоторых случаях непозволительная роскошь
	>  
	>  Но алгоритм простой, и для некоторых групп товаров получаем хороший результат, особенно для маленьких магазинов

1) Метч по признаку или **"пользователи из вашего города часто покупают"**

- - -

&nbsp;

## Коллаборативная фильтрация

Существует два вида коллаборативной фильтрации – *User based* и *Item based*, в первом случае ищется близость между пользователями на основе их оценок товаров, и пользователю предлагаются рекомендации того, что выбирали похожие на него, а во втором случае ищется близость между товарами на основе того что поставили пользователи.

В user-based случае алгоритм следующий
1) Находим коллаборацию пользователей, похожих на данного
3) Среди этих пользователей находим самые рейтинговые товары
4) Предлагаем их

Теперь рекомендации не такие тривиальные, учитывается весь спектр интересов пользователя, но нам всё еще нужно хранить всю матрицу рейтингов и нам нечего рекомендовать вуйгвым (новым) пользователям

В item-based случае алгоритм следующий
1) Находим все достаточно близкие товары к хотя бы одному из тех, что покупал пользователь
2) Сортируем их по рейтингу

Тут рекомендации тривиальны, потому что опираемся на один товар и не объединяем пользователей в коллаборации, надо хранить всю матрицу. Но теперь нам есть что предложить холодному пользователю, так как у всех товаров есть рейтинг, и мы можем предлагать самые популярные

&nbsp;

Также существует два принципиально разных подхода – *Memory based* подходы и *Латентные модели*. В первом случае хранится вся матрица $R$. Тогда понятно как искать сходство товаров и пользователей. Во втором случае не хранится вся матрица, а вместо этого хранится некоторое векторное представление товаров и пользователей (эмбеддинги). Мы храним именно эти профили, которые агрегируют информацию из матрицы, и ищем сходство уже внутри них

&nbsp;

Как заполнять пустые значения матрицы? Можно воспользоваться *регрессией Надарая-Ватсона* с некоторым ядром $K(u, u')$
$$r_{ui} = \bar{r_{u}} + \frac{\sum_{u'} K(u,u')(r_{u'i} -\bar{r_{u'}})}{\sum_{u'} K(u,u')}$$
где $\bar{r_u}$ – средний рейтинг пользователя $u$

То есть в этой формуле мы берем среднее значение по всем юзерам, и уточняем его для каждого конкретного товара (берем ядро aka степень близости нашего товара со всеми теми что покупал наш пользователь и умножаем на отклонения от среднего оценки этих товаров). По сути первое слагаемое это общий масштаб, а второе это разброс

Эта функция сглаживает результат без перепадов и сохраняет масштаб

Всё что мы пока обсуждали это memory-based подходы, а значит использующие всю матрицу. Их легко понять и реализовать, но считается и хранится она очень дорого, а также остается проблема холодного старта

&nbsp;

### Метод ближайших соседей
Рассмотрим метод на примере следующей матрицы и попытаемся угадать оценку Борисом фильма "Легенда №17"

![[Pasted image 20221014002911.png]]

C точки зрения user-based подхода нам надо найти его соседей (допустим, двух). Тут можно вводить разные метрики, но очевидно, что больше всего на него похожи Алексей и Юля. У них оценки этого фильма 6 и 7, и мы либо предсказываем среднее, либо взвешиваем его в пользу Алексея, так как он похож сильнее  

![[Pasted image 20221014003148.png]]

А что насчет item-based? Тут все с точностью до транспонирования. Находим фильмы, сильнее всего похожие на "Легенду" (допустим, ищем одного). Судя по всему, ближайший сосед это "Три мушкетера", значит ожидаем от Бориса такую же оценку, то есть 9 

Как определять близость? Можно считать количество совпавших оцененных фильмов (и не обязательно с совпадающими оценками), ведь если мы смотрим одни и те же фильмы, то это уже говорит о многом. Можно считать число совпавших или близкое к тому оценок, вычислять корелляцию Пирсона или косинусное расстояние...

Еще хорошим вариантом будет не просто брать за близость количество совпавших оценок (проблема очевидна, некоторые оценки могли совпасть чисто слуачйно, и мы будем считать пользователей похожими только по ним, хотя расхождения у них гораздо массивнее совпавших оценок), а брать **вероятность того, что это совпадение не случайно**. Если считать что покупатели делают выбор независимо, то эта вероятность явно и комбинаторно выписывается и называется *точным тестом Фишера* 

&nbsp;

### Проблема холодного старта
Что делать с новыми пользователями, или с незалогинившимися, или сидящими с VPN? Например, можно использовать средний рейтинг товаров (то есть как бы считать коллаборантами всех пользователей). Интересный вариант это создание специальной сущности "новый пользователь" и построение оценок для неё (например, "новый пользователь" часто покупает телефон). Ну  или использовать гибридные алгоритмы

&nbsp;

### Проблема длинного хвоста
Принцип того, что 20% популярных товаров делают 80% выручки. Остальные товары смотрятся раз-два и их большинство. Они-то и являются длинным хвостом. На длинном хвосте мало статистике, и алгоритмы работают плохо

Как вариант, можем объединять редкие товары в кластеры. Например, не черные/синие/красные ручки по одиночке считать очень сложно, но если при подсчёте назвать их "цветные" ручки, то суммарная статистика будет куда более обширная и по ней можно будет строить предположения

&nbsp;

### Проблема первых оценщиков
Представим, что мы запустили новый сервис музыки. Что если большинство первых пользователей (бета-тестеры) смещены в сторону джаза? Теперь мы начинаем рекомендовать новым пользователям джаз, загоняя наше приложение в этот порочный круг

Можно либо иногда предлогать музыку другого жанра, чтобы смотреть оценку по ней, либо, например, самим предлагать человеку "поэкспериментировать", как это делает яндекс музыка. Еще хорошей идеей будет сделать несколько разных тестовых выборок и смотреть, насколько их друг от друга разносит. Если сильно, значит они вряд ли представляют мнение большинства и смещены

- - -

&nbsp;

## Матричные разложения
Можно ли приближенно разложить разреженную матрицу $R$ на две матрицы маленьких размерностей (скажем, толщины $d$) для пользователя и товаров? Да, такое разложение называется *разложением Функа* aka *Funk MF*

![[Pasted image 20221014130722.png]]

Толщина $d$ – это множество *латентных* (aka скрытых) *факторов* пользователей и фильмов соответственно. 

Это матричное разложение можно получить из SVD. Пусть имеется сингулярное разложение $$R \approx \bar{R} = \bar{X}\bar{\Sigma}\bar{Y}$$
Где множители нового "SVD" получены усечением матриц $X$ и $Y$ до длины/ширины $d$

Тогда если $P = (\bar{X}\bar{\Sigma})^*$, а $Q = \bar{V}^*$, то $$\bar{R} =P^*Q$$
Тогда поэлементно наша матрица записывается так $$\bar{r_{ij}} = q_i^Tp_u$$
Для улучшения Funk MF можно использовать средний рейтинг $\mu$, а так же сдвиг по пользователю и по товару $b_u, b_i$ $$\hat{r_{ui}} = \mu + b_u + b_i + q_i^Tp_u$$
&nbsp;

### Оптимизация
Понятно что с увеличением $d$ улучшается персонализация и матрица уточняется, однако во-первых нам нужно хранить больше данных, а во-вторых с какого-то момента матрица начинает переобучаться. Классической задачей оптимизации для построенного разложения $\bar{R} = UV$ является$$argmin(\|R - \bar{R}\|_F)$$
Однако для борьбы с переобучением имеет смысл ввести регуляризацию $$\arg\min\limits_{U,V}(\|R - \bar{R}\| + \alpha\|U\| + \beta\|V\|)$$
Эту оптимизацию можно делать благодаря простейшему *SGD*  или благодаря *ALS* (Altering Least Squares) – это когда мы фиксируем P или Q, и пытаемся найти оптимум по P или Q. Оно находится аналитически в явном виде

> [!summary]- NNMF
Особенность SVD в том, что оно плохо работает, если значения в матрице бинарны (0,1). С этой целью был придуман *NNMF* (Non-negative matrix factorization) 

&nbsp;

### SVD++
Представим, что у нас есть дополнительный набор факторов $y_i$ для каждого пользователя, который будет описывать то, что пользователь посмотрел, но не оценил. Тогда модифицируем нашу последнюю запись, изменив матрицу $p$
$$\hat{r}_{ui} = \mu + b_u + b_i + q_i^T(p_u + \frac{\sum y_k}{\sqrt{|V(u)|}})$$
Здесь мы нормируем на корень из мощности $V(u)$ – множества просматриваемых продуктов. Таким образом мы контролируем дисперсию и подстраиваемся под то, что у разных людей разное количество покупок/просмотров

> [!summary]- timeSVD++ 
> Как ни странно, то же самое, только теперь все элементы матриц зависят от времени $t$ и представляют собой $$b_i(t) = b_i + b_{i,t}$$

&nbsp;

### Регрессия по признакам
Попытка посмотреть на SVD под другим углом – сделаем регрессионную модель, где значения $b_{user}$ и $b_{item}$ зависят от некоторого дополнительного набора переменных $x_{u}$ и $x_i$ соответственно. Тогда модель выглядит так
$$\hat{r_{ui}} \sim \mu + b_{user}(x_u) + b_{item}(x_i) + q_i^Tp_u(t)$$
Где $$b_{user}(x_u) \sim N(\alpha(x_u), \sigma^2_\alpha)$$$$b_{item}(x_i) \sim N(\beta(x_i), \sigma^2_\beta)$$
А, альфа/бета есть любые регрессии

&nbsp;

### NMF (Non-negative matrix factorization)
Как понятно из названия, это группа алгоритмов, в которых матрица $V$ приближенно раскладывается на две (или больше) матрицы $W$ и $H$ так, чтобы они имели неотрицательные значения при условии, что у исходной матрицы все значения неотрицательны

В общем случае эта задача NP-трудна, то есть требует полного перебора. Однако мы можем потребовать приближенное решение, сформулировать его через задачу оптимизации, причем разными способами. Например$$\|V - WH\|^2 \rightarrow \min$$
$$W,H \geq 0$$
или$$D(V||WH) \rightarrow \min$$
$$W,H \geq 0$$
- - -
&nbsp;

## Задачи ранжирования

Мы знаем метрики качества для регрессии MAE и RMSE, accuracy / precision / recall для классификации

*Задача ражнирования*  это тип задач обучения с учителем, где по выборке мы строим ранжирующую модель, которая определяет отношение порядка между элементами. Это по сути задача сортировки списка с точки зрения нашей метрики (накрутили нейронки на сортировку)

Можем решать задачи ранжирования *поточечно* (то есть смотреть на каждую позицию отдельно), *попарно* (то есть оценивать ошибку по количеству неправильно переставленных выдачей) или *списочно* (то есть оценивать метрики по всему списку сразу)

&nbsp;

### Метрики ранжирования

Обозначим релевантность объекта за $rel(k)$. Это бинарная функция, которая единица, если объект релевантен. $u$, как обычно, пользователи.

&nbsp;

>[!summary]- precision
Напомним что такое *precision*
>
>$$
>precision = \frac{|S_{rel}\cap S_{retr}|}{|S_{retr}|}
>$$
>где $S_{rel},\;S_{retr}$ – релевантные и показанные объекты соответственно. Короче говоря, доля релевантных объектов из показанных.

>[!summary]- recall
>*recall* – это доля показанных релевантных среди всех релевантных (насколько большую часть релевантных мы показали)
>$$
>precision = \frac{|S_{rel}\cap S_{retr}|}{|S_{rel}|}
>$$

&nbsp;

>[!tip]- MAP@n
>Мы показываем пользователю $n$ объектов. *Precision@n* есть доля релевантных объектов из первых n объектов. Для одного пользователя считается как 
>$$
>Precision@n = \frac{|S^n_{rel}\cap S^n_{retr}|}{n}
>$$
>Её можно круто развить до следующей метрики 
>$$
>AvgPrecision@n = \frac{1}{n}\sum\limits_{k=1}^nPrecision@k\cdot rel(k)
>$$
>Эта величина учитывает порядок элементов, что очень нужно и желаемо.
>Заметим, что это всё еще характеристика **одного** пользователя. Чтобы агрегировать результат по всей матрице, используется *Mean Average Precision*, или *MAP*
>$$
>MAP@n = \frac{1}{|U|}\sum AvgPrecision@n(u_{i})
>$$



>[!tip]- Recall@n
>
*Recall@n* это то, какая доля релевантных объектов из показанных относительно общего числа релевантных объектов. Для одного пользователя считается так
$$Recall@n(u) = |\sum\limits_{i=1}^nrel(i, u) > 0|$$
А потом для всей матрицы усредняем так $$AvgRecall@n=\frac{1}{|U|}\sum Recall@n(u_{i})$$

>[!tip]- MRR@n
>
>Усредняем обратные велиичины к рангам первых найденных релевантных объектов (то есть если первый объект релевантент то присваем ему 1, если второй то 1/2, если третий то 1/3)
>$$
>MRR = \frac{1}{|U|}\sum \frac{1}{rank(u_{i})}
>$$

>[!tip]- nDCG@n
>
>*Discounted Cumulative Gain* aka *DCG@n* Популярная метрика, учитывающая и порядок, и количество релевантных объектов
>$$
>DCG@n = \sum\limits_{k=1}^n\frac{rel(k)}{\log_{2}(k+1)}
>$$
>Складываем все встреченные релевантности и "штрафуем" на всё меньший вес (уменьшается логарифмически)
>
Понятно, что эта величина никак не больше *идеального* $DCG$, где каждая вылача релевантна
$$IDCG@n = \sum\limits_{k=1}^n \frac{1}{\log_{2}(k+1)}$$
Тогда нормируем на нее и получим *нормированный* $DCG$ aka $nDCG$
>$$
>nDCG@n = \frac{DCG@n}{IDCG@n}
>$$