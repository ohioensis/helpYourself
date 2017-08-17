# Препроцессор SASS

```
@import "normalize.scss";
```

С подключением файлов все работают одинаково.

### Переменные:

```
$body-bg: #cc0000;
$body-color: #ffffff;
$link-color: darkblue;
```

### Математические операции:

```
$body-font-size: 16px;
$blockquote-font-size: 16px * 1.25;
```

функция `ceil` — округление в бОльшую сторону:

`ceil((16px + 2cm) / 2)` // 46px;

`floor((16px + 2cm) / 2)` // 45px — округление в меньшую сторону;

`round((16px + 2cm) / 2)` // 46px - округление по математическим правилам;

### Операции с цветом:

```
$link-color: darkblue;
$link-color-hover: rgba($link-color, 0.8);
```

### Вложенные селекторы

& "переносит" название селектора к ховеру или другим элементам вложенности (модификатору, например)

```
a {
	color: $link-color;
	text-decoration: none;

	&:hover, &:focus {
		color: $link-color-hover;
		text-decoration: underline;
	}
}

.main-nav {
	width: 320px;

	&--full {
		width: 100%;
	}
}
```

С вложенными селекторами надо быть осторожными! Элементы лучше не вкладывать. Лучше использовать вложенность **только для псевдоклассов, псевдоэлементов и модификаторов**.

### Примеси (миксины)

```
@mixin cards() {
	display: inline-block;
	vertical-align: top;
}

.news__item {
	@include cards;
} 
```