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