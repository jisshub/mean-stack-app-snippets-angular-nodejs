# Notes

- App Component is registered with angular.
  we can now see this App Component in other component.

![image](./screenshots/screen-1.jpg 'image')
**app.module.ts**

```typescript
  declarations: [
    AppComponent
  ],
```

- code in main.ts file is executed at first.

**main.ts**

```typescript
platformBrowserDynamic()
  .bootstrapModule(AppModule)
  .catch((err) => console.error(err));
```

---

## understanding angular component

![image](./screenshots/screen-2.jpg 'image')

- we can have different components in the app
  eg: app-header, app-brand, app-feature ..
  can have another components in app-header like app-nav-item, app-nav-search.

- in this app, we have to create, edit, delete post.

## adding new component

```bash
ng g c posts/post-create
```

- above command will create a _posts_ folder - then add _post-create_ component with in the _posts_ folder.

  final output will be like this:

![image](./screenshots/screen-3.jpg 'image')

- here v create _post-create_ component in posts folder.

- add contents in post-create template file

- add the post-create selector in app component.html

```html
<h1>Our First App</h1>
<app-post-create></app-post-create>
```

- thus the content in post-create component will be rendered in app component template.

---

## Listening to events - event binding

> event binding means a feature that allows angular to listen to events made by users.
> here v add input and clicking on button angular listen to that click event and execute some function there.

**post-create-component.html**

```html
<textarea cols="30" rows="10"></textarea>
<hr>
<button (click)="onAddPost()">Submit</button>
```

**post-create-component.ts**

```typescript
 onAddPost(){
    alert("post added");
  }
```

- binding click event on button. while click, the assigned method is fired.

---

## outputting contents - property binding

- outputting content user entered
- can use string interpolation or property binding
- in string interpolation, gives the value to be outputted in double curly braces.
- in property binding, binds the value to be outputted to the value property of html element.
  Eg:

### using only string interpolation

**post-create-component.html**
```html
<textarea cols="30" rows="10"></textarea>
<hr>
<button (click)="onAddPost()">Submit</button>
<p>{{firstPost}}</p>
```

**post-create-component.ts**
```ts
onAddPost(){
  this.firstPost = 'The user\'s post' ;
}
```

### using property binding

**post-create-component.html**

```html
<textarea cols="30" rows="10" [value]="firstPost"></textarea>
<hr>
<button (click)="onAddPost()">Submit</button>
<p>{{firstPost}}</p>
```
- bind firstPost property to value attribute of textarea element,

```ts
export class PostsCreateComponent implements OnInit {

  constructor() { }
  firstPost = 'No Content';
  ngOnInit(): void {
  }
onAddPost(){
  this.firstPost = 'The user\'s post' ;
}
}
```
- here v prepopulate the textrea with a value using property binding. on clicking on button
cause value to be changed.

---

## getting user input

- accessing the element - getting the value.

- first set a reference to the element.

```html
<textarea
  name="post"
  id="post"
  cols="30"
  rows="5"
  [value]="newPost"
  #postInput
></textarea
><br /><br />
```

_postInput_ is the reference to the textArea element.

- other elements can access this element using postInput reference.

```html
<button (click)="onAddPost(postInput)">Add Post</button>
```

- pass the reference as an argument to _onAddPost_ method.

```typescript
onAddPost(postInput: HTMLInputElement){
    this.newPost = postInput.value;
  }
```

- _onAddPost_ method receive the element sent as _argument_.
- access the _value_ property of the same.

---

## getting and outputting data at same time - two way binding

- use ngModel directive - assign a property.

**post-create.compoentn.html**
```html
<textarea cols="30" rows="10" [(ngModel)]="postInput"></textarea>
<hr>
<button (click)="onAddPost()">Submit</button>
<p>{{firstPost}}</p>


```

> here v get the value to _textArea_ element and output the value from _textArea_ at same time. earlier v do property binding to output the value. and do a local reference to get the value. by using ngModel directive, v get the value from user and output the value on template at same time.

**post-create.compoentn.ts**

```typescript
export class PostsCreateComponent implements OnInit {

  constructor() { }
  postInput: string = '';
  firstPost: string = '';
  ngOnInit(): void {
  }
onAddPost(){
  this.firstPost = this.postInput;
}
}

```
- Error: Can't bind to 'ngModel' since it isn't a known property of 'input'

Solution:
------------
Add **FormsModule** to the _imports_ array in _app module_.

---

## installing angular material

### create a form using angular material

(Angular Material)[https://material.angular.io/guide/getting-started]

(Angular Component)[https://material.angular.io/components/categories]

```html
<div class="container">
  <mat-card>
    <h3>Add Posts</h3>
    <mat-form-field class="example-form-field">
      <textarea
        rows="5"
        matInput
        [(ngModel)]="postValue"
      ></textarea></mat-form-field
    ><br />
    <button mat-raised-button color="primary" (click)="onAddPost()">
      Add Post
    </button>
    <br />

    <p>{{ newPost }}</p>
  </mat-card>
</div>
```

**app.module.ts**

```typescript
import { MatInputModule } from '@angular/material/input';
import { MatButtonModule } from '@angular/material/button';
import { MatCardModule } from '@angular/material/card';

imports: [MatInputModule, MatButtonModule, MatCardModule];
```

---

## adding toolbar

> to add header to the page.
> create header component in post component.

```bash
ng g component posts/header --skipTests=true
```

> import MatToolbar Module in app module
> add same in imports array.

(Angular Toolbar)[https://material.angular.io/components/toolbar/overview]

**header.component.html**

```html
<p>
  <mat-toolbar color="primary">
    <span>My Application</span>
  </mat-toolbar>
</p>
```

- update the app component template to use header component.

**app.component.html**

```html
<app-header></app-header>
<app-posts-create></app-posts-create>

```

---

## outputting posts using Expansion Panel Component.

> create post-list component in posts component

```bash
ng g component posts.post-list --skipTests=true
```

> add post-list in component app component template
> using _Expansion panel_ to output the post.
> import MatExpansion Module -add to imports array.

### populating the expansion panel with contents

#### structural directive- ngIf, ngFor

- declare an array,

```typescript
posts: Array<Object> = [];
```

```html
<mat-accordion *ngIf="posts.length > 0">
  <mat-expansion-panel hideToggle *ngFor="let post of posts">
    <mat-expansion-panel-header>
      <mat-panel-title> {{post.title}} </mat-panel-title>
    </mat-expansion-panel-header>
    <p>{{post.content}}</p>
  </mat-expansion-panel>
</mat-accordion>
<p class="mat-body-1 info-text" *ngIf="posts.length <= 0">No posts added yet</p>
```

---

## creating posts with property and event binding.

- attach ngModel directive for the inputs for two-way binding.
- so here v get and output the value at same time
  **post-create.component.html**

```html
<mat-form-field class="example-full-width">
  <input
    matInput
    type="text"
    placeholder="Title goes Here"
    [(ngModel)]="postTitle"
  />
</mat-form-field>
<mat-form-field class="example-form-field">
  <textarea
    rows="5"
    matInput
    [(ngModel)]="postContent"
    placeholder="content goes here"
  ></textarea></mat-form-field
><br />
```

**post-create.component.ts**

```typescript
constructor() { }
  postTitle: string ="";
  postContent: string ="";
  ngOnInit(): void {
  }
  onAddPost(){
    // create post object to store title and content
    const post = {title: this.postTitle, content: this.postContent};
  }
```

> here v got the values - next need to output those,
> first need to emit an event - create an event emitter
> also need to listen to this event from other components. for that add **Output** decorator to the event.
> thus v can listen it from the parent component.

```typescript
@Output() postEvent = new EventEmitter();
  ngOnInit(): void {
  }
  onAddPost(){
    // create post object to store title and content
    const post = {title: this.postTitle, content: this.postContent};
    // emit the post object
    this.postEvent.emit(post);
  }
```

**app.component.html**

> access the event emitted in app component.
> here _\$event_ argument receives the data emitted

```html
<app-post-create (postEvent)="onPostAdded($event)"></app-post-create>
```

**app.component.ts**

> push the post to posts array

```typescript
 // empty array
  storedPosts:Array<Object> = [];
  // define function, get post
  onPostAdded(post: Object){
    // push the received post object to posts Array
    this.posts.push(post);
  }
```

> next v have to pass the posts Array to post list component.

> for that make post property in post-list component bindable from outside thru **property binding**.

> for that v use _Input decorator_,

**post-list.component.ts**

```typescript
  // make posts property bindable from outside thru property binding
  @Input() posts: Array<Object> = [];

```

**app.component.html**

```html
<!--property binding of posts property on storedPosts array-->
<app-post-list [posts]="storedPosts "></app-post-list>
```

**app.component.ts**

```typescript
  storedPosts:Array<Object> = [];
  // define function, get post
  onPostAdded(post: Object){
    // push the received post object to posts Array
    this.storedPosts.push(post);
  }
```

---

## Creating Post Model

- create post model in posts folder.

```bash
ng generate class posts/post --type=model
```

- later declare fields in the model.

```typescript
export class Post {
  title: string;
  content: string;
}
```

- now v can use this **Post class** in components.

**app.component.ts**

- change type to Post class.
```typescript
  // empty array
  postToStore: Post[] = [];
  onPostEmitted(postRec: Post){
      this.postToStore.push(postRec);
  }
```

- storedArray is an array of type Post.

**post-create.component.ts**

```typescript
@Output() postEvent = new EventEmitter<Post>();
onAddPost(){
    // create post object to store title and content
    const post: Post = {title: this.postTitle, content: this.postContent};
    // emit the post object
    this.postEvent.emit(post);
  }
```

- data v emit is of type **Post** by setting generic type to event emitter.
- _post_ object is of type **Post** class.

---

**post-list.component.ts**

```typescript
@Input() posts: Post[] = [];

```

- here posts property accept only Post array type data.

**post-create.component.ts**
```typescript
 onAddPost(){
    const postObj: Post = {title: this.postTitle, content: this.postContent};
    this.postEvent.emit(postObj);
  }
```

---

## adding forms

> add _ngModel_ directive to the each inputs.
> thus it register the inputs as a control to the forms.
> but have to add _name_ attribute for the inputs since angular needs to know how to name the input.
> change button type to submit. while submitting, it triggers an event _submit_ that v can listen to.
> add _submit_ event to the form. assign the method to it.

```html
<form (submit)="onAddPost()">
  <mat-form-field>
    <input
      matInput
      type="text"
      placeholder="Title goes Here"
      ngModel
      name="title"
    />
  </mat-form-field>
  <mat-form-field>
    <textarea
      rows="5"
      matInput
      ngModel
      placeholder="content goes here"
      name="content"
    ></textarea>
  </mat-form-field>
  <br />
  <button mat-raised-button color="primary" type="submit">Add Post</button>
  <br />
</form>
```

- now need to get access to the form and the values in the form.
  > For that purpose, v set a **local reference** in the form element- assign ngForm directive to the **local reference** as a string - pass that **local reference** as an argument to the method.
  - by doing this, v get access to form object and its values.
  
  **post-create.component.html**
  ```html
  <form (submit)="onAddPost(postForm)" #postForm="ngForm">
  ```
  
  **post-create.component.ts**
  ```ts
  
    onAddPost(form: NgForm){
    const postObj: Post = {title: this.postTitle, content: this.postContent};
    this.postEvent.emit(postObj);
  }
  ```
  - here *onAddPost* method receive a form object of type NgForm.

---

## adding validations to the form

- use _required_ attribute on each inputs.
- can also use _min-length_ attribute as well.
- displaying error message using _mat-error_ component of material.

**post-create.component.html**

```html
<input
  matInput
  type="text"
  placeholder="Title goes Here"
  ngModel
  name="title"
  required
  minlength="5"
  #title="ngModel"
/>
<mat-error *ngIf="title.invalid">Please give valid title</mat-error>
```

- give both the inputs a _local reference_. thus can use in the _ngIf_.

- this form creation is done by _template driven approach_.

---

## getting post from post-create component to post-list component

- here v use _services_.
- create a service class in posts folder.

```bash
ng generate service posts/post --skipTests=true
```

> create a posts array of type Post.
> get posts and later add posts to the post array.

**post.service.ts**

```typescript
export class PostService {
  constructor() {}
  // create a private property of type Post array
  private posts: Post[] = [];
  getPosts() {
    // return copy of post array, use spread operator, so changes only affected on its copy, not the original array
    return [...this.posts];
  }
  // adding new post
  addPost(post: Post) {
    this.posts.push(post);
  }
}
```

- next v have to inject the instance of this service class to needed component. called dependency injection

- for that add a constructor in component > define an a public property of type **PostService** type.

- that property will store incoming value of PostService.

---

## Calling GET post

> add OnInIt interface on component.
> later define _ngOnInIt_ method - call the _getPosts_ method on postService property.


**post.service.ts**

```ts
export class PostService {

  constructor() { }
  private allPosts: Post[] = [];

  getPosts(){
    return [...this.allPosts];
  }
  addPosts(title: string, content: string){
    const post: Post = {title, content} ;
    this.allPosts.push(post);
  }
}

```
**post-list.component.ts**

```typescript  
  constructor(private postService: PostService) { }
  getPostsArray: Post[] = [];
  ngOnInit(): void {
    this.getPostsArray = this.postService.getPosts();
  }

```

also do the same in **post-create component** for creating post as well.

- inject _service_ to the constructor.

**post-create.component.ts**
```typescript
constructor(private postService: PostService) { }
  postTitle = '';
  firstPost = '';
  postContent = '';
  ngOnInit(): void {
  }
  onAddPost(form: NgForm){
    if (form.invalid) {
      return;
    }
    this.postService.addPosts(form.value.title, form.value.content);
  }
  ```

> here the after entering inputs posts wont be listed,
> since v fetched the post before adding them.
> to solve it, first remove the bindings in app component.

**app.component.html**
```html
<app-header></app-header>
<main>
    <app-posts-create></app-posts-create>
    <app-post-list></app-post-list>
</main>


```
> next use rxjs package.
> import **Subject** from rxjs package.
> create new instance of Subject.
> call next() on that subject instance - pass the copy of post array.

**post.service.ts**

```typescript
private postUpdated = new Subject<Post[]>();

  // adding new post
  addPost(post: Post){
    this.posts.push(post);
    // call next() on subject - pass copy of posts array as argument.
    this.postUpdated.next([...this.posts]);
  }
```

> next u have to listen to that subject whenever it emits a post.
> create new method - call _asObservable()_ on subject instance and then return it.
> it will return an object that v can listen to.

```typescript

  getPostUpdateListener(){
    // call asObservable method on postUpdated. which in turn returns an object.
    return this.postUpdated.asObservable();
  }
```

> next v have to subscribe to t his listener object from post-list component call **subscribe()**.
> subscribe takes a callback  function as an argument which will b called when new data is emitted.

**post-list.component.ts**

```typescript
  ngOnInit(): void {
    // call get post method here
    this.posts = this.postService.getPosts();
    this.postService.getPostUpdateListener()
    .subscribe((post: Post[]) => {
      // set posts property with post array received.
      this.posts = post;
    });

  }
```

> next have to store the subscription in a property.
> finally unsubscribe the subscription. implement _OnDestroy_ method.

**post-list.component.ts**

```typescript

  private postSub: Subscription;
  ngOnInit(): void {
    // call get post method here
    this.posts = this.postService.getPosts();
    this.postSub = this.postService.getPostUpdateListener()
    .subscribe((post: Post[]) => {
      // set posts property with post array received.
      this.posts = post;
    });
  }
  ngOnDestroy(){
    // unsubscribe
      this.postSub.unsubscribe()
  }

```

---

## Observables, Observers, Subscriptions

> observer subscribes to the observable, simply means they establishes the subscription.
> 3 methods in observer,

1. next()
2. error()
3. complete()

> v call next() on subject, subject is an observable.
> v invoke error() on observable when needed to throw some error.
> can call complete() on observable when no more data left to be emitted.

**Screenshot**
![Image](./screenshots/screen-4.jpg)

### Example

- An observable wraps a callback of http request.
- wraps an _http request_ with in an observable
  that takes that callback and whenever the request gives us the response, v use the observable to emit response data by invoking _next()_. or error message if error pops up. v can also complete the observable once got response by calling _complete()_. this how v manage _http request_ in observable.

**Screenshot**
![Image](./screenshots/screen-5.jpg)

### Subject

> a _subject_ is a kind of observable.
> normal observable is passive. eg: wraps callback, emitting events
> subject is active observable. v also subcribe to the subject, but here v call _next()_ manually.
> for example, when v add a new post, v actively can notify our entire application. it can be done with _subject_.

**Screenshot**
![Image](./screenshots/screen-6.jpg)

> consider observables/subjects as a stream of values/data.
> and these values r emitted over time.

> observer - have set of functions we can call,

1. next() - call next() for emitting a normal value, also call if v hav an observable that wraps an http request.
2. error()
3. complete() - call complete() when an observable ends emitting values.

> this is how observables work.

---

## working on forms

- resetting the form on submit.
- adding edit and delete button on expansion panel. use **Action Bar** in expansion panel.

---


