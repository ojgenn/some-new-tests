1) Написать функцию, которая принимает на вход любое количество отрезков, заданных двумя координатами, и возвращает `true`, если любые 2 отрезка пересекаются, иначе возвращает `false`. Пример вызова: `f({x1: 1, x2: 10}, {x1: 11, x2: 123}, {x1: 122, x2: 124}, ...)`

https://vscode.ru/prog-lessons/nayti-tochku-peresecheniya-otrezkov.html. можно переделать на js. но не в этот раз

2) Написать HOC, который будет менять `document.title` страницы при рендеринге компонента, который оборачиваем в HOC, и устанавливать старый `title` (который был до рендеринга) при unmount. Пример вызова: `@titlePage('Some new title')`

```
const titlePage = (ComposedComponent, title )=>
  class extends React.Component {
    constructor() {
      super()
      this.state = {oldTitle: ""}
    }

    componentDidMount() {
      this.setState({oldTitle: document.title})
      document.title = title;
    }

    componentWillUnmount() {
      console.log(document.title)
      document.title = this.state.oldTitle;
    }

    render() {
      return (
        <ComposedComponent
          {...this.props}
        />
      )
    }
  }
  ```

3) Написать функцию, принимающую на вход 2 числа `a` и `b`, и возвращающая `x = a/b`, при этом 
  a) в `x` после запятой может быть максимум 2 цифры 
  b) в `x` все нули после запятой должны быть убраны
Проверки (в том числе на то что `a` и `b` являются числами) делать не нужно.
Функция должна добавлять `$` в начало ответа.
Примеры вызова функции:
```
f(1, 2); // $0.5
f(1, 3); // $0.33
f(200, 1); // $200

let f = (a,b) => {
  let x=a/b;
  x = (x*100 -x*100%1)/100;
  return `$${x}`
}
```

4) Написать класс, который 
  a) позволяет добавлять промис в массив
  b) вызывать промисы из массива в порядке очереди, ожидая resolve от каждого и выводя его ответ (через console.log), с минимальной задержкой между вызовами в 3 секунды
Проверки (в том числе на catch) делать не нужно
 
Пример использования класса X:
```
const x = new X();
const p1 = new Promise(res => setTimeout(() => res('r1'), 1000));
const p2 = new Promise(res => setTimeout(() => res('r2'), 5000));
const p3 = new Promise(res => setTimeout(() => res('r3'), 0));
x.push(p1);
x.push(p2);
x.push(p3);
```
Код должен вывести `r1` через одну секунду, затем `r2` через 4 секунды, затем `r3` через 3 секунды.   
```
class X {
  constructor () {
    this. state= []
  }
  
  push(val) {
    this.state = [...this.state, val]
  }
    
  show () {
    let now = () => Date.now();
    
    let delay = (time) => {
      return new Promise(resolve => {
        setTimeout(() => resolve(), time)
      })
    }
    
    let iterator = 0;
    this.state.reduce((prev, item) => {
      return prev.then((time) => {
        return item.then(res => { 
          let interval = now() - time;
          let delayTime = 0;
          if(iterator > 0) { delayTime = 3000 - interval}
          return delay(delayTime).then(_ => {
            console.log(res)
           iterator ++; 
           return now(); 
          })
        })
      })
    }, Promise.resolve(now()))
  }
}

const p1 = new Promise(res => setTimeout(() => res('r1'), 1000));
const p2 = new Promise(res => setTimeout(() => res('r2'), 5000));
const p3 = new Promise(res => setTimeout(() => res('r3'), 0));

let x = new X
x.push(p1);
x.push(p2);
x.push(p3);

x.show()
```

5) Написать функцию, которая превращает массив в объект, используя четный элемент массива в качестве ключа, а следующий нечетный элемент как значение. Цикл `for` использовать нельзя. Пример вызова функции: `f([1, 2, 'a', 3, 'b', 'c]); // {1: 2, a: 3, b: 'c'}`
```
let f = function(args) {
  let obj = {};
  args.map(function(key, index) {
    if(index%2===0) {
      obj[key]=  args[index+1]
    }
  })
  console.log(obj)
}


f([1, 2, 'a', 3, 'b', 'c'])
```

6) Написать обратную функцию для функции из 5. Цикл `for` использовать нельзя. Пример вызова функции: `f({1: 2, a: 3, b: 'c'}); // [1, 2, 'a', 3, 'b', 'c]`
```
let f = function(args) {
  let arr = [];
  Object.keys(args).map(function(key) {
   arr = [... arr, key, args[key]]
  });
  console.log(arr);
}
   
f({1: 2, a: 3, b: 'c'})
```