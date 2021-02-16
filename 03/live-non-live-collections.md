Найти несколько DOM-элементов и получить к ним доступ из JavaScript можно разными способами: ```querySelectorAll```, ```getElementsByTagName```, ```children``` и так далее. В итоге в каждом случае будет возвращена **коллекция** — сущность, которая похожа на массив объектов, но при этом им не является, на самом деле это набор DOM-элементов. Стоит учесть, что фактически разные методы возвращают разные коллекции:

* ```HTMLCollection``` — коллекция непосредственно HTML-элементов.
* ```NodeList``` — коллекция узлов, более абстрактное понятие. Например, в DOM-дереве есть не только узлы-элементы, но также текстовые узлы, узлы-комментарии и другие, поэтому NodeList может содержать другие типы узлов.

При работе с DOM-элементами тип коллекции значительной роли не играет, поэтому для удобства будем рассматривать их как одну сущность — коллекцию.

Во время работы с коллекциями можно столкнуться с поведением, которое покажется странным, если не знать один нюанс — они бывают **живыми** (динамическими) и **неживыми** (статическими). То есть либо реагируют на любое изменение DOM, либо нет. Вид коллекции зависит от способа, с помощью которого она получена. Рассмотрим на примере.

## Разница между живыми и неживыми коллекциями
Допустим, в разметке есть список книг:
```html
<ul class="books">
    <li class="book book--one"></li>
    <li class="book book--two"></li>
    <li class="book book--three"></li>
</ul>
```
Для взаимодействия с книгами получим с помощью JavaScript список всех нужных элементов. Чтобы в дальнейшем увидеть разницу между видами коллекций, используем разные способы поиска элементов — свойство ```children``` и метод ```querySelectorAll```:
```javascript
const booksList = document.querySelector(`.books`);
const liveBooks = booksList.children;

// Выведем все дочерние элементы списка .books
console.log(liveBooks);
```
```javascript
const notLiveBooks = document.querySelectorAll(`.book`);

// Выведем коллекцию, содержащую все элементы с классом book
console.log(notLiveBooks);
```
Пока никакой разницы не видно. В обоих случаях ```console.log``` выведет одни и те же элементы. Но что, если попробовать удалить из DOM одну из книг?
```javascript
const booksList = document.querySelector(`.books`);
const liveBooks = booksList.children;


liveBooks[0].remove(); // Удалим первую книгу

console.log(liveBooks.length);
// Получим 2

console.log(liveBooks[0]);
// Получим элемент book--two, который теперь стал первым в коллекции
```
```javascript
const notLiveBooks = document.querySelectorAll(`.book`);


notLiveBooks[0].remove(); // Удалим первую книгу

console.log(notLiveBooks.length);
// Получим 3

console.log(notLiveBooks[0]);
// Получим ссылку на удалённый элемент book--one
```
В первом случае информация о количестве элементов внутри коллекции автоматически обновилась после удаления одного элемента из DOM — эта коллекция живая. Во втором случае в переменной ```notLiveBooks``` хранится первоначальное состояние коллекции, которое было актуально на момент вызова метода querySelectorAll. Эта коллекция неживая, она ничего не знает об изменении DOM. При этом доступна ссылка на удалённый элемент book--one, которого фактически больше нет в DOM.

## Другие способы получить коллекцию
Кроме ```children``` и ```querySelectorAll``` есть другие способы поиска DOM-элементов:

* ```getElementsByTagName(tag)``` — находит все элементы с заданным тегом,
* ```getElementsByClassName(className)``` — находит все элементы с заданным классом,
* ```getElementsByName(name)``` — находит все элементы с заданным атрибутом name.

Все эти методы возвращают живые коллекции. Они используются реже, потому что в большинстве случаев удобнее применять ```querySelectorAll```, но могут встречаться в старом коде.

## Как использовать
Для решения большинства задач можно ограничиться неживыми коллекциями. Но если нужно сохранить ссылку на реальное состояние DOM — понадобится живая коллекция. Это удобно в тех случаях, когда программе нужно постоянно манипулировать списком элементов, которые могут регулярно удаляться и добавляться. Хороший пример — задачи в таск-трекере. С помощью живой коллекции можно хранить именно те задачи, которые фактически существуют в данный момент времени.

Структура и некоторые свойства коллекции имеют много общего с массивом. Например, у неё тоже есть свойство ```length```, и элементы коллекции можно перебирать в цикле ```for...of```, потому что это перечисляемая сущность. Но, как упоминалось ранее, коллекции не во всём похожи на обычные массивы. С коллекциями не работают такие методы массивов, как ```push```, ```splice``` и другие. Для их использования нужно преобразовать коллекцию в массив — например, с помощью метода ```Array.from```:
```javascript
const booksList = document.querySelector(`.books`);
const books = booksList.children;

// Выведет обычный массив с элементами из коллекции books
console.log(Array.from(books));
```
При этом нужно помнить — массив статичен, поэтому при таком преобразовании теряются преимущества живых коллекций.