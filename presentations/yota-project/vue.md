# Простой пример компонента vue:
![alt text](https://ru.vuejs.org/images/vue-component.png)


# Использование data и включение компонентов
![alt text](https://ru.vuejs.org/images/vue-component-with-preprocessors.png)

# Vue следит за состоянием переменных и динамически обновляет компоненты
```js
<div id="example">
  <p>Изначальное сообщение: «{{ message }}»</p>
  <p>Сообщение задом наперёд: «{{ reversedMessage }}»</p>
</div>

var vm = new Vue({
  el: '#example',
  data: {
    message: 'Привет'
  },
  computed: {
    reversedMessage: function () {
      return this.message.split('').reverse().join('')
    }
  }
})
```

Во vue есть встроенная поддержка typescript

https://ru.vuejs.org/v2/guide/
