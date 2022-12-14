# Директивы

Директивы это классы, которые добавляют дополнительные поведения элементам в Angular приложениях.

## Структурные директивы

Структурные директивы отвечают за HTML структуру. Они обычно добавляют, удаляют или манипулируют DOM элементами к которым присоединены.

Основные структурные директивы: NgIf, NgFor, NgSwitch.

## Ngif

Структурный директив NgIf позволяет добавить или удалить элемент в зависимости от значения предиката.

```html
<p *ngIf="isActive">Hello, World!</p>
```

Если переменная isActive равна true, то элемент `<p>` будет добавлен в DOM. Если же переменная isActive равна false, то элемент `<p>` будет удален из DOM-а (не будет добавлен).

Пример использования NgIf:

Представим что на нашем сайте пользователь может указать свою фамилию, но не обязан это делать. Тогда при отображении полного имени этого пользователя, чтобы не отрисовывать пустой элемент или элемент с неправильным значением, если пользователь не указал свою фамилию, мы можем "прикрепить" директиву \*ngIf к элементу, отображающему фамилию.

```html
<p *ngIf="lastName">Фамилия: {{ lastName }}</p>
```

Так же NgIf может использоваться с `else`.

Пример:

```html
<p *ngIf="isLoggedIn; else loggedOut">
  Добро пожаловать, друг.
</p>

<ng-template #loggedOut>
  Дорогой друг, залогинься.
</ng-template>
```

`ng-template` это виртуальные теги, они не будут отображены на странице клиента. Эти теги используются Angular-ом для реализации функционала директив.

## NgFor

Структурный элемент NgFor позволяет отобразить список элементов в DOM-е.

```html
<p *ngFor="let item of list">{{ item }}</p>
```

В коде нашего компонента находится переменная list, которая хранит в себе итерируемый элемент (например, массив или объект). NgFor создает элемент `<p>` для каждого элемента этого массива и отображает внутри тэга `<p>` его значение.

Пример использования NgFor:

Возьмем пример из нашего домашнего задания. У нас есть массив адресов и альтернативных названий иконок социальных сетей.

```js
iconList = [
    {
      src: 'assets/facebook.svg',
      alt: 'Facebook icon',
    },
    {
      src: 'assets/google.svg',
      alt: 'Google icon',
    },
    {
      src: 'assets/twitter.svg',
      alt: 'Twitter icon',
    },
    {
      src: 'assets/linkedin.svg',
      alt: 'LinkedIn icon',
    },
  ];
```

Нам нужно создать столько элементов `<img>`, сколько элементов у нас находится в этом массиве. Значения `src` и `alt` этих элементов мы должны взять из элементов массива `iconList`.

Всё это мы можем записать в одну "команду":

```html
<img *ngFor="let icon of iconList.slice(0, 10)" [src]="icon.src" [alt]="icon.alt"/>
```

Значение `src` "биндится" к значению `icon.src`, а значение `alt` к значению `icon.alt`. Мы берем "слайс" первых 10 элементов массива, чтобы у нас не "съезжала" вёрстка при слишком большом количестве иконок.

## NgSwitch

NgSwitch это структурная директива, которая очень похожа на JavaScript-овскую комманту `switch`, и позволяет нам отображать определенные элементы значение NgSwitchCase которых совпадает с значением прикрепленным к NgSwitch. Проще будет рассмотреть пример.

```html
<div [ngSwitch]="userStatus">
  <p *ngSwitchCase="'unregistered'">
    Привет, новый пользователь!
  </p>
  <p *ngSwitchCase="'registered'">
    Добрый день, зарегестрированный пользователь!
  </p>
  <p *ngSwitchCase="'admin'">
    Соизвольте с вами поздороваться, администратор сайта!
  </p>
  <p *ngSwitchDefault>
    Кто ты? Тебя не звали.
  </p>
</div>
```

Таким образом, мы можем выбрать, какой или какие элементы нам нужно отображать в зависимости от значения переменной `userStatus`.

## Атрибутивные директивы

Атрибутивные директивы прислушиваются к и модифицируют поведение других HTML элементов, аттирутов, свойств и компонентов.

Основные атрибутивные директивы: NgClass, NgStyle, NgModel.

## NgClass

Атрибутивный директив NgClass позволяет добавить или удалить один или несколько классов с элемента.

Возьмем пример, у пользователя сайта есть кошелек, на нем есть какое-то количество денег. Если это количество денег приближается к нулю, мы хотим добавить стиль на элемент, отображающий это количество, чтобы пользователь обратил внимание и пополнил кошелек.

```html
<p [ngClass]="'low-money' : isMoneyLow">
  {{ moneyAmount | currency }}
</p>
```

Значение `isMoneyLow` считается в коде компонента и возвращает boolean значение. В зависимости от этого значения на этот тэг `<p>` будет или не будет добавляться класс `low-money`.

Так же мы можем управлять одновременно многими классами через один директив NgClass. Например, мы можем хранить в компоненте объект типа `Record<string, boolean>` и "забиндить" NgClass к этому объекту. Тогда в зависимости от boolean значения будут или не будут добавляться классы с названием, заданным в ключе объекта.

Пример:

```html
В *.ts файле компонента:
classMap: Record<string, boolean> = {
  wide: true,
  red: this.isRed,
  shown: !this.isHidden,
}

В *.html файле компонента:
<h1 [ngClass]="classMap">
  Я могу быть широким, красным и видимым.
</h1>
```

`h1` в этом примере будет иметь класс `wide`, класс `red` если `this.isRed` равно `true` и класс `shown` если `this.isHidden` равно `false`. Если эти значения изменятся, то NgClass на это "отреагирует".

## NgStyle

Атрибутивный директив NgStyle позволяет стилизовать элементы "инлайн", в зависимости от значений компонента.

Например, мы можем задать стиль элемента внутри переменной в коде компонента и затем прикрепить этот стиль к элементу, используя NgStyle.

```html
*.ts

inlineStyle = {
  'font-weight': isBold ? 'bold' : 'normal',
  'color': isMoneyLow ? 'red' : 'white',
  ...
}

*.html

<p [ngStyle]="inlineStyle">{{ moneyAmount }}</p>
```

Так же мы можем менять значение стиля компонента в зависимости от значения переменных внутри компонента.

Например, мы хотим сделать полосу загрузки. Значение процента от полной загрузки мы рассчитывам внутри компонента. Оно хранится в переменной `progress` и имеет значение от `0.0` до `100.0`. Мы можем изменять размер закрашенного в зеленый цвет `<div>` элемента, чтобы воссоздать полоску прогресса.

```html
*.ts

progress = currentAmount / fullAmount * 100.0;

*.html

<div class="progress-bar-container">
  <div class="progress-bar" [ngStyle]="{'width.%': progress}">
  </div>
</div>
```

Таким образом ширина "ползунка" будет увеличиваться от 0 до 100% в зависимости от состояния прогресса.

## NgModel

Директива NgModel позволяет связать данные компонента с отображаемыми данными в двухстороннем порядке. Это значит что, если данные изменятся внутри кода компонента, то они изменятся и на экране, и, если данные изменятся пользователем на экране, в коде компонента данные изменятся вслед за этим.

(для использования NgModel нужно импортировать модуль `FormsModule`)

Например, у нас на сайте есть строка поиска. Пользователь вводит данные в эту строку и сайт выводит результаты, совпадающие с запросом. Эта строка поисSка может привязать свое значение с переменной внутри кода компонента через директиву NgModel.

```html
<label for="search-field">Search: </label>
<input [(ngModel)]="searchTerm" id="search-field">
```

## Создание собственных директивов

Angular предоставляет возможность создавать собственные директивы для упрощения необходимого вам workflow-а. Описание создания собственных директивов не входит рамки этой статьи, но посмотреть информацию на оффициальном сайте Angular:

- [Attribute directives](https://angular.io/guide/attribute-directives)
- [Structural directives](https://angular.io/guide/structural-directives)

## Заключение

Директивы в Angular позволяют удобно и быстро создавать компоненты со сложными поведениями, упрощают код компонента и обеспечивают синхронизацию модели и "view". Знание, понимание и грамотное использование директивов упростит жизнь и повысит компетенцию любому Angular разработчику.

**Спасибо за внимание!**
