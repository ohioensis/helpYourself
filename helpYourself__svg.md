# Обзор SVG

Текстовый и человекочитаемый формат.

Откуда берется SVG?
- пишется вручную
- экспортируется из графического редактора

Внутренностями SVG можно управлять из CSS и можно делать SVG-спрайты.

SVG можно вставлять инлайново (тегом прям в HTML), но ОЧЕНЬ много кода и он не кэшируется. Поэтому лучше отдельным файлом в <img> или в CSS

Ширину SVG лучше всего прописывать в HTML (в теге <img>), потому что если CSS не загрузится, картинка займет всю ширину контейнера. Либо размеры должны быть прописаны внутри SVG-файла, который вставляется через тег <picture>

Атрибут `fill` в коде SVG-файла отвечает за его цвет, можно менять заливку.

Атрибут `preserveAspectRatio` отвечает за сохранение пропорций. По умолчанию пропорции сохраняются, а чтобы их "сломать", нужно задать ```preserveAspectRatio="none"```.

## SVG-спрайт

Вставляется в начале <body> тег <svg> с атрибутом `style="display:none"`, а внутри тега теги <symbol> c обязательным и уникальным атрибутом `id`.

Для сборки спрайтов есть специальные сервисы, либо можно вручную это делать.

И далее в нужных местах на странице вставляются теги <svg> вида:

```
<svg width="32" height="32"><use xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="#icon-arr-right"></use></svg>
```
где  `xlink:href="#icon-arr-right"` — указание id нужного элемента из спрайта.

Техника оптимизации: вставка кода <svg> из отдельного файла через Ajax.

## Автоматизация сборки SVG-спрайтов

В Grunt используется плагин `grunt-svgstore`, который сначала подключается в папку проекта, а затем код добавляется в файл `Gruntfile.js`:

```
module.exports = function(grunt) {
  require("load-grunt-tasks")(grunt);

  grunt.initConfig({

    svgstore: {
      options: {
      	svg: {
      	  style: "display: none"
      	}
      },
      symbols: {
      	files: {
      	  "img/symbols.svg":["img/icons/*.svg"]
      	}
      }
    }
  });
};
```

Еще существует плагин `grunt-svgmin`, чтобы минифицировать спрайт. Устанавливается, а затем вот так подключается:

```
module.exports = function(grunt) {
  require("load-grunt-tasks")(grunt);

  grunt.initConfig({

    svgmin: {
      symbols: {
      	files: [{
      	  expand: true,
      	  src: ["img/icons/*.svg"]
      	}]
      }
    }
  });
};
```

**Важно!** Таски `svgstore` и `svgmin` надо объединить. Делается это с помощью команды для запуска (см. последнюю строку кода):

```
module.exports = function(grunt) {
  require("load-grunt-tasks")(grunt);

  grunt.initConfig({

    svgstore: {
      options: {
      	svg: {
      	  style: "display: none"
      	}
      },
      symbols: {
      	files: {
      	  "img/symbols.svg":["img/icons/*.svg"]
      	}
      }
    },

    svgmin: {
      symbols: {
      	files: [{
      	  expand: true,
      	  src: ["img/icons/*.svg"]
      	}]
      }
    }
  });

  grunt.registerTask("symbols", ["svgmin", "svgstore"]);
};
```
После этого нужно ввести в консоли в папке проекта `grunt symbols`
