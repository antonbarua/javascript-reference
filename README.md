# JavaScript Reference

## ES6 Modules

*  There is exactly *one* module per file, and one file per module in ES6.
*  Modules have there own *scope*. They do not pollute the global scope.

    ### Naming modules
    The name of the module comes from the file or folder name, and the *.js* extension is omitted.

    ```javascript
    //module is at module.js
    import x from './module'

    //module is under /module and /module has an index.js /module/index.js

    import x from './module'
    ```
    ### Exports

    Any JavaScript value can be exported from a module. 

    ```javascript
    export default 100; //number
    export default {var:100}; //object
    export default 'Hello, world!'; //string
    export default function hello(){} //function
    ```

    #### Default Exports

    Every module can  have *one and only one* default export.
    Default exports can be imported without specifying variable name.
    
    ```javascript
    //helloworld.js
    export default function hello(){
        return 'hello, world!';
    }

    //importer.js
    import helloworld from './helloworld'
    console.log(helloworld());
    ```

   #### Named Exports
   Besides default exports, modules can export other things via named exports. In this case, the same name has to be used for exporting and importing the variable. 

   ```javascript
   //exporter.js
   const PI = 3.14;
   const HUNDRED = 100;
   export {PI, HUNDRED}; 

   //importer.js
   import {PI, HUNDRED} from '/exporter';
   console.log(PI + HUNDRED);
   ```
   The `as` keyword can be used to rename variables during import and export.

   ```javascript
   //exporter.js
   const hundred = 100;
   export {hundred as THE_VALUE};

   //importer.js
   import {THE_VALUE as ONE_HUNDRED} from './exporter';
   console.log(ONE_HUNDRED);
   ```

   #### Import All
   The `*` notation is used to import all the values a module exports.

   ```javascript
   //exporter.js
   const PI = 3.14;
   export default 100;
   export PI;

   //importer.js
   import * as exp from './exporter';
   console.log(exp.default); //100
   console.log(exp.PI); //3.14

   //import * as exp from './exporter' will import all values and the default will be available as exp.default

   //import exp from './exporter' will import the default only.
   ```

   #### Re-exporting

   A module can re-export another module's variables.

   ```javascript
   //exporter.js
   const PI = 3.14;
   const HUNDRED = 100;

   export {PI, HUNDRED};
   //importer.js
   export * from './exporter';
   export {PI} from './exporter';
   export {PI as THE_NUMBER} from './exporter';
   ```

Reference: [An Introduction To JavaScript ES6 Modules](https://strongloop.com/strongblog/an-introduction-to-javascript-es6-modules/)


## Promises

### Why Promises?

Let's take a look at the following code examples for adding two numbers.

```javascript
//Synchronous code
function add(a, b){
    return a + b;
}

console.log(add(1,2)); //3

//Asynchronous, broken code
let result = addOnRemoteServer(1, 2); //async call, won't return in time
console.log(result); //undefined

//Asynchronous code
function addAsync(a, b, callback){
    setTimeout(function(){
        callback(a + b);
    }, 1000);
}

addAsync(1, 2, function(result){
    console.log(result); 
});
```
But what if we need to add multiple times?

```javascript
//Synchronous code
let resultA = add(1,2); //3
let resultB = add(resultA, 3); //6
let resultC = add(resultB, 4); //10

console.log(resultC);

//Asynchronous code
addAsync(1, 2, function(resultA){//3
    addAsync(resultA, function(resultB){//6
        addAsync(resultB, function(resultC){//callback hell/pyramid code
            console.log(resultC);
        });
    });
});

```
*Promises to the rescue!*

```javascript
//instead of returning result, return a promise that a result will be returned
function addAsync(a, b){
    return new Promise((resolve, reject)=>{
        setTimeout(()=>{
            resolve(a + b);
        }, 1000);
    });
}

addAsync(1, 2)
    .then((resultA)=>{return addAsync(result, 3);})
    .then((resultB)=>{return addAsync(result, 4);})
    .then((resultC)=>{console.log(resultC);});
//NO PYRAMID CODE
```