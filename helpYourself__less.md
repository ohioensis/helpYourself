# Препроцессор less и как с ним работать

**Важно:** не использовать всё сразу, двигаться постепенно.

Стили для каждого блока прописывать в отдельном файле в папке `/less`.

## Цветовые функции:

**Важно:** Если дизайн готов и все цвета обозначены в стайлгайде, не нужно использовать цветовые функции для получения этих цветов

`@color-1: red;`

`@color-2: spin(@color-1, 180);` // сдвигает цвет по кругу на цветовой палитре. Число показывает градусы, может быть положительным и отрицательным.

`@color-3: lighten(@color-1, 30%);` // делает цвет на 30% светлее. В противоположную сторону — темнее — делает функция darken.

`@color-4: saturate(@color-1, 10%);` // увеличивает насыщенность цвета на 10%. В противоположную сторону насыщенность меняет — уменьшает — функция desaturate.

`@color-5: fade(@color-1, 80%);` // добавляет непрозрачность 0.8.

## Вложенные селекторы

Удобно описывать свойства модификаторов, псевдоэлементов и псевдоселекторов.

Избегать излишней вложенности

## Примеси

Бывают с параметрами и без

`.example(@parameter) {
	color: @parameter;
}`

## Расширение

[Статья про то, почему миксины лучше расширений](https://csswizardry.com/2016/02/mixins-better-for-performance/)

Лучше вообще не использовать

## Организация кода стилей

Папка `less/`, а в ней папка `blocks/`, где лежат отдельные файлы для каждого блока. И прямо в папке `less/` файлы `mixins.less` (примеси для определенных свойств или наборов свойств), `variables.less` (объявляются все переменные) и главный - `style.less`, куда импортированы все остальные файлы.

В файле `style.less` нет никакого кода — только поключения вида `@import "variables.less";` и `@import "blocks/page-header.less";`.

К переменным есть разные подходы: можно писать в отдельном файле только переменные цветовой схемы и шрифтов, а остальные там, где они используются (в маленьких стилевых файлах); а можно все в отдельный файл. Оба подхода имеют право на жизнь.

## Автоматизация (Grunt)

**Важно:** Устанавливать Grunt в директорию проекта!

`npm i -g grunt grunt-cli` - установка Grunt ГЛОБАЛЬНО 

`npm i grunt grunt-cli` - установка Grunt в директорию проекта

`npm init` - создание файла package.json

После этого создается файл Gruntfile.js в директории проекта (название с заглавной буквы — обязательно).

Найти нужный плагин на сайте gruntjs.com и установить его через консоль.

В файле Gruntfile.js пишется следующий код:

```
module.exports = function(grunt) {
	grunt.loadNpmTasks('grunt-contrib-less'); // эту строчку копируем из readme к найденному плагину
	grunt.initConfig({
		less: { 							// название конфигурации
			style: {						// название процесса, придумываем сами
				files: {
					"css/style.css" : "less/style.less" // путь (куда) <== (откуда)
 				}
			}
		}
	});
};
```

### Конфигурация сборки стилей

```
module.exports = function(grunt) {
	grunt.loadNpmTasks('grunt-contrib-less');
	grunt.loadNpmTasks("grunt-browser-sync");	**// подключения таска для живого сервера разработки**
	grunt.loadNpmTasks("grunt-contrib-watch");	**//  подключение таска для слежения за изменениями**
	grunt.loadNpmTasks("grunt-postcss");	**// подключение таска для автопрефиксера** 

	grunt.initConfig({
		less: { 							
			style: {						
				files: {
					"css/style.css" : "less/style.less" 
 				}
			}
		},

		postcss: {
			options: {
				processors: [
					require("autoprefixer")({browsers:
						[
						"last 1 version",
						"last 2 Chrome versions",
						"last 2 Firefox versions",
						"last 2 Opera versions",
						"last 2 Edge versions"
						]

					})]
				},
			style: {src: "css/*.css"}
		},

		watch: {
			style: {
				files: ["less/**/*.less"],
				tasks: ["less", "postcss"]
			}
		},

		browserSync: {
			server: {
				bsFiles: {
					src: ["*.html", "css/*.css"]
				},
				options: {
					server: "."
				}
			}
		}

	});
};
```
Для того, чтобы "автоматизировать автоматизацию" (загружать таски), в Grunt тоже есть плагин.

Чтобы его установить, в консоли в папке проекта нужно запустить команду `npm i --save-dev load-grunt-tasks`

После этого конфигурация сборки выглядит так:

```
module.exports = function(grunt) {
	
	require("load-grunt-tasks)(grunt);

	grunt.initConfig({
		less: { 							
			style: {						
				files: {
					"css/style.css" : "less/style.less" 
 				}
			}
		},

		postcss: {
			options: {
				processors: [
					require("autoprefixer")({browsers:
						[
						"last 1 version",
						"last 2 Chrome versions",
						"last 2 Firefox versions",
						"last 2 Opera versions",
						"last 2 Edge versions"
						]

					})]
				},
			style: {src: "css/*.css"}
		},

		watch: {
			style: {
				files: ["less/**/*.less"],
				tasks: ["less", "postcss"]
			}
		},

		browserSync: {
			server: {
				bsFiles: {
					src: ["*.html", "css/*.css"]
				},
				options: {
					server: "."
				}
			}
		}

	});
};
```


