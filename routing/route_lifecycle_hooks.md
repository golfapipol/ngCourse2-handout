# Route Lifecycle Hooks

Angular 2 routing supports two lifecycle hooks: the `@CanActivate` decorator
and the `CanDeactivate` interface.

These are places where you can perform logic to see if the user is able to navigate to or away from the component.

An example use case for `@CanActivate` would be doing a permissions check. A
potential use case for `routerCanDeactivate` might be is to confirm if a user
wants to leave a form where s/he has entered unsaved data.

Both of these hooks will receive the next and previous `componentInstruction`,
which represent the route you are navigating from and to, respectively.

## Example componentInstruction

```javascript
{
  "urlPath": "componentTwo/Route Params In Message",
  "urlParams": [],
  "terminal": true,
  "specificity": 10099,
  "params": {
    "message": "Route Params In Message"
  },
  "reuse": false,
  "routeData": {
    "data": {
      "passedData": "Passed in via Data"
    }
  }
}
```
## `@CanActivate`

Angular 2 uses `@CanActivate` as a decorator, because it is called before an instance if the class is actually created.

If the function returns a true value or a promise that resolves, it will allow the component to activate.
If a false value or a promise that gets rejected is returned, the component will not activate.

```javascript
import {RouteParams, RouteData, CanActivate, CanDeactivate} from 'angular2/router';

@CanActivate((next: ComponentInstruction, prev: ComponentInstruction)=>{
  return confirm('are you sure you want to go here?')
})
export default class ComponentTwo implements CanDeactivate {
 // ...
}
```

## CanDeactivate

To use the `CanDeactivate` hook, your component needs to implement the `CanDeactivate` interface, which means your component needs to have a `routerCanDeactivate` method available on it.

Just like the `@CanActivate`, if a true value/resolving promise is returned, the router will navigate away.
If a false value/rejecting promise is returned, it will stop navigation away from the component.

```javascript
import {RouteParams, RouteData, CanActivate, CanDeactivate} from 'angular2/router';
export default class ComponentTwo implements CanDeactivate {
// ...
routerCanDeactivate(next: ComponentInstruction, prev: ComponentInstruction) {
    return confirm('Are you sure you want to leave?');
  }
}
```
[View Example](http://plnkr.co/edit/BaW7I2x7dEIHyoeR9SNW?p=preview)