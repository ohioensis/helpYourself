# Установка и работа с Jade

Jade — препроцессор для написания html-кода

**Установка**

`npm install jade --g` - устанавливается глобально

Затем переходим в папку проекта

**Начало работы**

Создаем файл index.jade и начинаем в нем писать код.

Чтобы скомпилировать файл index.html, находясь в папке проекта, нужно ввести в консоль команду 

`jade index.jade`

*НО!* В этом случае получившийся код будет писаться в одну строчку. Чтобы синтаксис был нормальным, нужно ввести команду 

`jade -P index.jade`

А чтобы файл обновлялся автоматически, нужно ввести

`jade -P -w index.jade`

**Синтаксис Jade**

Код пишется без угловых скобок и без закрывающих тегов. Вложенные теги отбиваются табами. Чтобы вставить текст, который занимает несколько строк, после тега нужно поставить точку (можно еще вертикальные черты перед каждой строкой, но точка проще, а эффект тот же)

```
doctype html
html
	head
		title csssr test
	body
		p. 	Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed |tempor incididunt ut labore et dolore magna aliqua. Ut enim ad 
			minim veniam,
			quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
			consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
			cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
			proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

Атрибуты добавляются так:
- класс через точку (несколько классов через точки тоже можно)
- id через решетку
- любые другие (и классы с id тоже) с помощью скобок, где они перечисляются через запятую

```
div.wrapper#main
	a(href="https://github.com/ohioensis/helpYourself/", class="link") link text
```

Комментарии добавляются как в JS

```
// Текст комментария
```

**Include**

Разметку можно писать отдельными блоками, а затем импортировать их в основной файл. Для этого в папке проекта создается папка includes, там создаются отдельные файлы блоков, например, _head.jade и _body.jade. После этого код будет выглядеть так

```
doctype html
html
	include includes/_head
	include includes/_body
```

Более подробно можно почитать в [руководстве пользователя Jade](http://jsman.ru/jade/) 

