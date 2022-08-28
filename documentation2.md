# Object.keys, values, entries
1. 
    * Object.keys(obj) – returns an array of keys.
    * Object.values(obj) – returns an array of values.
    * Object.entries(obj) – returns an array of [key, value] pairs.

2. Destructuring assignment
    * Array destructuring:
        ```js
        let [firstName, surname] = "John Smith".split(' ');
        alert(firstName); // John
        alert(surname);  // Smith
        ```

        ```js
        let user = {
          name: "John",
          age: 30
        };      

        // loop over keys-and-values
        for (let [key, value] of Object.entries(user)) {
          alert(`${key}:${value}`); // name:John, then age:30
        }
        ```

        ```js
        let [name1, name2, ...rest] = ["Julius", "Caesar", "Consul", "of the Roman Republic"];

        // rest is array of items, starting from the 3rd one
        alert(rest[0]); // Consul
        alert(rest[1]); // of the Roman Republic
        alert(rest.length); // 2
        ```
    * Object destructuring:
        - If we want to assign a property to a variable with another name
        ```js
        let options = {
          title: "Menu",
          width: 100,
          height: 200
        };         

        // { sourceProperty:           targetVariable }
        let {width: w, height: h, title} =         options;       

        // width -> w
        // height -> h
        // title -> title          

        alert(title);  // Menu
        alert(w);      // 100
        alert(h);      // 200
        ```

        ```js
        let options = {
          title: "Menu"
        };         

        let {width = prompt("width?"),         title = prompt("title?")} =        options;   

        alert(title);  // Menu
        alert(width);  // (whatever the            result of prompt is)
        ```

        ```js
        let options = {
          title: "Menu",
          height: 200,
          width: 100
        };      

        // title = property named title
        // rest = object with the rest of       properties
        let {title, ...rest} = options;     

        // now title="Menu", rest={height:      200, width: 100}
        alert(rest.height);  // 200
        alert(rest.width);   // 100
        ```


# JSON methods, toJSON
1. 
    JSON.stringify to convert objects into JSON string
        + apply for: Objects, Array, Primitive types
        + except: Function properties (methods),Symbolic keys and values, Properties that store undefined.


    ```js
    let user = {
      sayHi() { // ignored
        alert("Hello");
      },
      [Symbol("id")]: 123, // ignored
      something: undefined // ignored
    };  

    alert( JSON.stringify(user) ); //   {} (empty object)
    ```

    + nested objects are supported and converted automatically


          ```js
          let meetup = {
            title: "Conference",
            room: {
              number: 23,
              participants: ["john",    "ann"]
            }
          };  

          alert( JSON.stringify     (meetup) );
          /* The whole structure is         stringified:
          {
            "title":"Conference",
            "room":{"number":23   ,     "participants":["john",       "ann"]},
          }
          */
              ```
        + replacer in JSON.stringify(value, replacer, space) only convert specifed properties:

            
            ```js
            let room = {
                  number: 23
                };              

                let meetup = {
                  title: "Conference",
                  participants: [{name: "John"},                 {name: "Alice"}],
                  place: room // meetup references               room
                };              

                room.occupiedBy = meetup; // room               references meetup           

                alert( JSON.stringify(meetup,               ['title', 'participants', 'place',              'name', 'number']) );
                /*
                {
                  "title":"Conference",
                  "participants":[{"name":"John"}               ,{"name":"Alice"}],
                  "place":{"number":23}
                }
                */
                ```


        + The third argument of JSON.stringify(value, replacer, space) is the number of spaces to use for pretty formatting.
                
            ```js
                let user = {
                  name: "John",
                  age: 25,
                  roles: {
                    isAdmin: false,
                    isEditor: true
                  }
                };              

                alert(JSON.stringify(user, null, 2));
                /* two-space indents:
                {
                  "name": "John",
                  "age": 25,
                  "roles": {
                    "isAdmin": false,
                    "isEditor": true
                  }
                }
                */              

                /* for JSON.stringify(user, null, 4) the result would be more indented:
                {
                    "name": "John",
                    "age": 25,
                    "roles": {
                        "isAdmin": false,
                        "isEditor": true
                    }
                }
                */
            
            ```


    - JSON.parse to convert JSON back into an object.

        ```js
        // stringified array
        let numbers = "[0, 1, 2, 3]";

        numbers = JSON.parse(numbers);

        alert( numbers[1] ); // 1
        ```

        + Using reviver:
        ```js
        let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

        let meetup = JSON.parse(str);       

        alert( meetup.date.getDate() ); //      Error!
        ```

        ```js
        let str = '{"title":"Conference","date":"2017-11-30T12:00:00.000Z"}';

        let meetup = JSON.parse(str,           function(key, value) {
          if (key == 'date') return new            Date(value);
          return value;
        });        

        alert( meetup.date.getDate() ); //         now works!
        ```

# Date and Time:
  1. Creation:
    - new Date():
      ```js
      let now = new Date();
      alert( now ); // shows current date/time
      ```
    - new Date(milliseconds)
      ```js
      // 0 means 01.01.1970 UTC+0
        let Jan01_1970 = new Date(0);
        alert( Jan01_1970 );   // 1970-01-01T00:00:00.000Z

        // now add 24 hours, get 02.01.1970 UTC+0
        let Jan02_1970 = new Date(24 * 3600 * 1000);
        alert( Jan02_1970 );
        // 1970-01-02T00:00:00.000Z


        ```

      - new Date(datestring)
        ```JS
        let date = new Date("2017-01-26");
        alert(date);
          // Thu Jan 26 2017 11:00:00 GMT+1100 (Australian Eastern Daylight Time)
          // or
          // Wed Jan 25 2017 16:00:00 GMT-0800          (Pacific Standard Time)
        ```

        * new Date(year, month, date, hours, minutes, seconds, ms) 
          - Only the first two arguments are obligatory.

          - The month count starts with 0 (Jan), up to 11 (Dec).

    2. Access date components:
      - getFullYear()
        Get the year (4 digits)
      - getMonth()
        Get the month, from 0 to 11.
      - getDate()
        Get the day of month, from 1 to 31, the         name of the method does look a little bit         strange.
      - getHours(), getMinutes(), getSeconds(),         getMilliseconds()
        Get the corresponding time components.

      - getDay()
        Get the day of week, from 0 (Sunday) to 6         (Saturday)
    * Use for local timezone:
      ```js
      // current date
        let date = new Date();        

        // the hour in your current time zone
        alert( date.getHours() );       

        // the hour in UTC+0 time zone (London time         without daylight savings)
        alert( date.getUTCHours() );
      ```

      * Date.parse from a string
      - The method Date.parse(str) can read a date from a string.

      - The string format should be: YYYY-MM-DDTHH:mm:ss.sssZ, where:

      - YYYY-MM-DD – is the date: year-month-day.
      - The character "T" is used as the delimiter.
      - HH:mm:ss.sss – is the time: hours, minutes, seconds and milliseconds.
      - The optional 'Z' part denotes the time zone in the format +-hh:mm. A single letter Z would mean UTC+0.
      - Shorter variants are also possible, like YYYY-MM-DD or YYYY-MM or even YYYY.

      ```js
      let ms = Date.parse('2012-01-26T13:51:50.417-07:00');

      alert(ms); // 1327611110417  (timestamp)
      ```

6.2 Rest parameters and spread syntax 

- Math.max(arg1, arg2, ..., argN) – returns the greatest of the arguments.
- Object.assign(dest, src1, ..., srcN) – copies properties from src1..N into dest.

- The “arguments” variable:
  + There is also a special array-like object named arguments that contains all arguments by their index.

  ```js
      function showName() {
      alert( arguments.length );
      alert( arguments[0] );
      alert( arguments[1] );    

      // it's iterable
      // for(let arg of arguments) alert(arg);
    }   

    // shows: 2, Julius, Caesar
    showName("Julius", "Caesar");   

    // shows: 1, Ilya, undefined (no second     argument)
    showName("Ilya");
  ```
- Rest parameters ...
  + When ... is at the end of function parameters, it’s “rest parameters” and gathers the rest of the list of arguments into an array.

  + Rest parameters are used to create functions that accept any number of arguments.
    ```js
    function showName(firstName, lastName, ...titles) {
      alert( firstName + ' ' + lastName ); //     Julius Caesar   

      // the rest go into titles array
      // i.e. titles = ["Consul", "Imperator"]
      alert( titles[0] ); // Consul
      alert( titles[1] ); // Imperator
      alert( titles.length ); // 2
    }   

    showName("Julius", "Caesar", "Consul",    "Imperator");
    ```
- Spread syntax
    + When ... occurs in a function call or alike, it’s called a “spread syntax” and expands an array, object into a list.

    ```js
    let obj = { a: 1, b: 2, c: 3 };

    let objCopy = { ...obj }; // spread the      object into a list of parameters
                              // then return the result in a new object    

    // do the objects have the same contents?
    alert(JSON.stringify(obj) === JSON.stringify     (objCopy)); // true    

    // are the objects equal?
    alert(obj === objCopy); // false (not same     reference)   

    // modifying our initial object does not     modify the copy:
    obj.d = 4;
    alert(JSON.stringify(obj)); // {"a":1,"b":2,     "c":3,"d":4}
    alert(JSON.stringify(objCopy)); // {"a":1,     "b":2,"c":3}
    ```

5. Global Object:
  - We should store values in the global object only if they’re truly global for our project. And keep their number at minimum.
  - In-browser, unless we’re using modules, global functions and variables declared with var become a property of the global object.

  6. Function object: 

      - The “name” property

       ```js
            function sayHi() {
              alert("Hi");
            }     

            alert(sayHi.name); // sayHi
       ```
     
      - The “length” property: 
        + There is another built-in property “length” that returns the number of function parameters
        + Here we can see that rest parameters are not counted.
        ```js
        function f1(a) {}
        function f2(a, b) {}
        function many(a, b, ...more) {}       

        alert(f1.length); // 1
        alert(f2.length); // 2
        alert(many.length); // 2
        ```
      - Custom properties:
        ```js
        function makeCounter() {

          function counter() {
            return counter.count++;
          };        

          counter.count = 0;        

          return counter;
        }       

        let counter = makeCounter();        

        counter.count = 10;
        alert( counter() ); // 10
        ```
      - NFE: Named Function Expression, or NFE, is a term for Function Expressions that have a name.

        ```js
          let sayHi = function func(who) {
            alert(`Hello, ${who}`);
          };
        ```
        + There are two special things about the name func, that are the reasons for it:
          * It allows the function to reference itself internally.
          * It is not visible outside of the function.
        ```js
        let sayHi = function func(who) {
          if (who) {
            alert(`Hello, ${who}`);
          } else {
            func("Guest"); // use func to re-call         itself
          }
        };        

        sayHi(); // Hello, Guest        

        // But this won't work:
        func(); // Error, func is not defined (not        visible outside of the function)
        ```
        + func is different with sayHi because func is not access from outer code but sayHi can do this.
        ```js
        let sayHi = function func(who) {
          if (who) {
            alert(`Hello, ${who}`);
          } else {
            func("Guest"); // Now all fine
          }
        };        

        let welcome = sayHi;
        sayHi = null;       

        welcome(); // Hello, Guest (nested call         works)
        ```




