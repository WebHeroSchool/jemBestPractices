# JavaScript Лучшие практики

## 1) Не объявляйте строки, числа или булевые типы как Object.
Хорошая практика использовать строки, числа и булины как примитивы. Объявление их как объектов может приводить к side эффектам, и замедлению исполения кода.

```javascript 
// Плохая практика 👎 
let name = new String('Jhon');
// Хорошая практика 👍
let name = 'John';
```
## 2) Объявляйте перменные вне выражений for.
```javascript
// Плохая практика 👎 
for(let i = 0; i < someArray.length; i++) {
   var container = document.getElementById('container');
   container.innerHtml += 'my number: ' + i;
   console.log(i);
} 
```
На каждой итерации, мы заного ищем элемент container, это не эффективно
```javascript
//Хорошая практика 👍
var container = document.getElementById('container');
for(var i = 0, len = someArray.length; i < len;  i++) {
   container.innerHtml += 'my number: ' + i;
   console.log(i);
}
```
## 3) Инициализируйте переменные когда их объявляете.
Это хорошая практика которая дает:
    - Более чистый код
    - Избегание неназначенных(undefined) переменных
```javascript 
//Хорошая практика 👍
var firstName = "",
lastName = "",
price = 0,
discount = 0,
fullPrice = 0,
myArray = [],
myObject = {};
```
## 4) Избегайте глобальных переменных
Хорошая практика сокращать кол-во глобальных переменных. Это относится к всем типам данных, функциям и объектам. Глобальные переменные могут быть переназначенны ниже по коду

## 5) Объявления в начале.
Делать все объявления в начале каждого скрипта или функции - хорошая практика.
Плюсы: 
    - Более читсый код
    - Единственное место что бы искать переменные
    - Сокращает кол-во глобальных переменных
```javascript
//Хорошая практика 👍
// Объявление в начале
var firstName, lastName, price, discount, fullPrice;

// Использование позже
firstName = "John";
lastName = "Doe";

price = 19.90;
discount = 0.10;

fullPrice = price + discount;
```
## 6) Используйте строгое сравнение
  `==` приводит типы при сравнении, а `===` строго сравнивает типы и значения.
```javascript
//Хорошая практика 👍
0 == "";        // true
1 == "1";       // true
1 == true;      // true

0 === "";       // false
1 === "1";      // false
1 === true;     // false
```
## 7) Заканчивайте конструкции `stiwch` ключевым словом `default`.
```javascript
//Хорошая практика 👍
switch (new Date().getDay()) {
  case 0:
    day = "Sunday";
    break;
  case 1:
    day = "Monday";
    break;
  case 2:
    day = "Tuesday";
    break;
  case 3:
    day = "Wednesday";
    break;
  case 4:
    day = "Thursday";
    break;
  case 5:
    day = "Friday";
    break;
  case 6:
    day = "Saturday";
    break;
  default:
    day = "Unknown";
}
```
## 8) Используйте `'use strict'`.
С использованием `'use strict'` код работает немного по-другому, "тихие ошибки" будут ошибками, ошибки которые могут затруднить оптимизацию движка будут выявлены. В ES5 `'use strict'` опционально, но в ES6 это необходимо для многих нововведений.
## 9) Одна функция для одной задачи.
По возможности функция должна выполнять одну задачу, это облегчает дебаг и работу с вашим кодом другим разработчикам.
Это также относится к вспомогательным функциям для общих задач. Если вы замечаете что делаете одно и тоже в нескольких функциях, лучше создать более универсальную вспомогательную функцию, и использовать ее где необходимо.
Например, необходимо сделать вспомогательную функцию для создания ссылок:
```javascript
function addLink(text,url,parentElement){
  var newLink = document.createElement('a');
  newLink.setAttribute('href',url);
  newLink.appendChild(document.createTextNode(text));
  parentElement.appendChild(newLink);
}
```
Это работает, но допустим нам нужно добавлять различные атрибуты в зависимости от того, к каким элементам вы применяете ссылку:
```javascript
function addLink(text,url,parentElement){
  var newLink = document.createElement('a');
  newLink.setAttribute('href',url);
  newLink.appendChild(document.createTextNode(text));
  if(parentElement.id === 'menu'){
    newLink.className = 'menu-item';
  }
  if(url.indexOf('mailto:')!==-1){
    newLink.className = 'mail';
  }
  parentElement.appendChild(newLink);
}
```
Это делает функцию более специфичной и сложной в переиспользовании. Более лучший способ возращать ссылку, и покрыть допольнительные случаи в основных функциях, при необходимости. Так `addLink()` превращается в более универсальную `createLink()`
```javascript
function createLink(text,url){
  var newLink = document.createElement('a');
  newLink.setAttribute('href',url);
  newLink.appendChild(document.createTextNode(text));
  return newLink;
}

function createMenu(){
  var menu = document.getElementById('menu');
  var items = [
    {t:'Home',u:'index.html'},
    {t:'Sales',u:'sales.html'},
    {t:'Contact',u:'contact.html'}
  ];
  for(var i=0;i<items.length;i++){
    var item = createLink(items.t,items.u);
    item.className = 'menu-item';
    menu.appendChild(item);
  }
}
```
## 10) Избегайте использования `eval()`
Функция `eval()` используется что бы преобразовать текст в код. Почти во всех случаях нет необходимости использовать ее. 
Поскольку она позволяет исполнятся произвольному коду, это так же может приводить к проблемам безопасности.
