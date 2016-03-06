## debounce
Функция debounce может сыграть важную роль когда дело касается производительности событий. Если вы не используете debounce с событиями scroll, resize, key*, скорее всего, вы что-то делаете не так. Ниже приведен код функции debounce, которая поможет повысить производительность вашего кода:
```javascript
// Возвращает функцию, которая не будет срабатывать, пока продолжает вызываться.
// Она сработает только один раз через N миллисекунд после последнего вызова.
// Если ей передан аргумент `immediate`, то она будет вызвана один раз сразу после
// первого запуска.
function debounce(func, wait, immediate) {
    var timeout;
    return function() {
        var context = this, args = arguments;
        var later = function() {
            timeout = null;
            if (!immediate) func.apply(context, args);
        };
        var callNow = immediate && !timeout;
        clearTimeout(timeout);
        timeout = setTimeout(later, wait);
        if (callNow) func.apply(context, args);
    };
};
```

```javascript
// Использование
var myEfficientFn = debounce(function() {
// All the taxing stuff you do
}, 250);
window.addEventListener('resize', myEfficientFn);
```
debounce не позволит обратному вызову исполняться чаще, чем один раз в заданный период времени. Это особенно важно при назначении функции обратного вызова для часто вызываемых событий.


## poll
Функцию debounce не всегда возможно подключить для обозначения желаемого состояния: если событие не существует — это будет не возможно. В этом случае вы должны проверять состояние с помощью интервалов:
```javascript
function poll(fn, callback, errback, timeout, interval) {
    var endTime = Number(new Date()) + (timeout || 2000);
    interval = interval || 100;

    (function p() {
            // Если условие не выполнено, то мы закончили
            if(fn()) {
                callback();
            }
            // Если условие выполнено, но таймаут ещё не наступл — повторяем
            else if (Number(new Date())  0;
    },
    function() {
        // Колбек, который будет вызван в случае успеха
    },
    function() {
        // Колбек, который будет вызван в случае неудачи
    }
);
```

## once
Иногда бывает нужно, чтобы функция выполнилась только один раз, как если бы вы использовали событие onload. Функция once даёт такую возможность:

```javascript
function once(fn, context) {
    var result;

    return function() {
        if(fn) {
            result = fn.apply(context || this, arguments);
            fn = null;
        }

        return result;
    };
}

// Пример использования
var canOnlyFireOnce = once(function() {
    console.log('Запущено!');
});
canOnlyFireOnce(); // "Запущено!"
canOnlyFireOnce(); // Не запущено
```
once гарантирует, что заданная функция будет вызвана только один раз, что предотвращает повторную инициализацию.

## version sort
Готовая функция для сортировки версий

```javascript
var sorter = function(a, b){
    a = a.split('.');
    b = b.split('.');
    var max = Math.max(a.length, b.length);
    var an, bn, result = -1;

    for (var i = 0; i < max; i++) {
        an = parseInt(a[i]) || 0;
        bn = parseInt(b[i]) || 0;

        if (an > bn) {
            result =  1;
            break;
        }
        else if (an < bn) {
            result =  -1;
            break;
        }
        else {
            result = 0;    
        }
    }
    return result;
};


// Пример использования
var arr = ["0.5.12", "1.52,12", "0.1.2.3.4.5", "1.4"];
arr.sort(sorter);
```
