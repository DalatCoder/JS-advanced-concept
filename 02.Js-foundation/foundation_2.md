# JS foundation 02

## Execution context

Khi 1 hàm được gọi, nó sẽ thực thi và tạo ra 1 execution context

Execution context mặc định, làm nền tảng cho các execution context khác là global execution context (window object)

Bất cứ khi nào chúng ta thực thi code javascript, nó luôn nằm bên trong execution context (window object trong trình duyệt hoặc global object trong node)

*Khi file JS được thực thi trong trình duyệt, JS engine tự động tạo ra global execution context, bên trong tạo sẵn 2 đối tượng là global object và this object. Trỏ this đến global object, vì vậy this === window -> true.*

I. Creation phase:

- Tạo 2 đối tượng global object và this, cho this trỏ tới global

II. Execution phase: 

- Thực thi code

## Lexical environment

Lexical: lexical environment is simply where you write something

Lexical means at compile time. Where is the code written.

```js
function printName() {
    return 'Nguyen Trong Hieu';
}

function findName() {
    return printName();
}

function sayMyName() {
    return fidnName();
}

sayMyName();
```

Lexical environment của function printName là global execution context, tương tự với function findName và sayMyName

The very first lexical environment is the global lexical environment where we write our code.

In JS, every time we have a function, it creates a new world for us inside of that function (new execution context)

Execution context ==> Which lexical environment is currently running 

In Javascript our *lexical scope* (available data + variables where the function was defined) determines our available variables. Not where the function is called (dynamic scope).

## Hoisting

I. Creation phase:

- Tạo 2 đối tượng global object và this, cho this trỏ tới global
- Hoisting

II. Execution phase:

- Thực thi code

Hoisting is the behavior of moving the variables or function declarations to the top of their respective environments during compilation phase

Variables are **partially** hoisted and function declarations are hoisted

```js
// Hoisting
// var teddy = undefined;
// function sing() {
//     console.log('ohh lalala');   
// }

console.log(teddy); // ==> undefined
sing(); // ==> ohh lalala

var teddy = 'bear';
function sing() {
    console.log('ohh lalala');
}
```

```js
console.log(teddy); // ==> error: teddy is not defined
sing(); // ==> error: sing is not defined

const teddy = 'bear';
(function sing() {
    console.log('ohh lalala');
})
```

Trong creation phase, JS engine sẽ đọc toàn bộ code 1 lượt, khi gặp lệnh bắt đầu với từ khóa var hoặc function thì quá trình hoisting sẽ diễn ra. JS engine sẽ cấp phát vùng nhớ (heap memory) cho biến var, nhưng không gán giá trị, và cấp phát toàn bộ vùng nhớ cho function. Vì vậy trong trường hợp gọi biến teddy như trên, teddy được khai báo với var và được hoist, cấp vùng nhớ trong heap, teddy đã tồn tại, nhưng chưa có giá trị nên được gán undefined, còn sing được khai báo với function, được hoist (full hoisting), cho nên có thể gọi hàm bình thường.

Hoisting exercise

```js
var favouriteFood = 'grapes';

var foodThoughts = function() {
    console.log("Original favourite food: " + favouriteFood);

    var favouriteFood = 'sushi';

    console.log('New favourite food: ' + favouriteFood);
}

foodThoughts();
```

==> Original favourite food: undefind

==> New favourite food: sushi

**Hoisting happens on every execution context**

==> Recommend: avoid hoisiting ==> not using var keyword

```js
const favouriteFood = 'grapes';

const foodThoughts = function() {
    console.log("Original favourite food: " + favouriteFood);

    const favouriteFood = 'sushi';

    console.log('New favourite food: ' + favouriteFood);
}

foodThoughts();
```

==> Error: ReferenceError: Cannot access 'favouriteFood' before initialization

## Function invocation

```js
// Function Expression
// Function defined at runtime
var sayHello = function() {
    console.log('Hello!!!');
}

// Function Declaration
// Function defined at compile time
function sayHi() {
    console.log('HI!!!');
}

// Function Invocation/Call/Execution
sayHello();
sayHi();

// When a function is called, it creates new execution context
// for that function
```

Khi 1 hàm được thực thi, nó tạo ra 1 execution context mới, bên trong execution context đó chứa 2 đối tượng là this và arguments

```js
function marry(person1, person2) {
    console.log(arguments);
    return `${person1} is now married to ${person2}`;
}

marry('Tim', 'Tina');
// ==> { 0: 'Tim', 1: 'Tina' }
// ==> Tim is now married to Tina
```

Arguments object không tồn tại ở global execution context

## Arguments keyword

Dùng từ khóa arguments ảnh hưởng đến việc tối ưu code của compiler, nếu phải dùng, chuyển nó về mảng để dễ lặp qua các phần tử

Cách 1: Dùng *Array.from()*

```js
function marry(person1, person2) {
    console.log(arguments);
    console.log(Array.from(arguments));
    return `${person1} is now married to ${person2}`;
}

marry('Tim', 'Tina');
// ==> { 0: 'Tim', 1: 'Tina' }
// ==> [ 'Tim', 'Tina' ]
// ==> Tim is now married to Tina
```

Cách 2: Dùng rest parameter

```js
function marry(...args) {
    console.log(arguments);
    console.log(args);
    return `${person1} is now married to ${person2}`;
}

marry('Tim', 'Tina');
// ==> { 0: 'Tim', 1: 'Tina' }
// ==> [ 'Tim', 'Tina' ]
// ==> Tim is now married to Tina
```

## Variable Environment

Tồn tại trong bất kỳ execution context nào (được tạo thông qua việc gọi hàm)

Trả lời câu hỏi: Chuyện gì xảy ra với các biến được tạo trong mỗi execution context???

```js
function two() {
    var isValid;
}

function one() {
    var isValid = true;
    two();
}

var isValid = false;
one();

// callstack - the value of isValid in each execution context
// two() -- undefined
// one() -- true
// global() -- false
```

Each execution context has its own variable environment

## Scope chain

Sự liên kết giữa các lexical enviroment

1 environment liên kết đến environment cha nó, cha nó là ai? Phụ thuộc vào khai báo hàm lexical (where the function is written)

```js
var x = 'x'
function findName() {
    var b = 'b'
    return printName()
}
function printName() {
    var c = 'c';
    return 'Hieu'
}
function sayMyName() {
    var a = 'a'
    return findName()
}


sayMyName()
```

Ở code trên, cả 3 hàm findName, printName, sayMyName đều truy cập được vào biến x. Vì cả 3 hàm đều được viết bên trong global execution context và biến x được định nghĩa trong global execution context (Lexical)

**In javascript our lexical scope (available data + variables where the function was defined) determines our available variables. Not where the function is called (dynamic scope)**

Scope: where we can access a variable    

```js
function sayMyName() {
    var a = 'a'
    return function findName() {
        var b = 'b'
        // console.log(c)
        return function printName() {
            var c = 'c'
            return 'Trong Hieu'
        }
    }
}

sayMyName() // return function findName
sayMyName()() // Invoke findName function, return function printName
sayMyName()()() // Invoke printName function ==> 'Trong Hieu'
```

Scope chain của printName: printName variable environment => findName variable environment ==> sayMyName variable environment ===> global variable environment (Lexical)

Global scope là scope ngoài cùng của bất kì scope nào

Eval() và with có thể thay đổi scope ==> Không tốt cho việc tối ưu

## Lexical Environment === [[scope]]

```js
'use strict'
function weird() {
    // when use 'use strict' ==> height is not defined
    height = 50 // leakage of global variable
    return height
}
weird()
```

Dùng 'use strict' ở đầu file js để loại bỏ weird behavior của js

```js
var heyhey = function doodle() {
    // do something
    console.log('Hello')
    // doodle()
}

heyhey() // ==> 'Hello'
doodle() // ==> error: doodle is not defined
```

doodle không thể truy cập ở global execution context bởi vì doodle bị kẹt bên trong scope của chính nó, scope bên ngoài không thể truy cập vào

## Function scope vs Block scope

Khi ta tạo 1 hàm, nó tạo ra 1 execution context, bên trong chứa variable environment (scope) và tham chiếu đến scope bên ngoài (scope cha | lexical) thông qua thuộc tính [[scopes]] (Xem trên trình duyệt)

```js
if (5 > 4) {
    var secret = '12345'
}

secret; // ==> 12345 || var is defined with function scope


function a() {
    var secret = '12345'
}

secret; // ==> error: secret is not defined

if (5 > 4) {
    const secret = '12345'
}

secret; // ==> error
```

Block scope: khi gặp 1 cặp dấu {}, JS engine tạo ra 1 scope, dùng với const hoặc let (ES6)

```js
function loop() {
    for (var i = 0; i < 5; i++) {
        console.log(i)
    }

    console.log(i) // i = 5

    for (let j = 0; j < 5; j++) {
        console.log(j)
    }
    console.log(j) // errror: j is not defined
}
```

## Pollute the global namespace

```html
<!DOCTYPE html>
<html>
<head></head>
<body>
    <script>var z = 1</script>        
    <script>var zz = 2</script>
    <script>var zzz = 3</script>
    <script>var z = 1000</script>
</body>
</html>
```

Trong tag script, tất cả code js sẽ được gom chung vào 1 execution context, do đó xảy ra tình trạng các biến giống nhau ghi đè lên nhau, trong trường hợp này biến z có giá trị 1, sau đó bị ghi đè bởi giá trị 1000.

**Giải quyết bằng cách dùng IIFE hoặc module**

## IIFE - Immediately invoked function expression

```js
(function() {

})();

function(){}() // syntax error
```

Lợi ích khi dùng IIFE:

- Khai báo hàm anonymous, không có tên hàm nên sẽ không tạo thêm thuộc tính vào window object 

- Tất cả khai báo biến, hàm sẽ scope bên trong hàm anonymous, không ảnh hưởng đến bên ngoài

- Sau khi chạy hàm, garbage collection sẽ tự động dọn dẹp, không gây ra memory leak

```html
<!DOCTYPE html>

<html>
<head></head>
<body>
    <script>
        var z = 1
        var script1 = (function() {
            function a() {
                return 5
            }

            return {
                a: a
            }
        })()
    </script>        
    <script>var zz = 2</script>
    <script>var zzz = 3</script>
    <script>
        var z = 1000
        function a() {
            return 'Hello'
        }
    </script>
</body>
</html>

<!-- a() ==> Hello -->
<!-- script1.a() ==> 5 -->
```

## THIS

**This is the object that the function is a property of**

1 hàm là thuộc tính của đối tượng nào thì this bên trong hàm chính là đối tượng ấy.



```js
function a() {
   console.log(this)
}


a() // window
window.a() // window
```

Trong trường hợp trên, this bằng window bởi vì hàm a là thuộc tính của đối tượng window (bởi vì được khai báo trong global execution context)



```js
function b() {
    'use strict'
    console.log(this)
}

b() // undefined
```

ES6 mặc định dùng 'use strict'

```js
const obj = {
    name: 'Hieu',
    sing() {
        return 'lalala ' + this.name
    },
    singAgain() {
        return this.sing() + '!'
    }
}
```

Trong trường hợp trên, this bằng obj bởi vì hàm sing là thuộc tính của đối tượng obj.

**This refers to whatever is to the left of the dot**

Benefit of this

1. Gives methods access to their object

2. Execution same code for multiple objects

```js
const a = function() {
	console.log('a', this)
	const b = function () {
		console.log('b', this)
		const c = {
			hi() {
				console.log('c', this)
			}
		c.hi()
		}
	b()
	}
}

a()
```
Đoạn code trên in ra kết quả như sau:
- 'a': window
- 'b': window
- 'c': c { ... }
Giải thích, vì a là 1 thuộc tính của đối tượng window nên this trong a chính là đối tượng window, b là 1 hàm thông thường, được gọi bên trong hàm a, không là thuộc tính của đối tượng nào, mặc định this = window. Hàm hi là thuộc tính của đối tượng c nên this trong hàm hi bằng đối tượng c.

```js
const obj = {
	name: 'Hieu',
	sing() {
		console.log('a', this)
		var anotherFunc = function () {
			console.log('b', this)
		}
		anotherFunc()
	}
}

obj.sing()
// 'a': obj {...}
// 'b': window
// this default value to window object
```
**'This' is not lexically scope, it doesn't matter where it is run. It matters how the function was called**
**This object is dynamic scope**

Có 2 cách để xử lý các vấn đề với this:
1.  Dùng arrow function (ES6)
```js
const obj = {
	name: 'Hieu',
	sing() {
		console.log('a', this)
		var anotherFunc = () => {
			console.log('b', this)
		}
		anotherFunc()
	}
}

obj.sing()
// 'a': obj {...}
// 'b': obj {...}
```
Arrow function is **lexically** bound | Arrow function has a lexical this behavior.
2. Trước khi có ES6
```js
const obj = {
	name: 'Hieu',
	sing() {
		console.log('a', this)
		var anotherFunc = function () {
			console.log('b', this)
		}
		return anotherFunc.bind(this)
	}
}

obj.sing()
// 'a': obj {...}

obj.sing()()
// 'b': obj {...}
```
3. Tạo 1 tham chiếu đến this, 'self'
```js
const obj = {
	name: 'Hieu',
	sing() {
		console.log('a', this)
		var self = this 
		var anotherFunc = function () {
			console.log('b', self)
		}
		anotherFunc()
	}
}

obj.sing()
// 'a': obj {...}
// 'b': obj {...}
```

## call(), apply(), bind()
Mặc định, tất cả function sử dụng hàm call khi function được thực thi.
```js
function a() {
	console.log('Hi')
}

a.call()

// a() ==> a.call()
````
```js
const wizard = {
	name: 'Merlin',
	health: 100,
	heal() {
		this.health = 100
	}
}

const archer = {
	name: 'Robin Hood',
	health: 30
}

wizard.heal()
// borrow method of another object | DRY principle

wizard.heal.call(archer)
// this inside heal method will the the archer
```
Call với apply về cơ bản giống nhau, chúng khác nhau ở chỗ: với call ta có thể truyền tham số thoải mái, với apply, tham số được đặt bên trong 1 mảng
```js
wizard.heal.call(archer, param1, param2, param3)
wizard.heal.apply(archer, [param1, param2, param3])
```

Bind tương tự như call với apply, nhưng thay vì thực thi hàm trực tiếp thì bind trả về 1 hàm.
```js
const healArcher = wizard.heal.bind(archer, param1, param2)
healArcher()
````

**Call and apply are useful for borrowing methods from an object**
**Bind is useful for us to call function later on with a certain context or certain this keyword**

## Function currying
```js
function multiply(a, b) {
	return a * b
}

let multiplyByTwo = multiply.bind(this, 2)
console.log(multiplyByTwo(4)) // ==> 8
```

## this exercise
```js
var b = {
	name: 'jay',
	say() { console.log(this) }
}

var c = {
	name: 'jay',
	say() { return function() { console.log(this) } }
}

var d = {
	name: 'jay',
	say() { return () => console.log(this) }
}

b.say() // ==> b object
c.say()() // ==> window object
d.say()() // ==> d object, arrow function makes this lexically scope
```

## Context vs Scope
1. Context is most often determined by how a function is invoked with the value of this keyword
2. Scope refers to the visibility of variables