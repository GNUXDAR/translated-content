---
title: Стилизация таблиц
slug: Learn_web_development/Core/Styling_basics/Tables
---

{{LearnSidebar}}{{PreviousMenuNext("Learn/CSS/Styling_boxes/Borders", "Learn/CSS/Building_blocks/Debugging_CSS", "Learn/CSS/Styling_boxes")}}

Стилизация HTML таблиц это не самая гламурная работа в мире, но иногда нам нужно это делать. Эта статья руководство как сделать, чтобы ваши HTML таблицы выглядели хорошо, с некоторыми свойствами подробно рассмотренными в предыдущих статьях.

| Необходимые знания: | Базовый HTML ([Введение в HTML](/ru/docs/conflicting/Learn_web_development/Core/Structuring_content)), HTML таблицы (смотрите раздел HTML таблицы (TBD)), и представление о том как работает CSS ([Введение в CSS](/ru/docs/conflicting/Learn_web_development/Core/Styling_basics)). |
| ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Цель:               | Научиться эффективно стилизовать HTML таблицы.                                                                                                                                                                                                                                       |

## Типичная HTML таблица

Давайте начнём с рассмотрения типичной HTML таблицы. Когда мы говорим о примерах типичных HTML таблиц обычно речь идёт о обуви, погоде или сотрудниках; мы решили сделать это более интересным создав таблицу о знаменитых панк группах Великобритании. Разметка выглядит следующим образом:

```html
<table>
  <caption>
    A summary of the UK's most famous punk bands
  </caption>
  <thead>
    <tr>
      <th scope="col">Band</th>
      <th scope="col">Year formed</th>
      <th scope="col">No. of Albums</th>
      <th scope="col">Most famous song</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">Buzzcocks</th>
      <td>1976</td>
      <td>9</td>
      <td>Ever fallen in love (with someone you shouldn't've)</td>
    </tr>
    <tr>
      <th scope="row">The Clash</th>
      <td>1976</td>
      <td>6</td>
      <td>London Calling</td>
    </tr>

    ... some rows removed for brevity

    <tr>
      <th scope="row">The Stranglers</th>
      <td>1974</td>
      <td>17</td>
      <td>No More Heroes</td>
    </tr>
  </tbody>
  <tfoot>
    <tr>
      <th scope="row" colspan="2">Total albums</th>
      <td colspan="2">77</td>
    </tr>
  </tfoot>
</table>
```

Таблица размечена, немного стилизована и понятна, благодаря использованию таких свойств как [`scope`](/ru/docs/Web/HTML/Element/th#scope), {{htmlelement("caption")}}, {{htmlelement("thead")}}, {{htmlelement("tbody")}} и т.д. К сожалению при просмотре в браузере она не очень хорошо выглядит (посмотреть можно здесь [punk-bands-unstyled.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-unstyled.html)):

![](table-unstyled.png)

Это выглядит достаточно грубо, трудно читаемо и скучно. Нам нужно использовать немного CSS чтобы все исправить.

## Активное обучение: Стилизация таблицы

В этой части обучения мы будем работать над тем чтобы стилизовать наш пример таблицы.

1. В начале необходимо сделать копию [sample markup](https://github.com/mdn/learning-area/blob/master/css/styling-boxes/styling-tables/punk-bands-unstyled.html), загрузить оба изображения ([noise](https://github.com/mdn/learning-area/blob/master/css/styling-boxes/styling-tables/noise.png) и [leopardskin](https://github.com/mdn/learning-area/blob/master/css/styling-boxes/styling-tables/leopardskin.jpg)), и вставить эти файлы в отдельную папку на вашем компьютере.
2. Следующее, это создать новый файл `style.css` и сохранить его в той же папке вместе с другими файлами.
3. Подключить CSS в HTML для этого разместить следующую строку в HTML внутри {{htmlelement("head")}}:

   ```html
   <link href="style.css" rel="stylesheet" type="text/css" />
   ```

### Отступы и разметка

Первое что нам нужно это разобраться с отступами/разметкой, так как по умолчанию стилизация таблцы выглядит неразборчиво! Сделаем это, добавив CSS в ваш `style.css` файл:

```css
/* Отступы */

table {
  table-layout: fixed;
  width: 100%;
  border-collapse: collapse;
  border: 3px solid purple;
}

thead th:nth-child(1) {
  width: 30%;
}

thead th:nth-child(2) {
  width: 20%;
}

thead th:nth-child(3) {
  width: 15%;
}

thead th:nth-child(4) {
  width: 35%;
}

th,
td {
  padding: 20px;
}
```

Наиболее важные части следующие:

- Свойство {{cssxref("table-layout")}} со значением `fixed` как правило полезно использовать для вашей таблицы, это делает поведение таблицы немного более предсказуемым, чем значение по умолчанию. Обычно столбцы таблицы имеют размер в зависимости от того сколько в них контента, что приводит иногда к некоторым странным результатам. Когда `table-layout: fixed`, размер ваших столбцов определяется шириной их заголовков и делает их контент соответствующего размера. Вот почему вы выбрали четыре разных заголовка с помощью селектора `thead th:nth-child(n)` ({{cssxref(":nth-child")}}) ("Выберите _n-ый_ дочерний элемент {{htmlelement("th")}} в последовательности, внутри элемента {{htmlelement("thead")}}") и задать им заданную в процентах ширину. Ширина колонки соответствует ширине её заголовка, это правильное решение при определении размеров колонок таблицы. Крис Койер (Chris Coyier) более подробно рассматривает эту технику в статье [Fixed Table Layouts](https://css-tricks.com/fixing-tables-long-strings/).

  Мы также использовали {{cssxref("width")}} 100%, что означает, что таблица заполнит любой контейнер и будет отзывчивой (хотя для этого потребуется ещё некоторая работа для правильного отображения на экранах небольших размеров).

- Свойство {{cssxref("border-collapse")}} со значением `collapse` это стандартная практика при стилизации любой таблицы. По умолчанию, когда вы задали рамки для элементов таблицы, все они будут иметь пробелы между собой, как показано на рисунке ниже: ![](no-border-collapse.png)Это не очень хорошо выглядит (хотя может это то что вам нужно, кто знает?). Если установить `border-collapse: collapse;` рамки схлопываются в одну и так выглядит намного лучше: ![](border-collapse.png)
- Мы установили {{cssxref("border")}} вокруг всей таблицы, это понадобится когда чуть позже мы будет устанавливать рамки вокруг header и footer таблицы — когда по периметру всей таблицы нет рамки и граница заканчивается просто отступом, таблица выглядит странно и разрозненно.
- Мы установили {{cssxref("padding")}} на элементах {{htmlelement("th")}} и {{htmlelement("td")}} — это создаёт в талице воздух, который позволяет ей дышать, делая её более понятной.

На этом этапе наша таблица выглядит уже гораздо лучше:

![](table-with-spacing.png)

### Немного простой типографики

Теперь мы ещё кое-что изменим.

Во-первых, мы пойдём и найдём на [Google Fonts](https://www.google.com/fonts) шрифт который подходит в нашей ситуации с таблицей о панк группах. Вы можете можете выбрать для себя другой шрифт если захотят, тогда вам понадобится заменить представленный {{htmlelement("link")}} элемент и изменить объявление {{cssxref("font-family")}} на выбранный вами Google Fonts шрифт.

Добавьте элемент {{htmlelement("link")}} в блок head вашего HTML, на строчку выше существующего элемента `<link>`:

```html
<link
  href="https://fonts.googleapis.com/css?family=Rock+Salt"
  rel="stylesheet"
  type="text/css" />
```

Затем добавьте следующий CSS в ваш `style.css` файл, ниже предыдущего кода:

```css
/* Типографика */

html {
  font-family: "helvetica neue", helvetica, arial, sans-serif;
}

thead th,
tfoot th {
  font-family: "Rock Salt", cursive;
}

th {
  letter-spacing: 2px;
}

td {
  letter-spacing: 1px;
}

tbody td {
  text-align: center;
}

tfoot th {
  text-align: right;
}
```

Здесь нет ничего специально для таблиц, мы просто настраиваем стилизацию шрифтов, чтобы упростить чтение:

- Мы установили доступный глобально шрифт sans-serif; это вполне стандартный стилистический выбор. Мы установили выбранный нами шрифт для заголовков внутри элементов {{htmlelement("thead")}} и {{htmlelement("tfoot")}}, который подходит нам по тематике панков.
- Мы добавили немного {{cssxref("letter-spacing")}} в заголовках и ячейках которым необходимо добавить читаемости. Опять же это основной стилистический приём.
- Мы выравниваем по центру текст ячейках внутри {{htmlelement("tbody")}} чтобы они совпадали с заголовками. По умолчанию у ячеек свойство {{cssxref("text-align")}} имеет значение `left`, а заголовки значение `center`, но обычно выглядит лучше если они выравниваются в одном стиле. По умолчанию, чтобы внешний вид заголовков отличался у них задан жирный шрифт.
- Мы выровняли заголовок справа внутри {{htmlelement("tfoot")}} так чтобы он визуально ассоциировался с соответствующими ему данными.

В результате таблица выглядит немного аккуратнее:

![](table-with-typography.png)

### Графика и цвета

И наконец-то графика и цвета! Наша таблица заполнена тем что имеет отношение к панкам, поэтому нам нужно придать ей яркий впечатляющий вид. Не беспокойтесь, вам не обязательно делать таблицу слишком кричащей — вы можете выбрать что-то более утончённое и со вкусом.

Начнём с добавления в конец файла `style.css` следующего CSS:

```css
/* Графика и цвета */

thead,
tfoot {
  background: url(leopardskin.jpg);
  color: white;
  text-shadow: 1px 1px 1px black;
}

thead th,
tfoot th,
tfoot td {
  background: linear-gradient(to bottom, rgb(0 0 0 / 10%), rgb(0 0 0 / 50%));
  border: 3px solid purple;
}
```

Опять же здесь нет ничего конкретно для таблиц, но стоит отметить несколько вещей.

Мы добавили {{cssxref("background-image")}} в {{htmlelement("thead")}}, {{htmlelement("tfoot")}} и изменили {{cssxref("color")}} для всего текста внутри header и footer на белый (и ещё {{cssxref("text-shadow")}}) для лучшей читаемости. Вы должны всегда быть уверены что ваш текст хорошо контрастирует с фоном, для обеспечения читаемости.

Также мы добавили линейный градиент для {{htmlelement("th")}} и {{htmlelement("td")}} элементов внутри header и footer для придания лёгкой приятной текстуры, а также установили этим элементам яркие пурпурные границы. Полезно иметь несколько вложенных элементов, это позволяет накладывать несколько стилей друг на друга. Да, мы могли бы установить и фоновое изображение, и линейный градиент на {{htmlelement("thead")}} и {{htmlelement("tfoot")}} элементы используя множественные фоновые изображения, но мы решили сделать это отдельно для старых браузеров, которые не поддерживают [несколько фоновых изображений](/ru/docs/Web/CSS/CSS_backgrounds_and_borders/Using_multiple_backgrounds) и [линейные градиенты](</ru/docs/Web/CSS/gradient/linear-gradient()>).

#### Полосатая зебра

Мы хотели бы посвятить целый раздел, чтобы показать вам как реализовать **полосы зебры** — чередующиеся цветные строки которые упрощают чтение разных строк в вашей таблице. Добавим следующий CSS в ваш `style.css` файл:

```css
/* Полосатая зебра */

tbody tr:nth-child(odd) {
  background-color: #ff33cc;
}

tbody tr:nth-child(even) {
  background-color: #e495e4;
}

tbody tr {
  background-image: url(noise.png);
}

table {
  background-color: #ff33cc;
}
```

- Ранее вы видели как {{cssxref(":nth-child")}} селектор использовался для выбора специфичных дочерних элементов. В качестве параметра также может быть передана формула, тогда он будет выбирать последовательность элементов. Так формула `2n-1` выберет все нечётные дочерние элементы (1, 3, 5 и т.д.), а формула `2n` выберет все чётные (2, 4, 6 и т.д.). Мы использовали в нашем коде ключевые слова `odd` и `even`, которые делают тоже самое что и формулы выше. В данном случае мы устанавливаем чётным и нечётным строкам разные (яркие) цвета.
- Ещё мы добавили повторяющийся плиткой фон ко всем строкам тела таблицы, который добавляет немного шума (полупрозрачный `.png` с небольшим количеством визуальных искажений на нем), чтобы получилась некоторая текстура.
- И наконец мы установили для таблицы сплошной цвет фона, который обеспечит фон строкам таблицы в том случае если браузер не поддерживает селектор `:nth-child`.

Этот взрыв цвета выглядит следующим образом:

![](table-with-color.png)

То что получилось может быть не в вашем вкусе, но основная идея была в том, что мы попытались сделать таблицу которая не будет скучной и академической.

### Стилизация заголовка

Последнее что мы сделаем с нашей таблицей это стилизация заголовка. Для этого добавим следующие строки в наш файл `style.css`:

```css
/* Заголовок */

caption {
  font-family: "Rock Salt", cursive;
  padding: 20px;
  font-style: italic;
  caption-side: bottom;
  color: #666;
  text-align: right;
  letter-spacing: 1px;
}
```

Здесь нет ничего особенного, кроме свойства {{cssxref("caption-side")}}, которое имеет значение `bottom`. В этом случае заголовок будет размещён внизу таблицы и это вместе со всем остальным обеспечивает нашей таблице окончательный вид (можно посмотреть по ссылке [punk-bands-complete.html](https://mdn.github.io/learning-area/css/styling-boxes/styling-tables/punk-bands-complete.html)):

![](table-with-caption.png)

## Активное обучение: Стилизация вашей собственной таблицы

Теперь мы хотим, чтобы вы взяли наш пример таблицы (или использовали собственный!) и сделали что-то значительно более стильное и менее безвкусное чем наша таблица.

## Стилизация таблицы быстрые советы

Короткий список наиболее полезных вещей рассмотренных выше:

- Сделайте свою разметку простой и гибкой, например, используя для этого проценты, что сделает дизайн более отзывчивым.
- Используйте {{cssxref("table-layout")}}`: fixed` для более понятного поведения разметки, при этом легко установить ширину столбцов, установив ширину {{cssxref("width")}} для заголовков таблицы ({{htmlelement("th")}}).
- Используйте {{cssxref("border-collapse")}}`: collapse`, которое схлопнет границы элементов таблицы, что обеспечит аккуратный внешний вид.
- Используйте {{htmlelement("thead")}}, {{htmlelement("tbody")}} и {{htmlelement("tfoot")}} чтобы разбить вашу таблицу на логические фрагменты и предоставив таким образом дополнительные точки для применения CSS, это даёт возможность накладывать стили друг на друга, если это необходимо.
- Используйте полоски зебры, чтобы облегчить чтение между строк.
- Используйте {{cssxref("text-align")}} чтобы выровнять текст в {{htmlelement("th")}} и {{htmlelement("td")}} для более аккуратного и удобного оформления.

## Заключение

Несмотря на головокружительные успехи достигнутые в стилизации таблиц, у нас есть ещё кое-что чем мы можем занять наше время. В следующей главе мы рассмотрим некоторые продвинутые эффекты, уже устоявшиеся (например, тени box shadows) и те которые только недавно появились в браузерах, такие как режимы наложения blend-mode и фильтры.

{{PreviousMenuNext("Learn/CSS/Styling_boxes/Borders", "Learn/CSS/Building_blocks/Debugging_CSS", "Learn/CSS/Styling_boxes")}}

## В этом блоке

- [Box model recap](/ru/docs/Learn_web_development/Core/Styling_basics/Box_model)
- [Backgrounds](/ru/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)
- [Borders](/ru/docs/Learn_web_development/Core/Styling_basics/Backgrounds_and_borders)
- [Стилизация таблиц](/ru/docs/Learn_web_development/Core/Styling_basics/Tables)
- [Advanced box effects](/ru/docs/Learn_web_development/Core/Styling_basics/Advanced_styling_effects)
- [Creating fancy letterheaded paper](/ru/docs/Learn/CSS/Building_blocks/Creating_fancy_letterheaded_paper)
- [A cool looking box](/ru/docs/Learn/CSS/Building_blocks/A_cool_looking_box)
