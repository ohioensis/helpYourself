# Оптимизация

**- Объединение медиавыражений в CSS**

Для этого есть плагин `css-mqpacker`

Нужно установить этот плагин в папку проекта, а затем добавить в код **в секцию postcss (где автопрефиксеры)** через запятую настройки для mqpacker'а

```
module.exports = function(grunt) {
	
  require("load-grunt-tasks)(grunt);

  grunt.initConfig({
		

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
		  }),
		  require("css-mqpacker")({
			sort: true
		  })
		]
	  },
	  style: {src: "css/*.css"}
    },

  });
};
```

**- Минификация CSS**

Для этого есть плагин `grunt-csso`

Его тоже нужно установить в папку проекта, а затем дописать код: 

```
module.exports = function(grunt) {
	
  require("load-grunt-tasks)(grunt);

  grunt.initConfig({
		
	csso: {
	  style: {
		options: {
		  report: "gzip"
		},
		files: {
		  "css/style.min.css": ["css/style.css"]
		}
	  }
	}

  });
};
```

**- Минификация изображений**

И для этого есть плагин! `grunt-contrib-imagemin`

Надо ли говорить, что он тоже ставится в папку проекта, а потом настраивается с помощью кода:

```
module.exports = function(grunt) {
	
  require("load-grunt-tasks)(grunt);

  grunt.initConfig({
		
	imagemin: {
	  images: {
		options: {
		  optimizationLevel: 3
		},
		files: [{
		  expand: true,
		  src: ["img/**/*.{png,jpg,gif}"]
		}]
	  }
	}

  });
};
```
**Важно** `optimtzationLevel` - уровень сжатия по шкале от 1 до 9 (1 - максимальное сжатие, 9 - без сжатия). 3 - минимальное безопасное сжатие

**- Сборка SVG-спрайта**

Подробнее в [файле про SVG](https://github.com/ohioensis/helpYourself/blob/master/helpYourself__svg.md)

## Сборка проекта

1. Создаем папку build
2. Убираем папку build в .gitignore
3. Перед сборкой копируем всё необходимое в build
4. Все оптимизации проводим в build
5. Удаляем лишнее

Пример файла `.gitignore`:

```
build/
node_modules/
.DS_Store
Thumbs.db
.idea
*.sublime*
*.log
*.psd
*.ai
*.pdf
```

Для копирования в Grunt тоже есть плагин — `grunt-contrib-copy`

```
module.exports = function(grunt) {
  require("load-grunt-tasks")(grunt);

  grunt.initConfig({
  	copy: {
  	  build: {
  	    files: [{
  	      expand: true,
  	      src: [
  	        "fonts/**/*.{woff,woff2}",
  	        "img/**",
  	        "js/**",
  	        "*.html"
  	      ],
  	      dest: "build"
  	    }]
  	  },
  	  html: {
  	    files: [{
  	      expand: true,
  	      src: ["*.html"],
  	      dest: "build"
  	    }]
  	  }

  	}
  });

 };
 ```
После этого вызываем в консоли в папке проекта команду `grunt copy`.

Сборка делается не один раз, и перед каждым обновлением папку build имеет смысл очищать, чтобы там не оставалось ничего лишнего. Для этого нужен плагин `grunt-contrib-clean`:

```
module.exports = function(grunt) {
  require("load-grunt-tasks")(grunt);

  grunt.initConfig({
  	clean: {
  	  build: ["build"]
  	}
  });

 };
 ```
И перед новым запуском команды `grunt copy` запускаем команду `grunt clean`.

**Изменение пути к файлу CSS**

При обновлении файлов less имеет смысл теперь обновлять сразу файл `style.css` в папке `build`. Нужно поменять путь в таске less (см. [файл про less и Grunt](https://github.com/ohioensis/helpYourself/blob/master/helpYourself__less.md))

```
module.exports = function(grunt) {
  grunt.loadNpmTasks('grunt-contrib-less'); 

  grunt.initConfig({
	less: { 							
	  style: {						
		files: {
		  "build/css/style.css" : "less/style.less"
 		}
	  }
	}
  });
};
```

То же самое мы делаем (меняем путь на путь к файлам в папке `build`) в настройках тасков для плагинов "grunt-postcss", "css-mqpacker", "grunt-csso", "grunt-svgstore", "grunt-svgmin".

Перенастраивая "browserSync" надо поменять не только путь к файлам html и css на файлы в папке `build`, но и вместо точки написать `build/`.

В таски плагина "watch" (там пути менять не надо), добавляем "csso" (строчка будет выглядеть как `tasks: ["less", "postcss", "csso"]`).

Кроме того, внутри "watch" надо добавить новый таск, выглядит это так:

```
module.exports = function(grunt) {
	
  require("load-grunt-tasks)(grunt);

  grunt.initConfig({

	watch: {
	  html: {
	  	files: ["*.html"],
	  	tasks: ["copy:html"]
	  },
	  style: {
		files: ["less/**/*.less"],
		tasks: ["less", "postcss"]
	  }
    },

  });
};
```

**После всех манипуляций** с тасками `Gruntfile.js` выглядит так (вместо многоточий в скопках описаны способы выполнения тасков, см. предыдущие фрагменты кода):

```
"use strict";

module.exports = function(grunt) {
  require("load-grunt-tasks")(grunt);

  grunt.initConfig({
  	clean {...}
  	copy {...},
  	less: {...},
  	postcss: {...},
  	csso: {...},
  	svgstore: {...},
  	svgmin: {...},
  	imagemin: {...},
  	browserSync: {...},
  	watch: {...}
  	
  });

  grunt.registerTask("serve", ["browserSync", "watch"]);
  grunt.regiaterTask("symbols", ["svgmin", "svgstore"]);

  grunt.registerTask("build", [
  	"clean",
  	"copy",
    "less",
    "postcss",
    "csso",
    "symbols",
    "imagemin"
  ]);

};
```

После этого в консоли вводится `grunt build`