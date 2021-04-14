## ТЗ
### 1. Пожелания к прошлой версии
 > нужно обсудить 
- желательно исключить накладывание фильтров друг на друга - это создает как загруженность клиента, так и просто иногда нечитаемые данные внутри фильтра, если один из фильтров больше другого по значению и они расположены на одинаковых координатах - то меньший фильтр следует удалить/не рисовать
- поправить описание расположения текста с  leading, trailing, center на left, right, center (через enum делается вроде)
---
### 2. Общие положения

#### Новый метод расчета фильтра
- По значению (то что есть сейчас)
- По % на истории. Берем N количество дней назад на истории и сортируем наименшее значение и наибольше, среднее. В настройках будет указываться % наибольших значений. Т.е. если на протяжении прошлой недели наименьшее значение было 1, наибольшее было 1000 и мы укажем 25% в значении расчета и количество дней 7 то он покажет нам все фильтры начиная со значения 750 и выше. Соотсвествено если указать 100%  и так же 7 дней, то фильтр будет считаться со значения 1. Это необходимо для того чтобы избежать подстройки индикатора каждый раз под значения рынка - т.к. в кризис значения в моменьте очень маленькие и их много, а когда кризиса нет значений мало, но они оч большие.
---
#### 2.1 Полигоны
##### N или больше рядом стоящих по вертикали фильтров обьема обьединяются в полигон
референс на хэлп по рисованию полигонов- https://ninjatrader.com/support/helpGuides/nt8/?draw_polygon.htm
- Условия рисования : Возможность задавать максимальное количество фильтров от которого стоит скрывать полигоны (больше или равно)
- Все настройки для фильтров используются так же и для полигонов. Просто полигон рисуется от какого то количества рядом стоящих фильтров.
- 

![Два рядом стоящихфильтра с одинаковыми значениями (например 2000)](https://raw.githubusercontent.com/icesaw/mytrade/main/1.png)![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/2.png)
---
#### 2.2. POC Полигона
##### После обьединения фильтров в полигон выбирается наибольший по значению фильтр среди всех обьедененных в полигон и рисуется внутри полигона другим цветом
 
![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/3.png)
##### Визуальный пример полигона с POC
Фильтры одного обьема
![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/6.png)
##### Полигон с POC с условием рисования если подряд идут 2 или больше фильтров (1 фильтр подряд не рисуется)
![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/5.png)
#### 2.3 Градация обьемов с помощью прозрачности в одном фильтре 
*только для фильтров, не для полигона*
При установке или значения, или процента все обьемы делятся на 10 и прозрачность назначется с самого меньшего к самому большему обьему от 0.1 до 1 от установленного значения или процента.
Примерно это выглядит вот так : 
Отличие только в том что тут прозрачность меняется просто из-за того что фильтры друг на друга наложены, в Heatmap прозрачность 
![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/4.png)
### Настройки окна фильтра с пояснениями (в скобках - пояснения)
---
> Calculation Parameters *(параметры расчета)*
##### 1. Calculation Method
###### - 1.1 Volume Value *(целое значение)*
###### - 1.2 Volume Percentage *(процент от обьема прошлых дней)*
##### 2. Calculate Percentage Days Ago *(для пункта 1.2 - сколько дней назад смотреть)*
>Volume Filter 1
##### 3. Filter/Polygon 1 Volume *(значение фильтра и значение фильтра которые будут складываться в полигон)*
##### 4. Filter/Polygon 1 Bars Gap *(гэп по барам для фильтра - как в старой версии)*
##### 4. Filter 1 shape : *(как рисовать обычный фильтр, фильтр)*
###### -- 4.1 Rectangles Solid Color *(сплошная заливка цветом)*
###### -- 4.2 Rectangles Heatmap (градация прозрачности одного фильтра в зависимости от обьема)
###### -- 4.3 Polygons Solid Color (рисовать полигоны сплошным цветом)
##### 5. Polygon Min Filters Draw: - число*(минимальное количество фильтров для постройки полигона)*
##### 6. Polygon Info On (инфо внизу полигона просто сумма всех вошедних в него фильтров)
> Graphics
##### 7. Filter/Polygon Color *(цвет сплошной заливки фильтра/полигона)*
##### 8. Live Filter Color *(цвет сплошной заливки live фильтра)*
##### 9. Solid Color Opacity *(прозрачность фильтра/полигона)*
##### 10. Polygon Outline *(рисовать окантовку полигонов)*
##### 11. Polygon Outline Style *(стиль линии окантовки полигона)*
> Text
##### 12. Filter/Polygon Text On *(включение/выключение текста old + live фильтры вместе + полигоны)*
##### 13. Text position : *(расположение текста - для фильтров внутри коробки, для полигона снаружи коробки, внизу)*
###### -- 13.1 Left
###### -- 13.2 Right
###### -- 13.3 Center
##### 14. Text Color *(цвет текста фильров, полигонов)*
>Polygon POC
##### 15. Polygon POC
###### -- 15.1. Off
###### -- 15.2. Inside *(внутри полигона как фильтр)*
###### -- 15.3. Inside + Naked *(внутри полигона как фильтр, снаружи как линия)*
##### -- 15.4 Naked Only *(снаружи рисуется только линия)*
##### 16. Polygon POC Color (цвет линии и цвет текста)
##### 17. POC Info :
###### -- 17.1. Off
###### -- 17.2. Polygon Volume : POC Volume : Price : Date
##### 18. Polygon Extended Zone On (включение/выключение зон - см рисунок ниже)
##### 19. PEZ Noise Redaction (игнорирование подряд идущих баров пока зона не "закроется" об цену - в основном это будет 1-2 бара - рисунок ниже кину примерно как будет выглядеть)
##### 20. Show Broken Zones On (вклюение/выключение "отработавших" зон на графике - ниже на рисунке серым цветом)
##### 21. Broken Zones Color (цвет для "отработавших" зон)
>Alerts
##### 22. Filter Volume Alert On (только один алерт может быть включен в одно и тоже время)
##### 23. Polygon Volume Alert On
##### 24. Alert Sound

### Примеры:
**Naked POC** - это обычный POC + линия которая продолжается до тех пор пока не пересечется с графиком.

![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/7.png)
**Polygon Extention Zone** - Идея такая же как у Naked POC - только продолжается вперед зона пока не пересечется с графиком
>нужно обсудить как формулировать пересечение или касание границ или до какого-то % зоны, Чтобы легче было рисовать - есть Region Highlight Y - рисуется от времени до времени 

![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/8.png)
![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/10.png)
Игнорирование 2 баров подряд зоной из полигона
Т.е. Полигон нижне границей видит перед собой только 2 бара значит бары наверху не считаются - т.е. если хотя бы где то справа от полигона нет препятсвий - он "живой" и рисуем от него зону - рисунок кривоватый :) зона это квадратик, а стрелочки типа границы зоны (так же как и вверху все крч)
![enter image description here](https://raw.githubusercontent.com/icesaw/mytrade/main/11.png)


