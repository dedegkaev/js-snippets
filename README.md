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