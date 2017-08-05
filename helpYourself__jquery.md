# jQuery — полезные ссылки и готовые скрипты

### Скрипт для плавного скролла к якорям

```
$(document).ready(function(){
  $("#button").click(function() {
    $('html, body').animate({
      scrollTop: $("#scroll").offset().top
    }, 1000);
  });
});
```
`#button` — id элемента, на который кликать
`#scroll` — id элемента, к которому скроллить