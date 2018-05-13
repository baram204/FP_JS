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
 
```
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

```
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

ok, upper scope's context 
```
```


