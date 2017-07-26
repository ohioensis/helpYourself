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

**Синтаксис Jade**

Код пишется без угловых скобок и без закрывающих тегов. Вложенные теги отбиваются табами. Чтобы вставить текст, который занимает несколько строк, перед новыми строками нужно поставить вертикальные черты

```
doctype html
html
	head
		title csssr test
	body
		p 	Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed |tempor incididunt ut labore et dolore magna aliqua. Ut enim ad 
			|minim veniam,
			|quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
			|consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse
			|cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non
			|proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
```

