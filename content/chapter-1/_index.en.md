---
title: Basic concept
weight: 1
pre: "<b>1 </b>"
chapter: true
---

### Basic concept

# Function in JS

first class citizen
 - can put into variable
 - can pass through parameter
 - cab be a return value
 
first class object
 - same as first class citizen to object
 
first class function
 - same as first class citizen to object 
 - could create on runtime
 - could create as anonymous
 - anonymous function could be argument & return value
 
```javascript
// (anonymous) function could be a value & can assign to variable
function f1(){}
var a = typeof f1 == 'function' ? f1 : function() {};

// (anonymous) function could be return value
function f2(){
    return function() {};
}

// create & instant run (anonymous) function
(function (a,b) { return a+b;})(10,5);

// (anonymous) function could create instant & could be a argument 
function callAndAdd(a,b){
    return a()+b();
}

callAndAdd(function() { return 10;}, function() {return 5;});
```

# closure

closure holds outside context(upper scope) variable should be remove when it's created & use it inside .

the point is..
- it's function, but It
- holds upper scopes removed variable

```javascript
// closure is function that hold outside (context's) removed variable for later use
function thisFuction() {
    var useOutSideVariable = 5;
    // yes, below function closure.
    // but below function will removed by garbage collector.
    // doesn't need to reuse.
    function isClosure() {
        console.log(useOutSideVariable);
    }
} // there 'was' closure.

function thisFuction2(){
    var useOutSideVariable = 5;
    // below remain. because for future returnning.
    return function useClosure() {
        console.log(useOutSideVariable);
    }
}// now there is cosure.

thisFuction2()(); // 5

// ok, if parent function return result instaed of closure. there was closure.
function thisFuction2(){
    var useOutSideVariable = 5;
    // below not remain. 
    function useClosure() {
        console.log(useOutSideVariable);
    }
    // everythis done in here.
    return useClosuer();
}// now there was closure. It's gone.

// below nested function doesn't remember outside (context's)  removed  variable for later use.
function thisFuction3(){
    return function isNotClosure() {
        var useInSideVariable = 5;
        console.log(useInSideVariable);
    }
} // there is function now. not closure.

thisFuction3()();

var a = 10;
var b = 20;
function isNotCosureToo(){
    // it doesn't holds outside context's variable.
    // variables not removed also left global scope.
    return a+b;
} // so there is function. not closure.

```

ok, upper scope's context is whole not closure's before area.

```javascript
function upperContext(){
    var a = 10;
    // closure upper context catch time is not now!
    var closure = function(c) {
        // should holds a & b for later use with c
        return a + b + c
    };
    var b = 20;
    // now closure holds upper context's variable
    return closure;
}
var clos = upperContext();
cols(30);
// 60
```

# closure example

```html
<div class="user-list">
  <p>User list could follow</p>
</div>

<script>
// find elements
var banner = $("#banner-message")
var button = $("button")


var users = [
	{ id:1, name:'HA', age:25 },
  { id:2, name:'PJ', age:28 },
  { id:3, name:'JE', age:27 }
]

$('.user-list').append(
	_.map(users, function(user){
  	var button = $('<button>').text(user.name);
    button.click(function(){ 
      // user need to hold for later use.
      // yes, cloure
    	if(confirm(user.name +' follow?')) follow(user);
    });
		return button;    
  }));

function follow(user){
	$.post('/follow', {user_id: user.id}, function() {
  alert("now you could receive " + user.name + " news!");
  });
}
</script>
```

<a class="jsbin-embed" href="http://jsbin.com/xemoyij/embed?html,js,output">JS Bin on jsbin.com</a><script src="http://static.jsbin.com/js/embed.min.js?4.1.4"></script>

button holds handler f().   
and f()'s scope 0 is Closure.
and closure holds  upper(this time global) context's variable. 
![](../images/ch1-closure-example.png)
```js
$('.user-list').append(
	_.map(users, function(user){
        var button = $('<button>').text(user.name);
        button.click(function(){
    	    if(confirm(user.name +' follow?')) follow(user);
    });
		return button;    
  }));
```


closure mistakes

```js
var buttons = [];

var users = [
	{ id:1, name:'HA', age:25 },
  { id:2, name:'PJ', age:28 },
  { id:3, name:'JE', age:27 }
]

for (var i = 0; i < users.length; i++) {
  var user = users[i];
  console.log(user.name);
  buttons.push($('<button>').text(user.name).click(function() {
    alert(user.name);
  }));
}
$('.user-list').append(buttons);
```

```js
function() {
    alert(user.name);
    }
```

It is closuer. but, you know, closuer holds upper context varialbe. when all~~~ thing done.

`var user`  is not local variable, it's global scope variable. and re assign a new value.

It means when all `for loops` done, closure takes `user = users[2];`. It's ` { id:3, name:'JE', age:27 }`

var user inside loop scope. linked to global var user.

we could fix it

1. 1 loop 2 closure

```js
var buttons = [];

var users = [
	{ id:1, name:'HA', age:25 },
  { id:2, name:'PJ', age:28 },
  { id:3, name:'JE', age:27 }
]

for (var i = 0; i < users.length; i++) {
  var user = users[i];
  console.log(user.name);
  
  (function(user) {
      buttons.push($('<button>').text(user.name).click(function() {
        console.log(user.name);
      }));
    })(users[i]);
}
$('.user-list').append(buttons);
```

closure runs every loops instantly. upper context applied instantly with (`users[i]`).

2. use `_.map`

3. use let or const keyword

```javascript
var buttons = [];

var users = [
	{ id:1, name:'HA', age:25 },
  { id:2, name:'PJ', age:28 },
  { id:3, name:'JE', age:27 }
]

for (var i = 0; i < users.length; i++) {
  const user = users[i];
  console.log(user.name);
  buttons.push($('<button>').text(user.name).click(function() {
    alert(user.name);
  }));
}
$('.user-list').append(buttons);
```

const can't reassign value. so one local loop context variable extes. and one child closure take that const variable.
