# Observables Array Operations
In addition to simply iterating over an asynchronous collection, we can perform other operations such as filter or map and many more as defined in the RxJS API. This is what bridges observable with the Iterable pattern, and lets us conceptualize them as collections.

Let's expand our example and do something a little more with our stream:

```js
import {Component} from 'angular2/core';
import {Http, Headers} from 'angular2/http';
import 'rxjs/Rx';

@Component({
	selector: 'app-root',
	template: `
	  <b>Angular 2 HTTP requests using RxJs Observables!</b>
	  
	  <div><code>{{doctors.toString()}}</code></div>
	  `
})

export class AppComponent {
  private doctors = [];
  
  constructor(http: Http) {
    http.get('http://jsonplaceholder.typicode.com/users/')
        .flatMap((data) => data.json())
        .filter((person) => person.id > 5)
        .map((person) => "Dr. " + person.name)
        .subscribe((data) => {
          this.doctors.push(data);
        });
  }
}
```
[View Example](http://plnkr.co/edit/d3pigxK1JhfAeVMihGGq?p=preview)

Here are two really useful array operations - `map` and `filter`. What exactly do these do?

`map` will create a new array with the results of calling a provided function on every element in this array. In this example we have used it to create a new result set by iterating through each item and appending the "Dr." abbreviation in front of every users name. Now every object in our array has "Dr." prepended to whatever the value is of its name property. 

`filter` will create a new array with all elements that pass the test implemented by a provided function. Here we have used it to create a new result set by excluding any user whose `id` property is less than 6. 

Now when our `subscribe` callback gets invoked, the data it receives will be a list of JSON objects whose `id` properties are greater or equal to 5 and whose `name` properties have been pre-pended with `Dr. `. 

Note the chaining function style, and the optional static typing that comes with TypeScript we have used in this example. Most importantly functions like filter return an observable, as in observables beget other observables, similarly to promises. In order to use `map` and `filter` in a chaining sequence we have flattened the results of our Observable using `flatMap`. Since `filter` accepts an Observable, and not an array, we have to convert our array of JSON objects from `data.json()` to an Observable stream. This is done with `flatMap`.

There are many other array operations you can employ in your Observables, look for them in the [RxJS API](https://github.com/Reactive-Extensions/RxJS). 

[rxmarbles.com](http://rxmarbles.com) is a helpful resource to understand how the operations work.