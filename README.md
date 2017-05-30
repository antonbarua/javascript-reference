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
    