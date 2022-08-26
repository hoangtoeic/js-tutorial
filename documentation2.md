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