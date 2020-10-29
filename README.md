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
<button (click)="onAddPost()">Save post</button>
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

```html
<textarea name="post" id="post" cols="30" rows="5" [value]="newPost"></textarea>
<p>{{ newPost }}</p>
```

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

```html
<textarea
  name="post"
  id="post"
  cols="30"
  rows="5"
  [(ngModel)]="postValue"
></textarea
><br /><br />
```

> here v get the value to _textArea_ element and output the value from _textArea_ at same time. earlier v do property binding to output the value. and do a local reference to get the value. by using ngModel directive, v get the value from user and output the value on template at same time.

```typescript
  postValue: string ="";
  ngOnInit(): void {
  }
  onAddPost(){
    this.newPost = this.postValue;
  }
```

- declare _postValue_ property here, assign it to _newPost_.

- finally add **FormsModule** to the _imports_ array in _app module_.

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

```typescript
  // empty array
  storedPosts:Post[] = [];
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

- now need to get access to values in the form.
  > For that purpose, v set a **local reference** in the form element- assign ngForm directive to the **local reference** as string - pass that **local reference** as an argument to the method.
  - by doing this, v get access to form object and its values.

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

**post-list.component.ts**

```typescript
  ngOnInit(): void {
    // call get post method here
    this.postService.getPosts();
}
```

also do the same in **post-create component** for creating post as well.

- inject _service_ to the constructor.

```typescript
constructor(public postService: PostService) { }
  onAddPost(form: NgForm){
    // create post object to store title and content
    const post: Post = {title: form.value.title, content: form.value.postContent};
    // emit the post object
    this.postService.addPost(post);
  }
```

> here the after entering inputs posts wont be listed,
> since v fetched the post before adding them.
> to solve it, first remove the bindings in app component.
> next use rxjs package.
> import **Subject** from rxjs package.
> create new instance of Subject.
> call next on that subject instance - pass the post array copy.

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
> create new method - call _asObservable()_ on subject and return it.
> it will return an object that v can listen to.

```typescript

  getPostUpdateListener(){
    // call asObservable method on postUpdated. which in turn returns an object.
    return this.postUpdated.asObservable();
  }
```

> next v have to subscribe to this listener object from post-list component call **subscribe()**.
> subscribe takes a function as an argument which will b called when new data is emitted.

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
