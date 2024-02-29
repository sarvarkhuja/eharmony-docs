# Formatting and naming

### If you are passing data to a component, use property binding only if necessary <a href="#if-you-are-passing-data-to-a-component-use-property-binding-only-if-necessary" id="if-you-are-passing-data-to-a-component-use-property-binding-only-if-necessary"></a>

In the following example, property binding is unnecessary because we are assigning a string value.

```
<!-- bad -->
<my-app-component [name]="'Steve'"></my-app-component>

<!-- good -->
<my-app-component name="Steve"></my-app-component>
```

### Prefix output handlers with `on` <a href="#prefix-output-handlers-with-codeoncode" id="prefix-output-handlers-with-codeoncode"></a>

```
<!-- bad -->
<my-app-component (somethingHappened)="somethingHappened($event)">

<!-- bad -->
<my-app-component (onSomethingHappened)="somethingHappened($event)">

<!-- bad -->
<my-app-component (onSomethingHappened)="onSomethingHappened($event)">

<!-- good -->
<my-app-component (somethingHappened)="onSomethingHappened($event)">
```

### Name click handlers in a short and descriptive manner <a href="#name-click-handlers-in-a-short-and-descriptive-manner" id="name-click-handlers-in-a-short-and-descriptive-manner"></a>

```
<!-- bad -->
<button (click)="onClick()">Log in</button>

<!-- bad - use infinitive instead of past form -->
<button (click)="onLogInClicked()">Log in</button>

<!-- bad - too many words -->
<button (click)="onLogInButtonClick()">Log in</button>

<!-- good -->
<button (click)="onLogInClick()">Log in</button>
```

### Use the optional chaining operator <a href="#use-the-optional-chaining-operator" id="use-the-optional-chaining-operator"></a>

```
<!-- bad -->
<div *ngIf="user && user.loggedIn"></div>

<!-- good -->
<div *ngIf="user?.loggedIn"></div>
```

### Use `ng-container` when possible to reduce the amount of unnecessary DOM elements <a href="#use-codeng-containercode-when-possible-to-reduce-the-amount-of-unnecessary-dom-elements" id="use-codeng-containercode-when-possible-to-reduce-the-amount-of-unnecessary-dom-elements"></a>

```
<!-- bad - span is unnecessary -->
<span *ngIf="something"></span>

<!-- good - span was unnecessary -->
<ng-container *ngIf="something"></ng-container>

<!-- good - span is necessary because of the class -->
<span *ngIf="something" class="i-need-this-class"></span>
```

### Use `*ngIf; else` <a href="#use-codengif-elsecode" id="use-codengif-elsecode"></a>

```
<!-- bad - we have to type the same condition twice and check it twice -->
<ng-container *ngIf="something"></ng-container>
<ng-container *ngIf="!something"></ng-container>

<!-- good -->
<ng-container *ngIf="something; else anotherThing"></ng-container>
<ng-template #anotherThing></ng-template>
```

### Use attributes for transclusion selectors <a href="#use-attributes-for-transclusion-selectors" id="use-attributes-for-transclusion-selectors"></a>

When implementing transclusion with manual content selection via the `select` attribute of the `ng-content` element, we recommend using attribute selectors.

The `my-app-wrapper` component template:

```
<div class="wrapper">
  <p>Wrapper content:</p>
  <ng-content select="[some-stuff]"></ng-content>
</div>
```

Some other component's template:

```
<my-app-wrapper>
  <div class="some-item" some-stuff>Some stuff</div>
</my-app-wrapper>
```

### Order/group attributes and bindings <a href="#ordergroup-attributes-and-bindings" id="ordergroup-attributes-and-bindings"></a>

1. Structural directives (`*ngFor`, `*ngIf`, etc.)
2. Animation triggers (`@fade`, `[@fade]`)
3. Element reference (`#myComponent`)
4. HTML attributes (`class`, etc.)
5. Non-interpolated string inputs and attributes (`foo="bar"`)
6. Interpolated inputs (`[bar]="theBar"`)
7. Two-way bindings (`[(ngModel)]="email"`)
8. Outputs (`(click)="onClick()"`)

```
<!-- bad -->
<my-app-component
  (click)="onClick($event)"
  [(ngModel)]="fooBar"
  class="foo"
  #myComponent
  [bar]="theBar"
  @fade
  foo="bar"
  *ngIf="shouldShow"
  (someEvent)="onSomeEvent($event)"
></my-app-component>

<!-- good -->
<my-app-component
  *ngIf="shouldShow"
  @fade
  #myComponent
  class="foo"
  foo="bar"
  [bar]="theBar"
  [(ngModel)]="fooBar"
  (click)="onClick($event)"
  (someEvent)="onSomeEvent($event)"
></my-app-component>
```

### `[class]` vs `[ngClass]` syntax <a href="#codeclasscode-vs-codengclasscode-syntax" id="codeclasscode-vs-codengclasscode-syntax"></a>

If you have to set just one class depending on some condition, `[class.class-name]="condition"` is the way to go. Example:

```
<div [class.active]="isActive"></div>
```

If you need to set many classes, `[ngClass]="{ ... }"` is the way to go. Example:

```
<div [ngClass]="{ 'active': isActive, 'main-item': isMainItem, 'accent': isAccent }"></div>
```



### Prefer interface over type <a href="#prefer-interface-over-type" id="prefer-interface-over-type"></a>

When defining data structures, prefer using interfaces over types. Use types only for those things for which interfaces cannot be used—unions of different types.

```
// bad
export type User = {
  name: string;
};

// good
export interface IUser {
  name: string;
}

// good
export type CSSPropertyValue = number | string;
```

You might be tempted to use a type for a union of models. For example, in some CMS solutions you might have `AdminModel` and `AuthorModel`. The user can log in using either admin or author credentials.

```
export class AdminModel {
  public id: string;
  public permissions: Array<Permission>;
}

export class AuthorModel {
  public id: string;
  public posts: Array<Post>;
}

export type User = AdminModel | AuthorModel;

// in some component
@Input() public user: User;

// in template
User id: {{ user.id }}
```

This might seem fine at first, but it is actually an anti-pattern. What you should actually do in a case like this is create an abstraction above `AdminModel` and `AuthorModel`:

```
export abstract class User {
  public id: string;
}

export class AdminModel extends User {
  public permissions: Array<Permission>;
}

export class AuthorModel extends User {
  public posts: Array<Post>;
}
```

**TL;DR:** Prefer interfaces and abstractions over types. Use types only when you need union types.

### Ordering the class members (including getters and lifecycle hooks) <a href="#ordering-the-class-members-including-getters-and-lifecycle-hooks" id="ordering-the-class-members-including-getters-and-lifecycle-hooks"></a>

Follow this guideline when ordering class members:

1. `@Input`s
2. `@Output`s
3. properties
4. constructor
5. getters and setters
6. lifecycle hooks
7. methods

Example:

```
class MyComponent implements OnInit, OnChanges {
  @Input() public i1: string;
  @Input() public i2: string;
  @Output() public o2: EventEmitter<string> = new EventEmitter();
  public attr1: number;
  protected attr2: string;
  private attr3: Date;

  constructor(...) { ... }

  public get computedProp(): string { ... }

  private get secretComputedProp(): number { ... }

  public ngOnInit(): void { ... }

  public ngOnChanges(): void { ... }

  public onSomeButtonClick(event: MouseEvent): void { ... }

  private someInternalAction(): void { ... }
}
```

### Extract ngOnChanges logic into methods <a href="#extract-ngonchanges-logic-into-methods" id="extract-ngonchanges-logic-into-methods"></a>

If `ngOnChanges` contains some logic, it is often a good idea to separate that logic into private methods to increase readability of `ngOnChanges` method.

```
// bad
@Component(...)
class MyComponent {
  @Input() public user: UserModel;
  public userForm: FormGroup;

  constructor(private fb: FormBuilder) {}

  public ngOnChanges(changes: SimpleChanges) {
    if (changes.user) {
      this.userForm = this.fb.group({
        name: this.user.name,
        ...
      });
    }
  }
}
```

```
// good
@Component(...)
class MyComponent {
  @Input() public user: UserModel;
  public userForm: FormGroup;

  constructor(private fb: FormBuilder) {}

  public ngOnChanges(changes: SimpleChanges) {
    if (changes.user) {
      this.createUserForm();
    }
  }

  private createUserForm(): void {
    this.userForm = this.fb.group({
      name: this.user.name,
      ...
    });
  }
}
```

We can even go one step further and make things _pure_:

```
// even better
@Component(...)
class MyComponent {
  @Input() public user: UserModel;
  public userForm: FormGroup;

  constructor(private fb: FormBuilder) {}

  public ngOnChanges(changes: SimpleChanges) {
    if (changes.user) {
      this.userForm = this.createUserForm(changes.user.currentValue);
    }
  }

  private createUserForm(user: UserModel): FormGroup {
    return this.fb.group({
      name: user.name,
      ...
    });
  }
}
```

### Prefer using `ngOnChanges` over `ngOnInit` <a href="#prefer-using-codengonchangescode-over-codengoninitcode" id="prefer-using-codengonchangescode-over-codengoninitcode"></a>

In this example we want to react on input changes and call some action.

```
//bad
@Component(...)
class MyComponent {
  @Input() public label: string;

  public ngOnChanges(changes: SimpleChanges) {
    if (changes.label) {
      this.someActions()
    }
  }

  public ngOnInit() {
    this.someActions()
  }

  private someActions() { ... }
}
```

Problem with the above example is that someActions will be called twice, first in `ngOnChanges` and then in `ngOnInit`. Yes, this order seems a bit strange - `ngOnInit` is called **after** the first `ngOnChanges` call; but this is just how it is.

```
//good
@Component(...)
class MyComponent {
  @Input() public label: string;

  public ngOnChanges(changes: SimpleChanges) {
    if (changes.label) {
      this.someActions()
    }
  }

  private someActions() { ... }
}
```

Look at your use cases and check if you can remove `ngOnInit`. You almost never need `ngOnInit` (more on this in the following sub-chapter). If there are some default input values which should be set, you can do it alongside input declaration (`@Input() label: string = 'default label'`). Any other reactions to input changes should be done in `ngOnChanges`.

### Consider different initial assignment options <a href="#consider-different-initial-assignment-options" id="consider-different-initial-assignment-options"></a>

There is often a need to assign some properties during component initialization. This can be done in multiple ways:

1. In component's constructor
2. During property declaration
3. In lifecycle hooks
4. Subscribing at initialization

When learning Angular, you hear a lot about different lifecycle hooks. Even when creating new components using the Angular CLI, you get a component with empty `constructor` and empty `ngOnInit` method. Because of this, it seems natural to use one of these for initialization. We will explore what the options for assignment on initialization and what is the best way to do it.

**Example #1** - how a constructor might be used for initial value assignment:

```
@Component(...)
class MyComponent {
  private foo: string;

  constructor() {
    this.foo = 'bar';
  }
}
```

This works and there is nothing wrong with this, but it can be written shorter like so:

**Example #2** - initial assignment alongside property declaration

```
@Component(...)
class MyComponent {
  private foo: string = 'bar';
}
```

We did pretty much the same thing, but a bit shorter.

**Example #3** - order during initial assignment alongside property declaration

```
@Component(...)
class MyComponent {
  private foo: string = 'bar';

  constructor() {
    this.foo = 'foo';
  }
}
```

Property assignments will be executed immediately after constructor execution has finished, so in this example the final value of `foo` will be `bar`.

We recommend setting initial values alongside property declaration since it is the shortest option.

However, there are some cases where you will have to use `ngOnInit` or `ngOnChanges`.

**Example #4** - when are `ngOnInit` and `ngOnChanges` necessary?

```
// bad
@Component(...)
class PersonDetailsComponent {
  @Input() public birthDate: Date;
  public isLegalAge: boolean;

  constructor() {
    this.isLegalAge = new Date().getYear() - this.birthDate.getYear() > 18;
    // This check is not 100% correct, we are keeping it simple.
  }
}
```

If you try this, you will get an exception because `birthDate` will be undefined at component construction time. Input binding values are available only after `ngOnInit` and `ngOnChanges` are called. To make this work, you can use `ngOnInit`:

```
// bad
@Component(...)
class PersonDetailsComponent implements OnInit {
  @Input() public birthDate: Date;
  public isLegalAge: boolean;

  ngOnInit() {
    this.isLegalAge = new Date().getYear() - this.birthDate.getYear() > 18;
  }
}
```

Using `ngOnInit` works and we get no exception (with the assumption that a valid `Date` object is passed down to our component via `birthDate` input).

We still have one problem with this solution. If input binding changes again (after the initial change), `isLegalAge` will not be re-assigned. To solve this we simply switch to using `ngOnChanges`:

```
// good
@Component(...)
class PersonDetailsComponent implements OnChanges {
  @Input() public birthDate: Date;
  public isLegalAge: boolean;

  ngOnChanges(changes: SimpleChanges) {
    if (changes.birthDate) {
      this.isLegalAge = this.checkIfLegalAge(changes.birthDate.currentValue);
    }
  }

  private checkIfLegalAge(birthDate): boolean {
    return new Date().getYear() - birthDate.getYear() > 18;
  }
}
```

Since this check is simple and returns a primitive value, using a getter here is also a valid option and it allows us to remove ngOnChanges completely and reduce the amount of code.

```
// even better
@Component(...)
class PersonDetailsComponent {
  @Input() public birthDate: Date;

  public get isLegalAge(): boolean {
    return new Date().getYear() - this.birthDate.getYear() > 18;
  }
}
```

But be careful when using getters for values which are used in templates because they will be called very often. This is OK only if there is not too much heavy lifting inside getters and if the getter does not return a newly initialized object every time it is called.

To sump up, we recommend using ngOnChanges for computing values based on input changes. You can also use getters if they return primitive values and if the computation is quick and simple.

### Postfix observables with `$` <a href="#postfix-observables-with-codecode" id="postfix-observables-with-codecode"></a>

When you want to observe a value, use some of the classes derived from `Observable`. Most use cases are covered with one of these:

* `Subject`
* `BehaviorSubject`
* `ReplaySubject`

You can subscribe to all classes derived from `Observable` in order to receive value changes. You can also use `operators` to manipulate how and when the value changes are sent to the subscribers.

There is a naming convention for `Observable` variables—postfixing with a dollar sign:

```
// bad
const observable: Observable;

// good
const observable$: Observable;

// good
const mySubject$: Subject;
```

The `$` suffix is here to indicate and let you know it is something you can subscribe to, but it does not let you know if you can change the observable in case it is some type of a `Subject`, e.g. `ReplaySubject` or `BehaviorSubject`. This naming convention goes along with the `no-exposed-subjects` RxJS ESLint rule. The aforementioned rule will cause an error in case you have a publicly exposed`Subject`. The reasoning behind this is that subjects should never be publicly exposed unless converted to an observable using some pipeable operators or, for example, `asObservable` function.

```
class ExampleComponent {
  private _isLoading$ = new BehaviorSubject(false); // for internal use for "state management"
  public isLoading$ = this._isLoading$.pipe(debounceTime(250)); // for use in template

  // to be called from some methods, for example during data fetching
  private updateLoadingState(state) {
    this._isLoading$.next(state);
  }
}
```

The potential downside of this convention/pattern is the name separation, in case of a naming conflict, the private member name should be prefixed with the `_`.

### Get to know the RxJS operators <a href="#get-to-know-the-rxjs-operators" id="get-to-know-the-rxjs-operators"></a>

To make good use of RxJS and develop the application in a reactive way, it is important to utilize the operators. RxJS operators allow you to modify the stream of data in multiple ways. Some common operators are:

* `map`
* `filter`
* `tap` (replaces `.do`)
* `delay`
* `debounceTime`
* `distinctUntilChanged`
* `catchError` (replaces `.catch`)
* `switchMap`

You can also write your own operators. If you do so, it is strongly recommended to write them as [pure pipeable operator functions](https://github.com/ReactiveX/rxjs/blob/master/doc/operator-creation.md#operator-as-a-pure-function).

Covering all the various operators is out of the scope of this handbook. Please refer to some of the learning resources mentioned earlier \[[1](https://infinum.com/handbook/books/frontend/angular/getting-started-with-angular/get-to-know-rxjs)] \[[2](https://infinum.com/handbook/books/frontend/angular/angular-guidelines-and-best-practices/core-libraries-configuration-and-tools#a-hrefhttpsgithubcomreactivexrxjs-target\_blankrxjsa)].

### Don't forget to use Destroy service <a href="#tap-vs-subscribe" id="tap-vs-subscribe"></a>

````
```typescript
    constructor(
        @Self() @Inject(DestroyService) private readonly destroy$: DestroyService,
    ) {}
```
````

### Don't forget to add resolution modifers

````
```typescript
    constructor(
        @Optional() @Inject(TUI_ELEMENT_REF) el: ElementRef<HTMLElement> | null,
        @Host() @Inject(TUI_IS_IOS) isIos: boolean,
        @Inject(ElementRef) {nativeElement}: ElementRef<HTMLElement>,
        @Inject(Renderer2) renderer: Renderer2,
        @Self() @Inject(DestroyService) destroy$: DestroyService,
    )
```
````

### tap() vs subscribe() <a href="#tap-vs-subscribe" id="tap-vs-subscribe"></a>

When to use the `tap` operator in Rx flow? As per the [official documentation for the `tap` operator](https://rxjs.dev/api/operators/tap), it should be used when performing "side-effects". This means that the operator should be used when the user wants to "affect outside state with a notification without altering the notification". Although side-effects can be performed inside of the map operator, we should strive to write pure functions wherever possible and have a clear separation of responsibilities. This is one of the cases where the `tap` operator comes in handy. Since the `tap` operator's callback is the same as the subscriber's `next` callback, nothing stops us from writing the same logic in either of those places. Well, nothing besides semantics. When writing Rx flows, we should convey as much information as we can just through our code, which means - use `tap` for performing side-effects and debugging/logging mostly, whereas subscribe represents the end of the flow which means that we have probably received some data which can then be used in application. This makes subscribe callback the correct place to do something with the data you've received through piped operators. This is also why an empty subscribe call is basically a code smell. Of course, there are exceptions when you cannot avoid using an empty subscribe(), but you should be mindful of it.

```
// Bad
source$.pipe(
  tap(() => {
    this.router.navigate(['some-route']);
  }),
).subscribe();
```

In the example above, we are navigating to a new location before we've reached the end of the flow which doesn't make too much sense. We could possibly add another operator in the flow which would make this approach even more wrong, especially if you forget to reorder operators so that the `tap` comes after all of the newly added mapping operators.

```
// Better
source$.subscribe(() => {
  this.router.navigate(['some-route']);
})
```

The example above is more correct. Navigation and other, similar actions should be executed at the end of the Rx flow, which is in the subscribe's `next` callback.

### Observables and async/await <a href="#observables-and-asyncawait" id="observables-and-asyncawait"></a>

You can use async/await to await for the completion of observables. Keep in mind that this probably does not make too much sense for observables that emit more than one value, but it might be OK for things like HTTP requests.

To convert an observable to a promise, just call `obs$.toPromise()` and then you can await it.

One important thing to note is that you are waiting for the **completion**, not **emission** of values. Knowing that, what do you expect the result to be in the following example?

```
const source$: Subject<string> = new Subject();
const sourcePromise: Promise<string> = source$.toPromise();

source$.next('foo');
source$.next('bar');
source$.complete();

const result = await sourcePromise;
```

Spoiler alert! The result will be `bar`, and if we do not complete the source observable, awaiting will hang indefinitely.

### Avoid manual subscriptions and asynchronous property assignment <a href="#avoid-manual-subscriptions-and-asynchronous-property-assignment" id="avoid-manual-subscriptions-and-asynchronous-property-assignment"></a>

We recommend avoiding making manual subscription calls and asynchronous property assignment. The code example below has some comments indicating what is meant by "manual subscription" and "asynchronous property assignment".

It is best to leave subscription handling to the `async` pipe because it:

* Encourages [reactive programming](https://medium.com/front-end-weekly/why-should-we-use-reactive-programming-in-angular-2e2913297054)
* Handles unsubscribing automatically
* Works well with `OnPush` change detection
* Avoids the need to do asynchronous property assignment

Component:

```
// bad
@Component(...)
export class ArticlesList {
  public articles: Array<Article>;

  constructor(...) {
    this.articleService.fetchArticles().subscribe((articles) => { // manual subscription
      this.articles = articles; // asynchronous property assignment
    });
  }
}

// good
@Component(...)
export class ArticlesList {
  public readonly articles$: Observable<Array<Article>> = this.articleService.fetchArticles();

  constructor(...) { }
}
```

Template:

```
<!-- bad -->
<my-app-article-details
  *ngFor="let article of articles"
  [article]="article"
></my-app-article-details>

<!-- good -->
<my-app-article-details
  *ngFor="let article of articles$ | async"
  [article]="article"
></my-app-article-details>

```

### Avoid unnecessary creation of multiple observables and avoid subscriptions <a href="#avoid-unnecessary-creation-of-multiple-observables-and-avoid-subscriptions" id="avoid-unnecessary-creation-of-multiple-observables-and-avoid-subscriptions"></a>

Taking the learnings from all the previous examples and the fact that we can use services during initial assignment, we can greatly simplify initialization of observables and their usage in templates.

In this example, we have to fetch the user by ID. The ID is read from route parameters.

```
// bad
@Component({
  template: `
  <ng-container *ngIf="user">
    {{ user.name }}
  </ng-container>
  `,
  ...
})
class MyComponent {
  public user: UserModel;
  private userSubscription: Subscription;

  constructor(
    private route: ActivatedRoute,
    private usersService: UsersService,
  ) {
    this.userSubscription = this.route.params.pipe(switchMap((params: Params) => {
      return this.usersService.fetchById(params.userId);
    })).subscribe((user) => {
      this.user = user;
    });
  }

  ngOnDestroy(): void {
    this.userSubscription.unsubscribe();
  }
}
```

This is bad because it will not work correctly with OnPush change detection and we have to take care of subscriptions manually - it is easy to forget to unsubscribe and we have to introduce `ngOnDestroy`.

```
// even worse
@Component({
  template: `
  <ng-container *ngIf="user$ | async as user">
    {{ user.name }}
  </ng-container>
  `,
  ...
})
class MyComponent {
  constructor(
    private route: ActivatedRoute,
    private usersService: UsersService,
  ) {}

  public get user$(): Observable<UserModel> {
    return this.route.params.pipe(switchMap((params: Params) => {
      return this.usersService.fetchById(params.userId);
    }));
  }
}
```

Here we no longer have to take care of unsubscribing, but the issue now is that during each template check, `user$` getter will be called. In _Example #4_ in one of the previous sub-chapters we had a case where using a getter was fine, but in this case it is not. Each time it is called it creates a new observable, template will subscribe to this new observable during each change detection, meaning that a new subscription will trigger another API call every CD cycle - which is unnecessary.

```
// good
@Component({
  template: `
  <ng-container *ngIf="user$ | async as user">
    {{ user.name }}
  </ng-container>
  `,
  ...
})
class MyComponent {
  public user$: Observable<UserModel> = this.route.params.pipe(switchMap((params: Params) => {
    return this.usersService.fetchById(params.userId);
  }));

  constructor(
    private route: ActivatedRoute,
    private usersService: UsersService,
  ) {}
}
```

Finally the correct solution simply assigns user$ to an observable which is created only once. The template will have only one subscription and we will just react to updates in the data stream which happen when params change.

If the observable creation pipeline gets larger, you can move it to a private method like so:

```
// good
@Component(...)
class MyComponent {
  public user$: Observable<UserModel> = this.createUserObservable();

  constructor(
    private route: ActivatedRoute,
    private usersService: UsersService,
  ) {}

  private createUserObservable(): Observable<UserModel> {
    return this.route.params.pipe(switchMap((params: Params) => {
      return this.usersService.fetchById(params.userId);
    }));
  }
}
```

### Subscribe as late as possible and use the operators <a href="#subscribe-as-late-as-possible-and-use-the-operators" id="subscribe-as-late-as-possible-and-use-the-operators"></a>

If you are new to RxJS, you will probably be overwhelmed by the amount of operators and the "Rx-way" of doing things. To do things in the "Rx-way", you will have to embrace the usage of operators and think carefully when and how you subscribe.

A rule of thumb is to subscribe as little and as late as possible, and do minimal amount of work in subscription callbacks.

We will demonstrate this with an example. Imagine you have a stream of data to which you would like to subscribe and transform the data in some way.

```
interface IUser {
  id: number;
  user_name: string;
  name: string;
  surname: string;
}
```

`/account/me` returns JSON:

```
{
  "id": 42,
  "user_name": "TheDude",
  "name": "Jeff",
  "surname": "Lebowski"
}
```

```
// bad
const user$: Observable<IUser> = http.get('/account/me');

user$.subscribe((response: IUser) => {
  console.log(response.user_name); // TheDude
});
```

```
// good
const userName$: Observable<string> = http.get('/account/me').pipe(
  map((response: IUser) => response.user_name)
);

userName$.subscribe(console.log); // TheDude
```

The reason why is because there might be multiple subscribers who want the same thing, and they would all have to do the same transformation in their respective success callbacks.

It is, of course, possible that the subscribers want different things, and for those cases you might want to expose some observable with fewer piped operations.

The point here is that, as soon as you notice that you are repeating yourself in subscription callbacks, you should move that repeated logic into some operator that is piped to the source observable.

### Do not use observables if unnecessary <a href="#do-not-use-observables-if-unnecessary" id="do-not-use-observables-if-unnecessary"></a>

We recommend using `OnPush` change detection strategy. This means that some things that work with `Default` CD will no longer work when you start using `OnPush` CD. The best way to make things work again is to take a reactive approach when designing the component and utilize the Observables, operators and `async` pipe.

However, there are cases when you do not need an Observable. Sometimes, direct values work just as fine. Do not implement an Observable if it is not needed, it will just add some unnecessary overhead.

Here is an example where an observable is unnecessary:

```
<!-- bad -->
{{ someNumber }}! = {{ factorialOfTheNumber$ | async }}
```

```
// bad
@Component(...)
export class MyComponent implements OnChanges {
  @Input() public someNumber: number = 0;
  public readonly factorialOfTheNumber$ = new BehaviorSubject<number>(1);

  public ngOnChanges(changes: SimpleChanges): void {
    if (changes.someNumber) {
      this.factorialOfTheNumber$.next(calculateFactorial(changes.someNumber.currentValue));
    }
  }
}
```

```
<!-- good -->
{{ someNumber }}! = {{ factorialOfTheNumber }}
```

```
// good
@Component(...)
export class MyComponent implements OnChanges {
  @Input() public someNumber: number = 0;
  public readonly factorialOfTheNumber = 1;

  public ngOnChanges(changes: SimpleChanges): void {
    if (changes.someNumber) {
      this.factorialOfTheNumber = calculateFactorial(changes.someNumber.currentValue);
    }
  }
}
```

This can be a result of confusion about how `OnPush` CD works. If `factorialOfTheNumber$` is not an observable and is a direct value, it might be unclear if the template will be re-rendered or not. The answer is - yes, the template will be re-rendered correctly. While it is true that direct property assignment with `OnPush` CD can sometimes result in template not being re-rendered, that usually happens only when assigning in an asynchronous piece of code. If the `calculateFactorial` calculation is synchronous (as it is in this example), then we can assign the value directly, without the need to wrap it in an Observable.

Remember, any property assignments that are done synchronously in `ngOnChanges` will get rendered correctly. This is because CD is run synchronously after all the code in `ngOnChanges` is executed. This is true for both `OnPush` and `Default` CD.

### Consider not using ngOnChanges and use pure pipes <a href="#consider-not-using-ngonchanges-and-use-pure-pipes" id="consider-not-using-ngonchanges-and-use-pure-pipes"></a>

Building on the previous example with unnecessary Observables, we will go one step further and try to eliminate the `ngOnChanges` lifecycle hook altogether.

To do this, we have to understand how pipes work. Pipes can have multiple input values and one output value. Input values are mapped to the output value in the transformation method. Impure pipes will execute the transformation method on each change detection cycle. This is probably not optimal nor the desired behaviour in most situations. On the other hand, pure pipes will execute the transformation method only if some of the input values change. All pipes are pure by default, and that is great! You can read more about pipes [here](https://angular.io/guide/pipes).

We can not utilize the knowledge of pure pipes and update our component:

```
{{ someNumber }}! = {{ someNumber | factorial }}
```

```
@Component(...)
export class MyComponent implements OnChanges {
  @Input() public someNumber: number = 0;
}
```

```
@Pipe({ name: 'factorial' })
export class FactorialPipe implements PipeTransform {
  public transform(value: number): number {
    return calculateFactorial(value);
  }
}
```

We have greatly reduced the amount of code in our component and have created a reusable piece of code in the form of a pure pipe. This pipe can be reused in other places, but it is also ok if it is used in only this one component's template. If it is not reused in other places, it is ok to leave the pipe implementation file alongside the component module file and declare it in the component's module.

You might ask a question "But what if I need to use the factorial value inside my component code?". The solution depends on how and when you need to use the value that is calculated by the pipe. If you need to access the value on some user interaction, you can pass the calculated value from the template to the action handler method. This is great because it keeps your methods pure.

```
<ng-container *ngIf="someNumber | factorial as someNumberFactorial">
  {{ someNumber }}! = {{ someNumberFactorial }}

  <button (click)="saveFactorialValue(someNumberFactorial)">Save the factorial value</button>
</ng-container>
```

```
@Component(...)
export class MyComponent implements OnChanges {
  @Input() public someNumber: number = 0;

  public saveFactorialValue(someNumberFactorial): void {
    // do something with someNumberFactorial
  }
}
```

Some notes:

* We wrapped everything in one `ng-container` to avoid calling the pipe twice
* Be careful in case that your pipe can return a valid falsy value, as in that case the `*ngIf` will not render the content; in such case you might consider using a [custom `*ngLet`](https://github.com/ngrx-utils/ngrx-utils#nglet-directive) structural directive

### Be mindful of how and when the data is fetched <a href="#be-mindful-of-how-and-when-the-data-is-fetched" id="be-mindful-of-how-and-when-the-data-is-fetched"></a>

There are two basic approaches to data loading:

1. Via route resolve guards
2. Via container components

**Resolve guards**

A [resolve guard](https://angular.io/guide/router#resolve-pre-fetching-component-data) can be used to pre-fetch the data necessary for component rendering. The advantages of using resolve guards are:

* No need to fetch data on component initialization
  * Data is fetched during the routing event and ready when the component is initialized
* No need to implement logic for not showing the component until the data is fetched
  * When the component starts rendering, you know you will have the data
* A loader showing/hiding logic can be implemented in one place instead of in each component that loads data
  * If data is loaded via a resolve guard for all routes, you can have a global loader logic which hooks into router events, thus implementing the loaded showing/hiding logic only once

**Container components**

Even though guards are really easy to use and have some advantages, there might be situations where the data has to be loaded through multiple requests or the loading might be slow. In those cases, it might be worth to consider using a container component for data loading instead of guards. The container component would use some service to make the requests and then show the data once the requests have been completed. In this way, it is possible to show partial data as it comes in, and implement empty state design for each chunk of data that is shown.

A good example where partial data loading via a container component can be preferred over all-at-once loading via guards is a dashboard-like page with multiple graphs where each graph shows data from one request. In such cases, it is probably better to let the container component handle data loading, and presentational components (graphs) should implement an empty state with some nice loaders.

Bottom line:

* Use resolve guards if route data can be loaded in one big chunk and is also shown all-at-once.
* Use container components if data is loaded in chunks and results should be shown as they come in.

### The single observable pattern <a href="#the-single-observable-pattern" id="the-single-observable-pattern"></a>

Did you ever find yourself in the `ng-container` nesting hell? It might have looked something like this:

```
<ng-container *ngIf="frameworkName1$ | async as frameworkName1">
  <ng-container *ngIf="frameworkName2$ | async as frameworkName2">
    <ng-container *ngIf="frameworkName3$ | async as frameworkName3">
      {{ frameworkName1 }} is way better than {{ frameworkName2 }}. I will not even talk about {{ frameworkName3 }}.
    </ng-container>
  </ng-container>
</ng-container>
```

There is a simple solution to this problem, and it is called the single data observable pattern. In short, this pattern takes advantage of the reactive programming paradigm introduced with RxJS to combine multiple observable streams into one, allowing us to reduce the amount of nested subscriptions in the template. In its purest form, it allows us to have only one subscription in the template (one `async` pipe). You can also utilize this pattern to create multiple combined observables instead of having many more individual subscriptions.

The example above could be refactored to use the single data observable pattern:

```
@Component(...)
export class MyComponent {
  private readonly frameworkName1$ = ...;
  private readonly frameworkName2$ = ...;
  private readonly frameworkName3$ = ...;

  public readonly frameworkNames$ = combineLatest([
    frameworkName1$,
    frameworkName2$,
    frameworkName3$,
  ]).pipe(map(([
    frameworkName1,
    frameworkName2,
    frameworkName3,
  ]) => {
    return {
      frameworkName1,
      frameworkName2,
      frameworkName3,
    }
  }))
}
```

```
<ng-container *ngIf="frameworkNames$ | async as frameworkNames">
  {{ frameworkNames.frameworkName1 }} is way better than {{ frameworkNames.frameworkName2 }}. I will not even talk about {{ frameworkNames.frameworkName3 }}.
</ng-container>
```
